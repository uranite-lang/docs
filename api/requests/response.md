# uranite.requests.response

## Table of Contents

- [Imports](#imports)
- [class `Response`](#class-response)
  - [`Response()`](#Response)
  - [`raiseForStatus()`](#raiseForStatus)

## Imports

- `uranite.requests.errors`
  - `HTTPError`

## class `Response`

Represents an HTTP response returned by the requests library.

Provides convenient access to the status code, response body text, raw headers, and a boolean indicating whether the request succeeded (2xx status). Use raiseForStatus to convert non-success responses into exceptions.

### Fields

| Name | Type | Access |
|------|------|--------|
| `statusCode` | `I64` | public |
| `text` | `String` | public |
| `headers` | `String` | public |
| `ok` | `Boolean` | public |

### Methods

#### `function Response( self, I64 statusCode, String text, String headers ) -> Void`

Construct a new Response from raw HTTP response data.

The ok flag is automatically set to True if the status code is between 200 and 299 inclusive.

**Parameters**:

- `statusCode` (`I64`)
- `The HTTP status code.`
- `text` (`String`)
- `The response body content.`
- `headers` (`String`)
- `The raw response headers.`

#### `function raiseForStatus( self ) -> Void`

Raise an HTTPError if the response status code indicates failure.

Does nothing if the status code is in the 2xx success range. Otherwise, raises an HTTPError with the status code from the response.

**Raises**:

- `HTTPError` → `RequestError` → `Error` — If the response status code is outside the 2xx range.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

