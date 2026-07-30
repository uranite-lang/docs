# uranite.ffi.symbol

## Table of Contents

- [class `ForeignSymbol`](#class-foreignsymbol)
  - [`ForeignSymbol()`](#ForeignSymbol)
  - [`address()`](#address)
  - [`name()`](#name)

## class `ForeignSymbol`

Handle to a resolved symbol from a dynamic library. Wraps the raw address returned by DynamicLibrary.lookupSymbol() with metadata for debugging and introspection.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Fields

| Name | Type | Access |
|------|------|--------|
| `symbolAddress` | `I64` | public |
| `symbolName` | `String` | public |
| `libraryPath` | `String` | public |

### Methods

#### `function ForeignSymbol( self, String symbolName, I64 symbolAddress, String libraryPath ) -> Void`

Construct a ForeignSymbol from a resolved address and metadata.

**Parameters**:

- `symbolName` (`String`)
- `Name of the resolved symbol.`
- `symbolAddress` (`I64`)
- `Raw address of the symbol in memory.`
- `libraryPath` (`String`)
- `Path to the library containing this symbol.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function address( self ) -> I64`

Return the raw memory address of the symbol.

**Returns**: — I64:
Address suitable for casting to a function pointer.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function name( self ) -> String`

Return the symbol name as resolved by the dynamic linker.

**Returns**: — String:
Symbol name string.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

