# uranite.errors.traceback

## Table of Contents

- [class `Traceback`](#class-traceback)
  - [`Traceback()`](#Traceback)
  - [`getFile()`](#getFile)
  - [`getLine()`](#getLine)
  - [`getFunctionName()`](#getFunctionName)
  - [`getModuleName()`](#getModuleName)
  - [`toString()`](#toString)

## class `Traceback`

Stack frame record for diagnostic reporting.

Each Traceback instance captures the source location of a single stack frame, including the file path, line number, function name, and module name. These records are used to construct stack traces when errors or exceptions are raised.

### Fields

| Name | Type | Access |
|------|------|--------|
| `file` | `String` | protect |
| `line` | `I64` | protect |
| `functionName` | `String` | protect |
| `moduleName` | `String` | protect |

### Methods

#### `function Traceback( self, String file, I64 line, String functionName, String moduleName ) -> Void`

Create a new traceback entry for the given source location.

**Parameters**:

- `file` (`String`)
- `The file path of the source file containing this stack frame.`
- `line` (`I32`)
- `The line number within the source file.`
- `functionName` (`String`)
- `The name of the function at this stack frame.`
- `moduleName` (`String`)
- `The name of the module containing this stack frame.`

#### `function getFile( self ) -> String`

Return the source file path of this stack frame. 

#### `function getLine( self ) -> I64`

Return the source line number of this stack frame. 

#### `function getFunctionName( self ) -> String`

Return the function name at this stack frame. 

#### `function getModuleName( self ) -> String`

Return the module name containing this stack frame. 

#### `function toString( self ) -> String`

Return a string representation of this traceback entry, which is the file path. 

