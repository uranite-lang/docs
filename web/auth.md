# uranite.web.auth

## Table of Contents

- [Imports](#imports)
- [interface `AuthMiddleware`](#interface-authmiddleware)
  - [`authenticate()`](#authenticate)

## Imports

- `uranite.web.middleware`
  - `Middleware`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`

## interface `AuthMiddleware`

Interface for authentication middleware. Implementations should inspect the incoming request for credentials (e.g., headers, cookies, tokens) and determine whether the request is authenticated.

### Methods

#### `function authenticate( self, HttpRequest req ) -> Boolean`

Determine whether the given HTTP request is authenticated.

**Parameters**:

- `req` (`HttpRequest`)
- `The incoming HTTP request to authenticate.`

**Returns**: — True if the request is authenticated and should proceed, False otherwise.

