# uranite.io.errors

## Table of Contents

- [Imports](#imports)
- [class `IOError`](#class-ioerror)
  - [`IOError()`](#IOError)
- [class `FileNotFoundError`](#class-filenotfounderror)
  - [`FileNotFoundError()`](#FileNotFoundError)
- [class `PermissionError`](#class-permissionerror)
  - [`PermissionError()`](#PermissionError)
- [class `FileExistsError`](#class-fileexistserror)
  - [`FileExistsError()`](#FileExistsError)
- [class `IsADirectoryError`](#class-isadirectoryerror)
  - [`IsADirectoryError()`](#IsADirectoryError)
- [class `NotADirectoryError`](#class-notadirectoryerror)
  - [`NotADirectoryError()`](#NotADirectoryError)
- [class `DirectoryNotEmptyError`](#class-directorynotemptyerror)
  - [`DirectoryNotEmptyError()`](#DirectoryNotEmptyError)
- [class `BrokenPipeError`](#class-brokenpipeerror)
  - [`BrokenPipeError()`](#BrokenPipeError)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.errors.throwable`
  - `Throwable`

## class `IOError`

**Extends**: `Error`

Base error class for all input/output operations. The error code typically corresponds to a Linux errno value for precise identification of the failure cause.

### Methods

#### `function IOError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new IOError with a descriptive message and error code.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the I/O failure.`
- `code` (`I64`)
- `The error code, typically a Linux errno value.`
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `FileNotFoundError`

**Extends**: `IOError` → `Error`

Raised when an operation references a file or directory that does not exist on the filesystem. Corresponds to Linux errno ENOENT (2).

### Methods

#### `function FileNotFoundError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new FileNotFoundError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the missing file or directory.`
- `code` (`I64`)
- `The error code, typically ENOENT` (`2`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PermissionError`

**Extends**: `IOError` → `Error`

Raised when an operation is denied due to insufficient filesystem permissions. Corresponds to Linux errno EACCES (13).

### Methods

#### `function PermissionError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new PermissionError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the permission failure.`
- `code` (`I64`)
- `The error code, typically EACCES` (`13`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `FileExistsError`

**Extends**: `IOError` → `Error`

Raised when an operation attempts to create a file or directory that already exists. Corresponds to Linux errno EEXIST (17).

### Methods

#### `function FileExistsError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new FileExistsError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the conflict.`
- `code` (`I64`)
- `The error code, typically EEXIST` (`17`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `IsADirectoryError`

**Extends**: `IOError` → `Error`

Raised when a file operation is attempted on a path that is a directory. Corresponds to Linux errno EISDIR (21).

### Methods

#### `function IsADirectoryError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new IsADirectoryError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the error.`
- `code` (`I64`)
- `The error code, typically EISDIR` (`21`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `NotADirectoryError`

**Extends**: `IOError` → `Error`

Raised when a directory operation is attempted on a path that is not a directory. Corresponds to Linux errno ENOTDIR (20).

### Methods

#### `function NotADirectoryError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new NotADirectoryError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the error.`
- `code` (`I64`)
- `The error code, typically ENOTDIR` (`20`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `DirectoryNotEmptyError`

**Extends**: `IOError` → `Error`

Raised when attempting to remove a directory that still contains entries. Corresponds to Linux errno ENOTEMPTY (39).

### Methods

#### `function DirectoryNotEmptyError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new DirectoryNotEmptyError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the error.`
- `code` (`I64`)
- `The error code, typically ENOTEMPTY` (`39`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `BrokenPipeError`

**Extends**: `IOError` → `Error`

Raised when writing to a pipe or socket whose read end has been closed. Corresponds to Linux errno EPIPE (32).

### Methods

#### `function BrokenPipeError( self, String message, I64 code, ?Throwable previous ) -> Void`

Construct a new BrokenPipeError.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the broken pipe condition.`
- `code` (`I64`)
- `The error code, typically EPIPE` (`32`)
- `previous` (`?Throwable`)
- `An optional previous exception in the cause chain, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

