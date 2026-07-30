# uranite.web.controller

## Table of Contents

- [Imports](#imports)
- [const `MAX_CONTROLLER_ROUTES`](#const-max-controller-routes)
- [class `Controller`](#class-controller)
  - [`Controller()`](#Controller)
  - [`setPrefix()`](#setPrefix)
  - [`route()`](#route)
  - [`routeCount()`](#routeCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`
- `uranite.web.route`
  - `Route`

## const `MAX_CONTROLLER_ROUTES`

Maximum number of routes that can be registered on a single controller.

## class `Controller`

Base class for HTTP controllers. Subclasses register routes in their constructor by calling the route method with an HTTP method, URL pattern, and handler function address. An optional URL prefix can be set to scope all routes under a common path.

### Fields

| Name | Type | Access |
|------|------|--------|
| `controllerRoutes` | `Memory<I64>` | public |
| `controllerRouteCount` | `I64` | public |
| `prefix` | `String` | public |

### Methods

#### `function Controller( self ) -> Void`

Create a new Controller with no prefix and an empty route list.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setPrefix( self, String prefix ) -> Controller`

Set the URL prefix for this controller. All routes registered after this call will have this prefix prepended to their pattern.

**Parameters**:

- `prefix` (`String`)
- `The URL path prefix to prepend to route patterns.`

**Returns**: — This Controller instance for method chaining.

#### `function route( self, String method, String pattern, <type> handler ) -> Void`

Register a route on this controller with a typed handler. The pattern is combined with the controller's prefix to form the full URL pattern.

**Parameters**:

- `method` (`String`)
- `The HTTP method this route responds to` (`e.g., "GET", "POST"`)
- `pattern` (`String`)
- `The URL pattern for this route, which may include path parameters`
- `prefixed with a colon` (`e.g., "/users/:id"`)
- `handler` (`Callable<Void, <HttpRequest, HttpResponse>>`)
- `The handler function invoked when this route matches.`

**Complexity**:
- Time: `O(s) where s is number of segments in pattern`

#### `function getRoute( self, I64 index ) -> I64`

Retrieve the address of the Route object at the given index.

**Parameters**:

- `index` (`I64`)
- `The zero-based index of the route to retrieve.`

**Returns**: — The memory address of the Route object.

#### `function routeCount( self ) -> I64`

Return the number of routes registered on this controller.

**Returns**: — The total number of registered routes.

#### `function destroy( self ) -> Void`

Release the heap-allocated route storage buffer.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

