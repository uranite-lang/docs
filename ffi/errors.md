# uranite.ffi.errors

## Table of Contents

- [Imports](#imports)
- [class `FfiError`](#class-ffierror)
  - [`FfiError()`](#FfiError)
- [class `LibraryLoadError`](#class-libraryloaderror)
  - [`LibraryLoadError()`](#LibraryLoadError)
- [class `SymbolNotFoundError`](#class-symbolnotfounderror)
  - [`SymbolNotFoundError()`](#SymbolNotFoundError)

## Imports

- `uranite.errors.error`
  - `Error`

## class `FfiError`

**Extends**: `Error`

Construct an FFI error with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the FFI failure.`
- `code` (`I64`)
- `Numeric error code, 0 for generic FFI errors.`
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function FfiError( self, String message, I64 code, ?Error cause ) -> Void`

Construct an FFI error with a descriptive message.

**Parameters**:

- `message` (`String`)
- `Human-readable description of the FFI failure.`
- `code` (`I64`)
- `Numeric error code, 0 for generic FFI errors.`
- `cause` (`?Error`)
- `Optional chained error, or None.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `LibraryLoadError`

**Extends**: `FfiError` → `Error`

Raised when a dynamic library cannot be loaded.

Typically caused by a missing shared object file, invalid path, or unresolved symbol dependencies within the library.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `libraryPath` | `String` | public |

### Methods

#### `function LibraryLoadError( self, String libraryPath, String systemError ) -> Void`

Construct a LibraryLoadError from the library path and system error message.

**Parameters**:

- `libraryPath` (`String`)
- `Filesystem path to the library that failed to load.`
- `systemError` (`String`)
- `Error message returned by the dynamic linker.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `SymbolNotFoundError`

**Extends**: `FfiError` → `Error`

Raised when a named symbol cannot be resolved within a loaded dynamic library.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `symbolName` | `String` | public |
| `libraryPath` | `String` | public |

### Methods

#### `function SymbolNotFoundError( self, String symbolName, String libraryPath ) -> Void`

Construct a SymbolNotFoundError from the symbol name and library path.

**Parameters**:

- `symbolName` (`String`)
- `Name of the symbol that could not be resolved.`
- `libraryPath` (`String`)
- `Filesystem path to the library that was searched.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

