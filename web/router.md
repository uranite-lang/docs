# uranite.web.router

## Table of Contents

- [Imports](#imports)
- [const `MAX_ROUTES`](#const-max-routes)
- [function `addrToRoute`](#function-addrtoroute)
- [struct `MatchResult`](#struct-matchresult)
  - [`MatchResult()`](#MatchResult)
- [class `Router`](#class-router)
  - [`Router()`](#Router)
  - [`addRoute()`](#addRoute)
  - [`resolve()`](#resolve)
  - [`get()`](#get)
  - [`post()`](#post)
  - [`put()`](#put)
  - [`del()`](#del)
  - [`patch()`](#patch)
  - [`destroy()`](#destroy)

## Imports

- `uranite.functions.builtin`
  - `equals`
  - `substring`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
- `uranite.memory.memory`
  - `Memory`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`
- `uranite.web.route`
  - `Route`

## const `MAX_ROUTES`

Maximum number of routes that can be registered in a single Router. 

## function `addrToRoute`

Reinterpret a raw memory address as a Route object reference.

Uses inline assembly to cast the integer address back to a Route pointer. The caller must ensure the address points to a valid Route.

**Parameters**:

- `addr` (`I64`)
- `The memory address of a previously stored Route object.`

**Returns**: `Route` — The Route object at the given address.

### Methods

#### `function addrToRoute( I64 addr ) -> Route`

Reinterpret a raw memory address as a Route object reference.

Uses inline assembly to cast the integer address back to a Route pointer. The caller must ensure the address points to a valid Route.

**Parameters**:

- `addr` (`I64`)
- `The memory address of a previously stored Route object.`

**Returns**: `Route` — The Route object at the given address.

## struct `MatchResult`

Holds the result of a route resolution attempt, containing the matched handler address and a flag indicating whether a match was found.

### Fields

| Name | Type | Access |
|------|------|--------|
| `handlerAddr` | `I64` | protect |
| `matched` | `Boolean` | public |

### Methods

#### `function MatchResult( self, I64 handlerAddr, Boolean matched ) -> Void`

Construct a new MatchResult.

**Parameters**:

- `handlerAddr` (`I64`)
- `The handler function address, or 0 if unmatched.`
- `matched` (`Boolean`)
- `True if a route matched, False otherwise.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `Router`

HTTP request router that stores route definitions and matches incoming requests against registered URL patterns.

Routes are matched by HTTP method and URL path segment comparison. Path parameters (segments prefixed with ':' in the route pattern) are extracted and stored on the HttpRequest during resolution.

### Fields

| Name | Type | Access |
|------|------|--------|
| `routes` | `Memory<I64>` | public |
| `routeCount` | `I64` | public |

### Methods

#### `function Router( self ) -> Void`

Construct a new Router with empty route storage.

Allocates space for up to MAX_ROUTES route entries.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function addRoute( self, Route route ) -> Void`

Register a route with the router.

Stores the route's memory address for later resolution. If the maximum number of routes has been reached, the route is silently dropped.

**Parameters**:

- `route` (`Route`)
- `The route definition to register.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function resolve( self, String method, String path, HttpRequest req ) -> MatchResult`

Match an incoming request against all registered routes.

Splits the request path into segments and compares each segment against registered route patterns. For parameter segments, the corresponding path value is extracted and stored on the request via setParam. The first matching route wins.

**Parameters**:

- `method` (`String`)
- `The HTTP method of the incoming request.`
- `path` (`String`)
- `The URL path of the incoming request.`
- `req` (`HttpRequest`)
- `The request object where extracted path parameters will be stored.`

**Returns**: — MatchResult:
A result containing the handler address and match status.
If no route matches, handlerAddr is 0 and matched is False.

**Complexity**:
- Time: `O(r * s) where r is the number of registered routes and s is the number of segments in the longest pattern`

#### `function get( self, String pattern, <type> handler ) -> Router`

Register a GET route with a typed handler and return this router for method chaining.

**Parameters**:

- `pattern` (`String`)
- `The URL path pattern` (`e.g., "/users/:id"`)
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Returns**: — Router:
This router instance.

**Complexity**:
- Time: `O(s) where s is the number of segments in the pattern`

#### `function post( self, String pattern, <type> handler ) -> Router`

Register a POST route with a typed handler and return this router for method chaining.

**Parameters**:

- `pattern` (`String`)
- `The URL path pattern.`
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Returns**: — Router:
This router instance.

**Complexity**:
- Time: `O(s) where s is the number of segments in the pattern`

#### `function put( self, String pattern, <type> handler ) -> Router`

Register a PUT route with a typed handler and return this router for method chaining.

**Parameters**:

- `pattern` (`String`)
- `The URL path pattern.`
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Returns**: — Router:
This router instance.

**Complexity**:
- Time: `O(s) where s is the number of segments in the pattern`

#### `function del( self, String pattern, <type> handler ) -> Router`

Register a DELETE route with a typed handler and return this router for method chaining.

**Parameters**:

- `pattern` (`String`)
- `The URL path pattern.`
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Returns**: — Router:
This router instance.

**Complexity**:
- Time: `O(s) where s is the number of segments in the pattern`

#### `function patch( self, String pattern, <type> handler ) -> Router`

Register a PATCH route with a typed handler and return this router for method chaining.

**Parameters**:

- `pattern` (`String`)
- `The URL path pattern.`
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Returns**: — Router:
This router instance.

**Complexity**:
- Time: `O(s) where s is the number of segments in the pattern`

#### `function destroy( self ) -> Void`

Release the heap-allocated route storage buffer.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

