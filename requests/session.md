# uranite.requests.session

## Table of Contents

- [Imports](#imports)
- [class `Session`](#class-session)
  - [`Session()`](#Session)
  - [`request()`](#request)
  - [`get()`](#get)
  - [`post()`](#post)
  - [`put()`](#put)
  - [`del()`](#del)
  - [`patch()`](#patch)
  - [`postForm()`](#postForm)
  - [`destroy()`](#destroy)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.net.http.client`
  - `sendRequestRaw`
- `uranite.net.http.request`
  - `HttpRequest`
- `uranite.net.http.response`
  - `HttpResponse`
- `uranite.requests.headers`
  - `Headers`
  - `parseHeaders`
- `uranite.requests.response`
  - `Response`
- `uranite.requests.url`
  - `ParsedURL`
  - `parseURL`

## class `Session`

HTTP session with persistent default headers applied to every request.

Create a Session, configure shared headers (e.g., User-Agent, Authorization), then use get/post/put/delete/patch with URL strings. Session-level headers are merged into each request automatically.

### Fields

| Name | Type | Access |
|------|------|--------|
| `headers` | `Headers` | public |

### Methods

#### `function Session( self ) -> Void`

Create a new session with no default headers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function request( self, String method, String url, ?String body, ?String contentType ) -> Response`

Send an HTTP request using a URL string.

Parses the URL into host, port, and path components, constructs an HttpRequest, applies session-level headers, and sends via the low-level HTTP client.

**Parameters**:

- `method` (`String`)
- `The HTTP method` (`e.g., "GET", "POST"`)
- `url` (`String`)
- `The full URL` (`e.g., "http://127.0.0.1:8080/api/users"`)
- `body` (`?String`)
- `The request body, or empty string for no body.`
- `contentType` (`?String`)
- `The Content-Type header, or empty string to omit.`

**Returns**: — Response:
The server's response with parsed headers.

**Complexity**:
- Time: `O(n) where n is URL + headers size`
- Space: `O(n)`

#### `function get( self, String url ) -> Response`

Send an HTTP GET request to the given URL.

**Parameters**:

- `url` (`String`)
- `The full URL.`

**Returns**: `Response` — The server's response.

#### `function post( self, String url, String body ) -> Response`

Send an HTTP POST request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

#### `function put( self, String url, String body ) -> Response`

Send an HTTP PUT request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

#### `function del( self, String url ) -> Response`

Send an HTTP DELETE request.

**Parameters**:

- `url` (`String`)
- `The full URL.`

**Returns**: `Response` — The server's response.

#### `function patch( self, String url, String body ) -> Response`

Send an HTTP PATCH request with a JSON body.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The JSON-encoded request body.`

**Returns**: `Response` — The server's response.

#### `function postForm( self, String url, String body ) -> Response`

Send an HTTP POST request with URL-encoded form data.

**Parameters**:

- `url` (`String`)
- `The full URL.`
- `body` (`String`)
- `The URL-encoded form data.`

**Returns**: `Response` — The server's response.

#### `function destroy( self ) -> Void`

Release resources held by session headers.

