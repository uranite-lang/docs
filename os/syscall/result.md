# uranite.os.syscall.result

## Table of Contents

- [class `SyscallResult`](#class-syscallresult)
  - [`SyscallResult()`](#SyscallResult)
  - [`isOk()`](#isOk)
  - [`isError()`](#isError)
  - [`value()`](#value)
  - [`errorCode()`](#errorCode)
  - [`rawValue()`](#rawValue)

## class `SyscallResult`

Wraps the raw return value from a syscall instruction (RAX after syscall). By convention, a non-negative value indicates success and carries the result, while a negative value indicates an error where the absolute value is the error code from SyscallError.

### Fields

| Name | Type | Access |
|------|------|--------|
| `raw` | `I64` | protect |

### Methods

#### `function SyscallResult( self, I64 raw ) -> Void`

Constructs a new SyscallResult wrapping the raw value returned in RAX after executing a syscall instruction.

#### `function isOk( self ) -> Boolean`

Returns True if the syscall succeeded (raw return value is non-negative). 

#### `function isError( self ) -> Boolean`

Returns True if the syscall failed (raw return value is negative). 

#### `function value( self ) -> I64`

Returns the success value of the syscall result. This method should only be called when isOk() returns True, as the value is undefined for error results.

#### `function errorCode( self ) -> I64`

Returns the error code as a positive integer by negating the raw return value. This method should only be called when isError() returns True. The returned code corresponds to a SyscallError enum value.

#### `function rawValue( self ) -> I64`

Returns the raw uninterpreted return value from the syscall instruction. 

