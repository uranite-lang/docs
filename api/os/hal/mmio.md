# uranite.os.hal.mmio

## Table of Contents

- [Imports](#imports)
- [function `mmioRead8`](#function-mmioread8)
  - [`mmioRead8()`](#mmioRead8)
- [function `mmioWrite8`](#function-mmiowrite8)
  - [`mmioWrite8()`](#mmioWrite8)
- [function `mmioRead16`](#function-mmioread16)
  - [`mmioRead16()`](#mmioRead16)
- [function `mmioWrite16`](#function-mmiowrite16)
  - [`mmioWrite16()`](#mmioWrite16)
- [function `mmioRead32`](#function-mmioread32)
  - [`mmioRead32()`](#mmioRead32)
- [function `mmioWrite32`](#function-mmiowrite32)
  - [`mmioWrite32()`](#mmioWrite32)
- [function `mmioRead64`](#function-mmioread64)
  - [`mmioRead64()`](#mmioRead64)
- [function `mmioWrite64`](#function-mmiowrite64)
  - [`mmioWrite64()`](#mmioWrite64)
- [function `memoryFence`](#function-memoryfence)
  - [`memoryFence()`](#memoryFence)
- [function `storeFence`](#function-storefence)
  - [`storeFence()`](#storeFence)
- [function `loadFence`](#function-loadfence)
  - [`loadFence()`](#loadFence)

## Imports

- `uranite.memory.memory`
  - `Memory`

## function `mmioRead8`

Read an 8-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

### Methods

#### `function mmioRead8( I64 address ) -> I64`

Read an 8-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

## function `mmioWrite8`

Write an 8-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

### Methods

#### `function mmioWrite8( I64 address, I64 value ) -> Void`

Write an 8-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

## function `mmioRead16`

Read a 16-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

### Methods

#### `function mmioRead16( I64 address ) -> I64`

Read a 16-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

## function `mmioWrite16`

Write a 16-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

### Methods

#### `function mmioWrite16( I64 address, I64 value ) -> Void`

Write a 16-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

## function `mmioRead32`

Read a 32-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

### Methods

#### `function mmioRead32( I64 address ) -> I64`

Read a 32-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

## function `mmioWrite32`

Write a 32-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

### Methods

#### `function mmioWrite32( I64 address, I64 value ) -> Void`

Write a 32-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

## function `mmioRead64`

Read a 64-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

### Methods

#### `function mmioRead64( I64 address ) -> I64`

Read a 64-bit value from the given memory-mapped I/O address using a volatile load instruction. The memory clobber ensures the compiler does not reorder or eliminate this read.

## function `mmioWrite64`

Write a 64-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

### Methods

#### `function mmioWrite64( I64 address, I64 value ) -> Void`

Write a 64-bit value to the given memory-mapped I/O address using a volatile store instruction. The memory clobber ensures the compiler does not reorder or eliminate this write.

## function `memoryFence`

Execute a full memory barrier to ensure all prior memory reads and writes are completed before any subsequent memory operations. This orders both loads and stores.

### Methods

#### `function memoryFence(  ) -> Void`

Execute a full memory barrier to ensure all prior memory reads and writes are completed before any subsequent memory operations. This orders both loads and stores.

## function `storeFence`

Execute a store memory barrier to ensure all prior store operations are globally visible before any subsequent stores. This is a write-only memory barrier.

### Methods

#### `function storeFence(  ) -> Void`

Execute a store memory barrier to ensure all prior store operations are globally visible before any subsequent stores. This is a write-only memory barrier.

## function `loadFence`

Execute a load memory barrier to ensure all prior load operations have completed before any subsequent loads. This is a read-only memory barrier.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function loadFence(  ) -> Void`

Execute a load memory barrier to ensure all prior load operations have completed before any subsequent loads. This is a read-only memory barrier.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

