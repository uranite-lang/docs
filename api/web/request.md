# uranite.web.request

## Table of Contents

- [Imports](#imports)
- [const `MAX_PARAMS`](#const-max-params)
- [const `MAX_HEADERS`](#const-max-headers)
- [const `MAX_QUERY_PARAMS`](#const-max-query-params)
- [class `HttpRequest`](#class-httprequest)
  - [`HttpRequest()`](#HttpRequest)
  - [`setParam()`](#setParam)
  - [`param()`](#param)
  - [`query()`](#query)
  - [`destroy()`](#destroy)

## Imports

- `uranite.functions.builtin`
  - `equals`
  - `indexOf`
  - `ptrToString`
  - `substring`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`

## const `MAX_PARAMS`

Maximum number of path parameters a single request can hold. 

## const `MAX_HEADERS`

Maximum number of headers a single request can hold. 

## const `MAX_QUERY_PARAMS`

Maximum number of query string parameters a single request can hold. 

## class `HttpRequest`

Represents an incoming HTTP request with method, path, headers, body, path parameters, and query parameters.

Path parameters are populated by the router when matching URL patterns containing :param segments. Query parameters are extracted from the raw path at construction time.

### Fields

| Name | Type | Access |
|------|------|--------|
| `method` | `String` | public |
| `path` | `String` | public |
| `rawPath` | `String` | public |
| `body` | `String` | public |
| `contentType` | `String` | public |
| `rawHeaders` | `String` | public |
| `contentLength` | `I64` | public |
| `paramCount` | `I64` | public |
| `paramKeys` | `Memory<I64>` | public |
| `paramValues` | `Memory<I64>` | public |
| `queryCount` | `I64` | public |
| `queryKeys` | `Memory<I64>` | public |
| `queryValues` | `Memory<I64>` | public |

### Methods

#### `function HttpRequest( self, String method, String path, String body, String rawHeaders ) -> Void`

Construct a new HttpRequest from raw HTTP data.

Parses the path to separate the URL path from the query string. Allocates storage for path parameters and query parameters.

**Parameters**:

- `method` (`String`)
- `The HTTP method` (`e.g., "GET", "POST"`)
- `path` (`String`)
- `The full request path, possibly including a query string.`
- `body` (`String`)
- `The raw request body content.`
- `rawHeaders` (`String`)
- `The raw header block from the HTTP request.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setParam( self, String key, String value ) -> Void`

Store a path parameter extracted during route matching.

If the maximum number of path parameters has been reached, the parameter is silently dropped.

**Parameters**:

- `key` (`String`)
- `The parameter name` (`from the route pattern, without the leading colon`)
- `value` (`String`)
- `The parameter value extracted from the request path.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function param( self, String name ) -> String`

Retrieve a path parameter value by name.

Performs a linear scan over stored path parameters for a case-sensitive name match.

**Parameters**:

- `name` (`String`)
- `The parameter name to look up.`

**Returns**: — String:
The parameter value, or empty string if not found.

**Complexity**:
- Time: `O(n) where n is the number of stored path parameters`

#### `function query( self, String name ) -> String`

Retrieve a query parameter value by name.

Performs a linear scan over stored query parameters for a case-sensitive name match.

**Parameters**:

- `name` (`String`)
- `The query parameter name to look up.`

**Returns**: — String:
The query parameter value, or empty string if not found.

**Complexity**:
- Time: `O(n) where n is the number of stored query parameters`

#### `function destroy( self ) -> Void`

Release all heap-allocated parameter and query buffers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

