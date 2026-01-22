---
title: Submanifold Convolution [part 2] - Hash Table
description: A hash table on the GPU.
date: '2026-01-22'
categories:
    - deep-learning
    - sparse-convolution
published: true
---

This is the second part of my ? part series on the implementation of submanifold convolution. Here, we will discuss about the implementation of a hash table on the GPU. The entire codebase can be found [here](https://github.com/Arktetra/sparse-conv).

Before we continue, let us set some notations.

![Sparse Tensor Representation](spconv/sparse-tensor-representation.png)

Let $\mathcal{F} \in \mathbb{R}^{W \times H \times C_{in}}$ be the original feature map, $\mathcal{C}^{(s)} \in \mathbb{Z}^{M \times d}$ be the coords tensor in the sparse representation, and $\mathcal{F}^{(s)} \in \mathbb{R}^{M \times C_{in}}$ be the features tensor in the sparse representation. Here, $W$, $H$, and $C_{in}$ are the width, height, and input channels. Also, $M$ is the number of active sites, and $d$ is the spatial dimension of the original feature map, which in our case is $2$ (the width and the height).

## Hash Table

A **hash table** is a data structure that stores items as unique key-value pairs. We require a hash table to efficiently fetch the neighbors of $\mathcal{F}^{(s)}_{i, \cdot}$ when applying the convolution algorithm. With a little contemplation, we can observe that for $\mathcal{C}^{(s)}_{i, \cdot}$, the row index $i$ is the value, but what do use as the keys?

With some mathematics, we can find the neighbors of $\mathcal{C}^{(s)}_{i, \cdot}$ by itself (without converting it to any other form) with combination of the spatial dimensions of the kernel. However, querying the neighbors in such way clearly has computational overheads, so what we use instead is flattened indices in the original map, $\mathcal{F}$, as the keys:

![Hash Map](spconv/hashmap.png)

The flattened index correponding to $\mathcal{C}^{(s)}_{i, \cdot}$, can be easily obtained from a combination of $\mathcal{C}^{(s)}_{i, \cdot}$, and the spatial dimensions. For example, if we have only two dimensions, $W$ and $H$, the flattened index can be obtained as,
$$
\text{flat-idx} = \mathcal{C}^{(s)}_{i, 0} * W + \mathcal{C}^{(s)}_{i, 1}
$$

Thus, we will use $\text{flat-idx}$ as the keys and the corresponding indices in $\mathcal{C}^{(s)}$ as values. We will be implementing this hash table in a GPU.

### Why GPU?

In practical applications, there can be large number of elements in the sparse tensor. If we want to create a hash table in a CPU, then the insertions and lookups need to be performed sequentially (assuming that no parallel processing is done). However, if we use a GPU for this purpose, then we can perform insertions and lookups in bulk, which increases efficiency.

### The Hash Function

As we know, we require a hash function to compute the slot in the hash table from the key. This slot will hold both the key and the value.

For our implementation, we will follow the references and use the "finalize" part of the MurmurHash3:

```c++
// 32 bit Murmur3 hash
__forceinline__ __device__ size_t hash(uint32_t k, size_t N) {
    k ^= k >> 16;
    k *= 0x85ebca6b;
    k ^= k >> 13;
    k *= 0xc2b2ae35;
    k ^= k >> 16;
    return k % N;
}


// 64 bit Murmur3 hash
__forceinline__ __device__ size_t hash(uint64_t k, size_t N) {
    k ^= k >> 33;
    k *= 0xff51afd7ed558ccdULL;
    k ^= k >> 33;
    k *= 0xc4ceb9fe1a85ec53ULL;
    k ^= k >> 33;
    return k % N;
}
```

### Initialization

We assume the following factors about the initialization of the hash table:

1. All keys are initialized to the maximum value that the data type of the key can represent.
2. All values are initialized to be empty.
3. The size of the hash table is given by $N$.

### Insertion

The code to insert the key-value pairs in the hash table is as follows:

```c++
template<typename V>
__forceinline__ __device__ void linear_probing_insert(
    uint64_t* hashmap_keys,
    V* hashmap_values,
    const uint64_t key,
    const V value,
    const size_t N
) {
    size_t slot = hash(key, N);
    while (true) {
        uint64_t prev = atomicCAS(
            reinterpret_cast<unsigned long long*>(&hashmap_keys[slot]),
            static_cast<unsigned long long>(std::numeric_limits<uint64_t>::max()),
            static_cast<unsigned long long>(key)
        );
        if (prev == std::numeric_limits<uint64_t>::max() || prev == key) {
            hashmap_values[slot] = value;
            return;
        }
        slot = (slot + 1) % N;
    }
}
```

In the above code, we do the following:

1. We compute a slot in the hash table from the key, $A$, using the hash function.
2. We perform atomic compare-and-swap operation i.e. 
    1. we compare the existing key in the slot, $B$, with the maximum value, $V$, and 
    2. if it matches then we replace the value with the key, and return the replaced value, $B$,
    3. otherwise we just return the maximum value, $V$.
3. If the result from the atomic compare-and-swap operation matches the key or the maximum value, then we insert the value in that slot, otherwise we repeat from step 2 after performing linear probing.

### Lookup

The code to lookup the value at a key in the hast table is as follows:

```c++
template<typename K, typename V>
__forceinline__ __device__ V linear_probing_lookup(
    const K* hashmap_keys,
    const V* hashmap_values,
    const K key,
    const size_t N
) {
    size_t slot = hash(key, N);
    while (true) {
        K prev = hashmap_keys[slot];

        if (prev == std::numeric_limits<K>::max()) {
            return std::numeric_limits<V>::max();
        }

        if (prev == key) {
            return hashmap_values[slot];
        }
        
        slot = slot + 1;
        if (slot >= N) slot = 0;
    }
}
```

In the above code, we do the following:

1. We compute the slot in the hash table for the key, $A$, using the hash function.
2. We compare the key at the slot, $B$, with the maximum value, $V$, and if it matches then we return $V$.
3. Otherwise, we compare $A$ with $B$, and if it matches then we return the value at the slot.
4. Otherwise, we increment the slot, and repeat from step 2. Note that we set the slot to 0 if it exceeds the size of the hash table.

### Inserting 2D Index as Value

The kernel for inserting the $i^{th}$ row index in $\mathcal{C}^{(s)}$ as value in the hashmap is as follows:

```c++
template<typename K, typename V>
static __global__ void hashmap_insert_2d_idx_as_val_cuda_kernel(
    const size_t N,
    const size_t M,
    int W, int H,
    K* __restrict__ hashmap_keys,
    V* __restrict__ hashmap_values,
    const int32_t* __restrict__ keys
) {
    size_t thread_id = blockIdx.x * blockDim.x + threadIdx.x;

    if (thread_id < M) {
        int3 coord = reinterpret_cast<const int3*>(keys)[thread_id];
        int b = coord.x;    // batch
        int x = coord.y;
        int y = coord.z;

        size_t flat_idx = (size_t)b * W * H + (size_t)x * H + y;
        K key = static_cast<K>(flat_idx);
        V value = static_cast<V>(thread_id);
        linear_probing_insert(hashmap_keys, hashmap_values, key, value, N);
    }
}
```

In the above code, we do the following:

1. Compute the `thread_id`, which is corresponding to $i^{th}$ row index in $\mathcal{C}^{(s)}$.
2. Use the `thread_id` to obtain $\mathcal{C}^{(s)}_{i, \cdot}$.
3. Obtain $\text{flat-idx}$ from $\mathcal{C}^{(s)}_{i, \cdot}$.
4. Insert $<\text{flat-idx, } i>$ as row-value pair in the hash table.

### Inserting 3D Index as Value

The above code works for feature maps with two spatial dimensions (width and height), but it can be easily extended to feature maps with three spatial dimensions (width, height and depth) as follows:

```c++
template<typename K, typename V>
static __global__ void hashmap_insert_3d_idx_as_val_cuda_kernel(
    const size_t N,
    const size_t M,
    int W, int H, int D,
    K* __restrict__ hashmap_keys,
    V* __restrict__ hashmap_values,
    const int32_t* __restrict__ keys
) {
    const size_t thread_id = blockIdx.x * blockDim.x + threadIdx.x;

    if (thread_id < M) {
        int4 coord = reinterpret_cast<const int4*>(keys)[thread_id];
        int b = coord.x;    // batch
        int x = coord.y;
        int y = coord.z;
        int z = coord.w;

        size_t flat_idx = (size_t)b * W * H * D + (size_t)x * H * D + (size_t)y * D + z;
        K key = static_cast<K>(flat_idx);
        V value = static_cast<V>(thread_id);
        linear_probing_insert(hashmap_keys, hashmap_values, key, value, N);
    }
}
```

## References

- [A Simple GPU Hash Table](https://nosferalatu.com/SimpleGPUHashTable.html)
- [MurmurHash](https://en.wikipedia.org/wiki/MurmurHash)
- [FlexGEMM: A Cross-Platform Backend for High-Performance Sparse Convolutions](https://jeffreyxiang.github.io/en/blogs/flexgemm)