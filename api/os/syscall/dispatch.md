# uranite.os.syscall.dispatch

## Table of Contents

- [Imports](#imports)
- [class `SyscallDispatch`](#class-syscalldispatch)
  - [`SyscallDispatch()`](#SyscallDispatch)
  - [`register()`](#register)
  - [`lookup()`](#lookup)
  - [`isRegistered()`](#isRegistered)
  - [`unregister()`](#unregister)
  - [`getTableSize()`](#getTableSize)
  - [`getRegisteredCount()`](#getRegisteredCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.error`
  - `SyscallError`

## class `SyscallDispatch`

Kernel-side syscall dispatch table that maps syscall numbers (passed in RAX on x86_64) to handler function addresses. Each handler follows the signature (I64, I64, I64, I64, I64, I64) -> I64, where the six arguments correspond to the register values RDI, RSI, RDX, R10, R8, and R9 as passed by the syscall instruction. The table is a fixed-size array of function addresses indexed by syscall number.

### Fields

| Name | Type | Access |
|------|------|--------|
| `handlers` | `Memory<I64>` | protect |
| `tableSize` | `I64` | protect |

### Methods

#### `function SyscallDispatch( self, I64 maxSyscalls ) -> Void`

Constructs a new syscall dispatch table with the specified maximum number of syscall entries. All handler slots are initialized to 0 (unregistered).

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function register( self, I64 number, I64 handlerAddress ) -> Boolean`

Registers a handler function address for the given syscall number. The handler will be invoked when userspace executes a syscall instruction with this number in RAX. Returns True if registration succeeded, or False if the syscall number is out of range.

#### `function lookup( self, I64 number ) -> I64`

Returns the handler function address registered for the given syscall number. Returns 0 if the syscall number is out of range or no handler has been registered.

#### `function isRegistered( self, I64 number ) -> Boolean`

Checks whether a handler has been registered for the given syscall number. Returns True if a non-zero handler address is present, or False if the number is out of range or unregistered.

#### `function unregister( self, I64 number ) -> I64`

Unregisters the handler for the given syscall number by setting the slot to 0. Returns the previously registered handler address, or 0 if the number was out of range or had no handler registered.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getTableSize( self ) -> I64`

Returns the maximum number of syscall entries this dispatch table supports. 

#### `function getRegisteredCount( self ) -> I64`

Counts and returns the number of syscall numbers that have registered handlers. Iterates through all slots and counts those with non-zero handler addresses.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Releases the heap-allocated memory buffer used to store handler addresses. 

