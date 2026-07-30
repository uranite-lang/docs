# uranite.web.application

## Table of Contents

- [Imports](#imports)
- [const `MAX_CONTROLLERS`](#const-max-controllers)
- [const `MAX_MIDDLEWARE`](#const-max-middleware)
- [function `addrToController`](#function-addrtocontroller)
- [function `addrToRoute`](#function-addrtoroute)
- [class `Application`](#class-application)
  - [`Application()`](#Application)
  - [`config()`](#config)
  - [`controller()`](#controller)
  - [`middleware()`](#middleware)
  - [`exceptionHandler()`](#exceptionHandler)
  - [`buildRouter()`](#buildRouter)
  - [`run()`](#run)
  - [`destroy()`](#destroy)

## Imports

- `uranite.io.console`
  - `putsln`
- `uranite.memory.memory`
  - `Memory`
- `uranite.web.config`
  - `ServerConfig`
- `uranite.web.controller`
  - `Controller`
- `uranite.web.exception-handler`
  - `ExceptionHandler`
- `uranite.web.middleware`
  - `Middleware`
- `uranite.web.route`
  - `Route`
- `uranite.web.router`
  - `Router`
- `uranite.web.server`
  - `HttpServer`

## const `MAX_CONTROLLERS`

Maximum number of controllers that can be registered with an Application.

## const `MAX_MIDDLEWARE`

Maximum number of middleware instances that can be registered with an Application.

## function `addrToController`

### Methods

#### `function addrToController( I64 addr ) -> Controller`

## function `addrToRoute`

### Methods

#### `function addrToRoute( I64 addr ) -> Route`

## class `Application`

Top-level entry point for the Uranite web framework. Registers controllers, middleware, and exception handlers, then builds a router from all controller routes and starts an HTTP server. Provides a fluent API for configuration.

### Fields

| Name | Type | Access |
|------|------|--------|
| `appConfig` | `ServerConfig` | public |
| `controllers` | `Memory<I64>` | public |
| `controllerCount` | `I64` | public |
| `middlewares` | `Memory<I64>` | public |
| `middlewareCount` | `I64` | public |
| `exHandler` | `?ExceptionHandler` | public |

### Methods

#### `function Application( self ) -> Void`

Create a new Application with default configuration: port 8080, host 0.0.0.0, no controllers, no middleware, and no exception handler.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function config( self, ServerConfig cfg ) -> Application`

Set the server configuration for this application.

**Parameters**:

- `cfg` (`ServerConfig`)
- `The server configuration to use.`

**Returns**: — This Application instance for method chaining.

#### `function controller( self, Controller ctrl ) -> Application`

Register a controller with this application. The controller's routes will be collected and added to the router when the application starts.

**Parameters**:

- `ctrl` (`Controller`)
- `The controller to register.`

**Returns**: — This Application instance for method chaining.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function middleware( self, Middleware mw ) -> Application`

Register a middleware instance with this application. Middleware processes requests and responses in the order they are registered.

**Parameters**:

- `mw` (`Middleware`)
- `The middleware instance to register.`

**Returns**: — This Application instance for method chaining.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function exceptionHandler( self, ExceptionHandler handler ) -> Application`

Set the exception handler for this application. The handler converts unhandled exceptions into appropriate HTTP error responses.

**Parameters**:

- `handler` (`ExceptionHandler`)
- `The exception handler to use.`

**Returns**: — This Application instance for method chaining.

#### `function buildRouter( self ) -> Router`

Build a Router by collecting all routes from all registered controllers. Iterates through each controller and adds its routes to the router.

**Returns**: — A fully populated Router containing all registered routes.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function run( self ) -> Void`

Start the web application. Builds the router from registered controllers, creates an HTTP server with the configured settings, and enters the accept loop to handle incoming connections. This method blocks indefinitely.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release heap-allocated controller and middleware storage.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

