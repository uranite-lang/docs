# uranite.web.static-files

## Table of Contents

- [Imports](#imports)
- [function `guessMimeType`](#function-guessmimetype)
  - [`guessMimeType()`](#guessMimeType)
- [class `StaticFileHandler`](#class-staticfilehandler)
  - [`StaticFileHandler()`](#StaticFileHandler)
  - [`serve()`](#serve)

## Imports

- `uranite.functions.builtin`
  - `endsWith`
  - `ptrToString`
- `uranite.io.syscall`
  - `O_RDONLY`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `sysClose`
  - `sysOpen`
  - `sysRead`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## function `guessMimeType`

Determine the MIME type of a file based on its extension.

Checks the file path suffix against a set of known extensions and returns the corresponding MIME type. Falls back to "application/octet-stream" for unrecognized extensions.

**Parameters**:

- `path` (`String`)
- `The file path or filename to inspect.`

**Returns**: — String:
The MIME type string for the file's extension.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function guessMimeType( String path ) -> String`

Determine the MIME type of a file based on its extension.

Checks the file path suffix against a set of known extensions and returns the corresponding MIME type. Falls back to "application/octet-stream" for unrecognized extensions.

**Parameters**:

- `path` (`String`)
- `The file path or filename to inspect.`

**Returns**: — String:
The MIME type string for the file's extension.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `StaticFileHandler`

Serves static files from a directory on disk for requests matching a given URL prefix.

Maps the portion of the request path after the URL prefix to a file within the root directory, reads the file content, determines the MIME type from the file extension, and populates the response.

### Fields

| Name | Type | Access |
|------|------|--------|
| `rootDir` | `String` | public |
| `urlPrefix` | `String` | public |

### Methods

#### `function StaticFileHandler( self, String urlPrefix, String rootDir ) -> Void`

Construct a new StaticFileHandler.

**Parameters**:

- `urlPrefix` (`String`)
- `The URL path prefix to match` (`e.g., "/static"`)
- `rootDir` (`String`)
- `The filesystem directory containing the static files.`

#### `function serve( self, HttpRequest req, HttpResponse res ) -> Boolean`

Attempt to serve a static file for the given request.

Strips the URL prefix from the request path, resolves the remaining path against the root directory, reads the file content (up to 64KB), sets the appropriate MIME type, and populates the response body.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request whose path is checked against`
- `the URL prefix.`
- `res` (`HttpResponse`)
- `The response to populate with file content and MIME type`
- `if the file exists.`

**Returns**: — Boolean:
True if the file was found and served successfully,
False if the path does not match the prefix or the file
could not be opened.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

