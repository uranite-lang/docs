# uranite.io.line-iterator

## Table of Contents

- [Imports](#imports)
- [class `LineIterator`](#class-lineiterator)
  - [`LineIterator()`](#LineIterator)
  - [`has()`](#has)
  - [`next()`](#next)
  - [`close()`](#close)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.buffered-reader`
  - `BufferedReader`
- `uranite.io.syscall`
  - `O_RDONLY`
  - `stringLen`
  - `stringToPtr`
  - `sysOpen`
- `uranite.iterators.iterator`
  - `Iterator`

## class `LineIterator`

**Implements**: `Iterator<String>`

A line-by-line file iterator that implements the Iterator<String> interface. Reads a file using a BufferedReader with a 4096-byte chunk size, yielding one line at a time. Memory-safe for large files since only one chunk and the current line are held in memory at any time.

### Fields

| Name | Type | Access |
|------|------|--------|
| `reader` | `BufferedReader` | protect |
| `currentLine` | `String` | protect |
| `hasLine` | `Boolean` | protect |
| `closed` | `Boolean` | protect |

### Methods

#### `function LineIterator( self, String path ) -> Void`

Open the file at the given path and prepare the iterator for line-by-line reading. The file is opened in read-only mode and the first line is pre-fetched via advance.

**Parameters**:

- `path` (`String`)
- `The filesystem path to the file to iterate over.`

**Raises**:

- `IOError` → `Error` — If the file cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function advance( self ) -> Void`

Read the next line from the buffered reader and store it in currentLine. Sets hasLine to False when the end of file is reached with no remaining data.

#### `property has( self ) -> Boolean`

`property` 

Check whether the iterator has another line available.

**Returns**: `Boolean` — True if a line is available via next, False at end of file.

#### `property next( self ) -> String`

`property` 

Return the current line and advance the iterator to the next line. The returned line does not include the trailing newline character.

**Returns**: `String` — The current line from the file.

#### `function close( self ) -> Void`

Close the underlying buffered reader and release file descriptor resources. Safe to call multiple times; subsequent calls after the first are no-ops.

#### `function destroy( self ) -> Void`

Destructor that ensures the underlying file resources are released by delegating to close.

