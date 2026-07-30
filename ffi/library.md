# uranite.ffi.library

## Table of Contents

- [Imports](#imports)
- [const `RTLD_LAZY`](#const-rtld-lazy)
- [const `RTLD_NOW`](#const-rtld-now)
- [const `RTLD_GLOBAL`](#const-rtld-global)
- [const `RTLD_LOCAL`](#const-rtld-local)
- [class `DynamicLibrary`](#class-dynamiclibrary)
  - [`DynamicLibrary()`](#DynamicLibrary)
  - [`open()`](#open)
  - [`close()`](#close)
  - [`lookupSymbol()`](#lookupSymbol)
  - [`isOpen()`](#isOpen)

## Imports

- `uranite.ffi.errors`
  - `LibraryLoadError`
  - `SymbolNotFoundError`

## const `RTLD_LAZY`

## const `RTLD_NOW`

## const `RTLD_GLOBAL`

## const `RTLD_LOCAL`

Wrapper around POSIX dynamic linking functions for loading shared objects at runtime. Provides safe open/close lifecycle and symbol lookup.

The caller is responsible for correct type casting of resolved symbol pointers. This is a low-level FFI primitive, not a type-safe binding generator.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `DynamicLibrary`

Wrapper around POSIX dynamic linking functions for loading shared objects at runtime. Provides safe open/close lifecycle and symbol lookup.

The caller is responsible for correct type casting of resolved symbol pointers. This is a low-level FFI primitive, not a type-safe binding generator.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `libraryHandle` | `I64` | protect |
| `libraryPath` | `String` | protect |
| `isLoaded` | `Boolean` | protect |

### Methods

#### `function DynamicLibrary( self, String libraryPath ) -> Void`

Construct a DynamicLibrary from a filesystem path. Does not open the library — call open() to load it into the process.

**Parameters**:

- `libraryPath` (`String`)
- `Path to the shared object file` (`.so`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function open( self ) -> Void`

Load the shared library into the process address space using RTLD_NOW (immediate symbol resolution).

**Raises**:

- `LibraryLoadError` → `FfiError` → `Error` — When the dynamic linker cannot load the library.

**Complexity**:
- Time: `O(n) where n is the number of symbols in the library`
- Space: `O(1)`

#### `function close( self ) -> Void`

Unload the shared library from the process. After close, all symbol pointers obtained from this library become invalid.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function lookupSymbol( self, String symbolName ) -> I64`

Resolve a named symbol within the loaded library. Returns the raw address as I64. The caller must cast this to the correct function pointer type.

**Parameters**:

- `symbolName` (`String`)
- `Name of the exported symbol to resolve.`

**Returns**: `I64` — Raw address of the resolved symbol.

**Raises**:

- `SymbolNotFoundError` → `FfiError` → `Error` — When the symbol does not exist in the library.

**Complexity**:
- Time: `O(1) amortized (hash table lookup in dynamic linker)`
- Space: `O(1)`

#### `function isOpen( self ) -> Boolean`

Check whether the library is currently loaded.

**Returns**: — Boolean:
True if the library has been opened and not yet closed.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

