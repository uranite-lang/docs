# uranite.web.route

## Table of Contents

- [Imports](#imports)
- [const `MAX_SEGMENTS`](#const-max-segments)
- [class `Route`](#class-route)
  - [`Route()`](#Route)
  - [`parsePattern()`](#parsePattern)
  - [`matchesMethod()`](#matchesMethod)
  - [`destroy()`](#destroy)

## Imports

- `uranite.functions.builtin`
  - `equals`
  - `substring`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## const `MAX_SEGMENTS`

Maximum number of URL path segments a single route pattern can contain. 

## class `Route`

Defines an HTTP route binding an HTTP method and URL pattern to a handler.

URL patterns support path parameter segments prefixed with a colon (e.g., "/users/:id/posts/:postId"). The pattern is parsed into segments at construction time, with each segment flagged as either a literal match or a parameter extraction point.

### Fields

| Name | Type | Access |
|------|------|--------|
| `method` | `String` | public |
| `pattern` | `String` | public |
| `handlerAddr` | `I64` | protect |
| `segmentCount` | `I64` | public |
| `segmentPtrs` | `Memory<I64>` | public |
| `isParam` | `Memory<I64>` | public |

### Methods

#### `function Route( self, String method, String pattern, <type> handler ) -> Void`

Construct a new Route binding an HTTP method and URL pattern to a typed handler function. The handler receives the parsed HttpRequest and an HttpResponse to populate.

Segments prefixed with ':' (ASCII 58) are marked as path parameters. The pattern is split on '/' (ASCII 47) delimiters.

**Parameters**:

- `method` (`String`)
- `The HTTP method to match` (`e.g., "GET", "POST"`)
- `pattern` (`String`)
- `The URL pattern with optional`
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Complexity**:
- Time: `O(s) where s is number of segments in pattern`
- Space: `O(s)`

#### `function Route( self, String method, String pattern, I64 handlerAddr ) -> Void`

Construct a Route from a raw handler function address. Internal use by Router and Controller.

**Parameters**:

- `method` (`String`)
- `The HTTP method to match.`
- `pattern` (`String`)
- `The URL pattern with optional`
- `handlerAddr` (`I64`)
- `The raw memory address of the handler function.`

**Complexity**:
- Time: `O(s) where s is number of segments in pattern`
- Space: `O(s)`

#### `function parsePattern( self ) -> Void`

Parse the URL pattern into individual segments.

Splits the pattern on '/' delimiters and classifies each segment as either a literal (isParam=0) or a named parameter (isParam=1) based on whether it starts with ':' (ASCII 58).

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function matchesMethod( self, String method ) -> Boolean`

Check whether this route matches the given HTTP method.

**Parameters**:

- `method` (`String`)
- `The HTTP method to compare against` (`case-sensitive`)

**Returns**: `Boolean` — True if the route's method matches the given method.

#### `function destroy( self ) -> Void`

Release heap-allocated segment and parameter flag buffers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

