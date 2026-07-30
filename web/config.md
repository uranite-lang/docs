# uranite.web.config

## Table of Contents

- [class `ServerConfig`](#class-serverconfig)
  - [`ServerConfig()`](#ServerConfig)
  - [`setMaxConnections()`](#setMaxConnections)
  - [`setReadTimeout()`](#setReadTimeout)
  - [`setWriteTimeout()`](#setWriteTimeout)
  - [`setMaxRequestBodySize()`](#setMaxRequestBodySize)
  - [`setBacklog()`](#setBacklog)

## class `ServerConfig`

Configuration for the HTTP server including network binding, connection limits, timeouts, and request size constraints. All timeout values are specified in seconds.

### Fields

| Name | Type | Access |
|------|------|--------|
| `port` | `I64` | public |
| `host` | `String` | public |
| `maxConnections` | `I64` | public |
| `readTimeout` | `I64` | public |
| `writeTimeout` | `I64` | public |
| `maxRequestBodySize` | `I64` | public |
| `backlog` | `I64` | public |

### Methods

#### `function ServerConfig( self, I64 port, String host ) -> Void`

Create a new ServerConfig with the given port and host, using default values for all other settings.

**Parameters**:

- `port` (`I64`)
- `The TCP port number to listen on.`
- `host` (`String`)
- `The hostname or IP address to bind to.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function setMaxConnections( self, I64 max ) -> ServerConfig`

Set the maximum number of concurrent connections.

**Parameters**:

- `max` (`I64`)
- `The maximum connection count.`

**Returns**: — This ServerConfig instance for method chaining.

#### `function setReadTimeout( self, I64 secs ) -> ServerConfig`

Set the read timeout for incoming requests.

**Parameters**:

- `secs` (`I64`)
- `The read timeout in seconds.`

**Returns**: — This ServerConfig instance for method chaining.

#### `function setWriteTimeout( self, I64 secs ) -> ServerConfig`

Set the write timeout for outgoing responses.

**Parameters**:

- `secs` (`I64`)
- `The write timeout in seconds.`

**Returns**: — This ServerConfig instance for method chaining.

#### `function setMaxRequestBodySize( self, I64 size ) -> ServerConfig`

Set the maximum allowed request body size.

**Parameters**:

- `size` (`I64`)
- `The maximum body size in bytes.`

**Returns**: — This ServerConfig instance for method chaining.

#### `function setBacklog( self, I64 backlog ) -> ServerConfig`

Set the listen backlog size for pending connections.

**Parameters**:

- `backlog` (`I64`)
- `The maximum number of pending connections in the listen queue.`

**Returns**: — This ServerConfig instance for method chaining.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

