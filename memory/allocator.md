# uranite.memory.allocator

## Table of Contents

- [Imports](#imports)
- [const `PROT_READ`](#const-prot-read)
- [const `PROT_WRITE`](#const-prot-write)
- [const `PROT_RW`](#const-prot-rw)
- [const `MAP_PRIVATE`](#const-map-private)
- [const `MAP_ANONYMOUS`](#const-map-anonymous)
- [const `MAP_PRIVATE_ANON`](#const-map-private-anon)
- [const `HEADER_SIZE`](#const-header-size)
- [const `PAGE_SIZE`](#const-page-size)
- [class `AllocationError`](#class-allocationerror)
  - [`AllocationError()`](#AllocationError)
- [function `alignToPage`](#function-aligntopage)
  - [`alignToPage()`](#alignToPage)
- [function `alloc`](#function-alloc)
  - [`alloc()`](#alloc)
- [function `dealloc`](#function-dealloc)
  - [`dealloc()`](#dealloc)
- [function `realloc`](#function-realloc)
  - [`realloc()`](#realloc)
- [function `allocZeroed`](#function-alloczeroed)
  - [`allocZeroed()`](#allocZeroed)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.io.syscall`
  - `readByteAt`
  - `readI64At`
  - `writeByteAt`
  - `writeI64At`
- `uranite.os.arch.native.syscall`
  - `SYS_MMAP`
  - `SYS_MUNMAP`
- `uranite.os.syscall.invoke`
  - `syscall2`
  - `syscall6`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `PROT_READ`

Memory protection flag: page can be read. 

## const `PROT_WRITE`

Memory protection flag: page can be written. 

## const `PROT_RW`

Memory protection flags: page can be read and written. 

## const `MAP_PRIVATE`

Mapping flag: changes are private (copy-on-write). 

## const `MAP_ANONYMOUS`

Mapping flag: not backed by any file. 

## const `MAP_PRIVATE_ANON`

Combined MAP_PRIVATE and MAP_ANONYMOUS flags. 

## const `HEADER_SIZE`

Size in bytes of the allocation header that stores the total mapped region size. 

## const `PAGE_SIZE`

System page size in bytes used for aligning mmap allocations. 

## class `AllocationError`

**Extends**: `Error`

Raised when a memory allocation or deallocation operation fails, typically due to an mmap or munmap syscall returning an error.

### Methods

#### `function AllocationError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new AllocationError.

**Parameters**:

- `message` (`String`)
- `A description of the allocation failure.`
- `code` (`I64`)
- `The syscall error code returned by the kernel.`
- `cause` (`?Error`)
- `An optional underlying error, or None.`

## function `alignToPage`

Round a byte size up to the nearest multiple of the system page size (4096 bytes). Returns the size unchanged if it is already page-aligned.

**Parameters**:

- `size` (`I64`)
- `The byte size to align.`

**Returns**: — The smallest multiple of PAGE_SIZE that is greater than or
equal to size.

### Methods

#### `function alignToPage( I64 size ) -> I64`

Round a byte size up to the nearest multiple of the system page size (4096 bytes). Returns the size unchanged if it is already page-aligned.

**Parameters**:

- `size` (`I64`)
- `The byte size to align.`

**Returns**: — The smallest multiple of PAGE_SIZE that is greater than or
equal to size.

## function `alloc`

Allocate at least the requested number of bytes using mmap. An 8-byte header is prepended to store the total mapped region size, enabling dealloc and realloc to operate without external metadata. The returned pointer points past the header to the usable region.

**Parameters**:

- `size` (`I64`)
- `The minimum number of usable bytes to allocate.`

**Returns**: — A pointer (as I64) to the start of the usable memory region.

**Raises**:

- `AllocationError` → `Error` — If the mmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function alloc( I64 size ) -> I64`

Allocate at least the requested number of bytes using mmap. An 8-byte header is prepended to store the total mapped region size, enabling dealloc and realloc to operate without external metadata. The returned pointer points past the header to the usable region.

**Parameters**:

- `size` (`I64`)
- `The minimum number of usable bytes to allocate.`

**Returns**: — A pointer (as I64) to the start of the usable memory region.

**Raises**:

- `AllocationError` → `Error` — If the mmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `dealloc`

Free a previously allocated memory region by calling munmap. The pointer must have been returned by alloc, realloc, or allocZeroed. Reads the allocation header to determine the mapped region size.

**Parameters**:

- `ptr` (`I64`)
- `The pointer` (`as I64`)

**Raises**:

- `AllocationError` → `Error` — If the munmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function dealloc( I64 ptr ) -> Void`

Free a previously allocated memory region by calling munmap. The pointer must have been returned by alloc, realloc, or allocZeroed. Reads the allocation header to determine the mapped region size.

**Parameters**:

- `ptr` (`I64`)
- `The pointer` (`as I64`)

**Raises**:

- `AllocationError` → `Error` — If the munmap syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `realloc`

Resize a previously allocated memory region. If the new size maps to the same page-aligned total, the original pointer is returned unchanged. Otherwise, a new region is allocated, the existing data is copied byte-by-byte, and the old region is freed.

**Parameters**:

- `ptr` (`I64`)
- `The pointer` (`as I64`)
- `newSize` (`I64`)
- `The desired new usable size in bytes.`

**Returns**: — A pointer to the resized memory region, which may differ
from the original pointer.

**Raises**:

- `AllocationError` → `Error` — If the underlying alloc or dealloc fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function realloc( I64 ptr, I64 newSize ) -> I64`

Resize a previously allocated memory region. If the new size maps to the same page-aligned total, the original pointer is returned unchanged. Otherwise, a new region is allocated, the existing data is copied byte-by-byte, and the old region is freed.

**Parameters**:

- `ptr` (`I64`)
- `The pointer` (`as I64`)
- `newSize` (`I64`)
- `The desired new usable size in bytes.`

**Returns**: — A pointer to the resized memory region, which may differ
from the original pointer.

**Raises**:

- `AllocationError` → `Error` — If the underlying alloc or dealloc fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `allocZeroed`

Allocate memory and zero-initialize all bytes. Equivalent to calling alloc followed by filling every byte with zero.

**Parameters**:

- `size` (`I64`)
- `The number of usable bytes to allocate and zero.`

**Returns**: — A pointer (as I64) to the zero-initialized memory region.

**Raises**:

- `AllocationError` → `Error` — If the underlying alloc fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function allocZeroed( I64 size ) -> I64`

Allocate memory and zero-initialize all bytes. Equivalent to calling alloc followed by filling every byte with zero.

**Parameters**:

- `size` (`I64`)
- `The number of usable bytes to allocate and zero.`

**Returns**: — A pointer (as I64) to the zero-initialized memory region.

**Raises**:

- `AllocationError` → `Error` — If the underlying alloc fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

