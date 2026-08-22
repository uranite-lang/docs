# uranite.errors.traceback.frame

## Table of Contents

- [struct `Frame`](#struct-frame)
  - [`Frame()`](#Frame)
  - [`toString()`](#toString)

## struct `Frame`

Single stack frame record for diagnostic reporting.

Each Frame captures the source location of one call site in the execution stack. Traceback objects hold an ordered sequence of these frames representing the full call stack at the point where an error was raised.

### Fields

| Name | Type | Access |
|------|------|--------|
| `file` | `String` | public |
| `line` | `I64` | public |
| `functionName` | `String` | public |
| `moduleName` | `String` | public |

### Methods

#### `function Frame( self, String file, I64 line, String functionName, String moduleName ) -> Void`

Create a new stack frame entry for the given source location.

**Parameters**:

- `file` (`String`)
- `The file path of the source file containing this stack frame.`
- `line` (`I64`)
- `The line number within the source file.`
- `functionName` (`String`)
- `The name of the function at this stack frame.`
- `moduleName` (`String`)
- `The name of the module containing this stack frame.`

#### `function toString( self ) -> String`

Return the file path as the string representation of this frame. 

