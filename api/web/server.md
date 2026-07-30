# uranite.web.server

## Table of Contents

- [Imports](#imports)
- [const `RECV_BUF_SIZE`](#const-recv-buf-size)
- [function `parseMethod`](#function-parsemethod)
- [function `parsePath`](#function-parsepath)
- [function `parseBody`](#function-parsebody)
- [function `parseHeaders`](#function-parseheaders)
- [class `HttpServer`](#class-httpserver)
  - [`HttpServer()`](#HttpServer)
  - [`HttpServer()`](#HttpServer)
  - [`start()`](#start)
  - [`acceptLoop()`](#acceptLoop)
  - [`handleConnection()`](#handleConnection)
  - [`stop()`](#stop)

## Imports

- `uranite.functions.builtin`
  - `indexOf`
  - `ptrToString`
  - `substring`
- `uranite.io.console`
  - `putsln`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
- `uranite.net.socket`
  - `sendString`
  - `setTcpNoDelay`
  - `socketAccept`
  - `socketClose`
  - `socketRecv`
  - `tcpListen`
- `uranite.web.config`
  - `ServerConfig`
- `uranite.web.request`
  - `HttpRequest`
- `uranite.web.response`
  - `HttpResponse`
- `uranite.web.route`
  - `Route`
- `uranite.web.router`
  - `MatchResult`
  - `Router`

## const `RECV_BUF_SIZE`

Size in bytes of the receive buffer for incoming client data. 

## function `parseMethod`

Extract the HTTP method from a raw HTTP request string.

Reads from the start of the string up to the first space character (ASCII 32), which separates the method from the request URI.

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The HTTP method (e.g., "GET", "POST").

### Methods

#### `function parseMethod( String raw ) -> String`

Extract the HTTP method from a raw HTTP request string.

Reads from the start of the string up to the first space character (ASCII 32), which separates the method from the request URI.

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The HTTP method (e.g., "GET", "POST").

## function `parsePath`

Extract the request URI path from a raw HTTP request string.

Parses the text between the first and second space characters in the HTTP request line (e.g., "GET /path HTTP/1.1" yields "/path").

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The request URI path, or "/" if no path could be parsed.

### Methods

#### `function parsePath( String raw ) -> String`

Extract the request URI path from a raw HTTP request string.

Parses the text between the first and second space characters in the HTTP request line (e.g., "GET /path HTTP/1.1" yields "/path").

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The request URI path, or "/" if no path could be parsed.

## function `parseBody`

Extract the body content from a raw HTTP request string.

Locates the double CRLF sequence (\\r\\n\\r\\n) that separates headers from the body and returns everything after it.

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The request body, or empty string if no body is present.

### Methods

#### `function parseBody( String raw ) -> String`

Extract the body content from a raw HTTP request string.

Locates the double CRLF sequence (\\r\\n\\r\\n) that separates headers from the body and returns everything after it.

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The request body, or empty string if no body is present.

## function `parseHeaders`

Extract the raw header block from an HTTP request string.

Returns the text between the first CRLF (end of request line) and the double CRLF (end of headers), which contains all header lines.

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The raw header block, or empty string if no headers are found.

### Methods

#### `function parseHeaders( String raw ) -> String`

Extract the raw header block from an HTTP request string.

Returns the text between the first CRLF (end of request line) and the double CRLF (end of headers), which contains all header lines.

**Parameters**:

- `raw` (`String`)
- `The raw HTTP request data.`

**Returns**: `String` — The raw header block, or empty string if no headers are found.

## class `HttpServer`

HTTP/1.1 server that binds to a TCP socket, accepts client connections, parses raw HTTP requests, dispatches to matched route handlers, and sends serialized responses.

Uses raw syscalls for all network I/O with no C runtime dependency. Each connection is handled synchronously in the accept loop.

### Fields

| Name | Type | Access |
|------|------|--------|
| `serverFd` | `I64` | public |
| `router` | `Router` | public |
| `config` | `ServerConfig` | public |
| `running` | `Boolean` | public |

### Methods

#### `function HttpServer( self, I64 port ) -> Void`

Construct a new HttpServer on the given port with default configuration (host "0.0.0.0", backlog 128, max 1024 connections, 30s timeouts, 10 MB body limit) and an empty router.

**Parameters**:

- `port` (`I64`)
- `The TCP port number to listen on.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function HttpServer( self, ServerConfig config, Router router ) -> Void`

Construct a new HttpServer with the given configuration and router.

The server is not started until start() is called.

**Parameters**:

- `config` (`ServerConfig`)
- `The server configuration specifying port and backlog.`
- `router` (`Router`)
- `The router containing registered route definitions.`

#### `function start( self ) -> Void`

Bind the server socket and begin listening for connections.

Opens a TCP socket on the configured port with the configured backlog and marks the server as running.

#### `function acceptLoop( self ) -> Void`

Enter the main accept loop, handling one connection at a time.

Blocks on socketAccept, enables TCP_NODELAY on each accepted client socket, and delegates to handleConnection. Runs until the server is stopped via stop().

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function handleConnection( self, I64 clientFd ) -> Void`

Handle a single client connection from receive through response.

Reads raw HTTP data from the client socket, parses the method, path, body, and headers, constructs an HttpRequest and HttpResponse, resolves the route, serializes the response, and sends it back before closing the connection.

**Parameters**:

- `clientFd` (`I64`)
- `The file descriptor of the accepted client socket.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function stop( self ) -> Void`

Stop the server and close the listening socket.

Sets the running flag to False to exit the accept loop and closes the server socket if it was opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

