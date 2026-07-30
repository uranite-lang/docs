# uranite.net.errors

## Table of Contents

- [Imports](#imports)
- [class `SocketError`](#class-socketerror)
  - [`SocketError()`](#SocketError)
- [class `ConnectionError`](#class-connectionerror)
  - [`ConnectionError()`](#ConnectionError)
- [class `TimeoutError`](#class-timeouterror)
  - [`TimeoutError()`](#TimeoutError)
- [class `ConnectionRefusedError`](#class-connectionrefusederror)
  - [`ConnectionRefusedError()`](#ConnectionRefusedError)
- [class `ConnectionResetError`](#class-connectionreseterror)
  - [`ConnectionResetError()`](#ConnectionResetError)
- [class `AddressInUseError`](#class-addressinuseerror)
  - [`AddressInUseError()`](#AddressInUseError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `SocketError`

**Extends**: `Error`

Base error for all socket-related failures such as socket creation, binding, listening, accepting, connecting, sending, or receiving.

### Methods

#### `function SocketError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new SocketError with a descriptive message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the socket failure.`
- `code` (`I64`)
- `The errno value from the failed syscall.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ConnectionError`

**Extends**: `SocketError` → `Error`

Error raised when a connection-level operation fails, such as establishing or maintaining a TCP connection.

### Methods

#### `function ConnectionError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new ConnectionError with a descriptive message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the connection failure.`
- `code` (`I64`)
- `The errno value from the failed syscall.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `TimeoutError`

**Extends**: `SocketError` → `Error`

Error raised when a socket operation exceeds its configured timeout duration, such as a connect or read timeout.

### Methods

#### `function TimeoutError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new TimeoutError with a descriptive message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the timeout condition.`
- `code` (`I64`)
- `The errno value from the timed-out syscall.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ConnectionRefusedError`

**Extends**: `ConnectionError` → `SocketError` → `Error`

Error raised when a connect attempt is actively refused by the remote host, typically because no service is listening on the target port. Corresponds to ECONNREFUSED (errno 111).

### Methods

#### `function ConnectionRefusedError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new ConnectionRefusedError with a descriptive message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the refused connection.`
- `code` (`I64`)
- `The errno value from the failed connect syscall.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `ConnectionResetError`

**Extends**: `ConnectionError` → `SocketError` → `Error`

Error raised when an established connection is forcibly closed by the remote peer. Corresponds to ECONNRESET (errno 104).

### Methods

#### `function ConnectionResetError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new ConnectionResetError with a descriptive message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the connection reset.`
- `code` (`I64`)
- `The errno value from the failed syscall.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `AddressInUseError`

**Extends**: `SocketError` → `Error`

Error raised when a bind operation fails because the requested address and port combination is already in use by another socket. Corresponds to EADDRINUSE (errno 98).

### Methods

#### `function AddressInUseError( self, String message, I64 code, ?Error cause ) -> Void`

Create a new AddressInUseError with a descriptive message, error code, and optional cause.

**Parameters**:

- `message` (`String`)
- `A human-readable description of the address conflict.`
- `code` (`I64`)
- `The errno value from the failed bind syscall.`
- `cause` (`?Error`)
- `An optional chained error that triggered this one, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

