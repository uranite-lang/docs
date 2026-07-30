# uranite.crypto.random

## Table of Contents

- [Imports](#imports)
- [class `SecureRandom`](#class-securerandom)
  - [`SecureRandom()`](#SecureRandom)
  - [`nextI64()`](#nextI64)
  - [`nextBounded()`](#nextBounded)
  - [`nextInRange()`](#nextInRange)
  - [`fillRandom()`](#fillRandom)
  - [`isInitialized()`](#isInitialized)
- [function `randomBytes`](#function-randombytes)
  - [`randomBytes()`](#randomBytes)
- [const `O_RDONLY`](#const-o-rdonly)
- [class `DevRandom`](#class-devrandom)
  - [`DevRandom()`](#DevRandom)
  - [`readBytes()`](#readBytes)
  - [`readI64()`](#readI64)
  - [`fillBuffer()`](#fillBuffer)
  - [`close()`](#close)
- [class `DevUrandom`](#class-devurandom)
  - [`DevUrandom()`](#DevUrandom)
  - [`readBytes()`](#readBytes)
  - [`readI64()`](#readI64)
  - [`fillBuffer()`](#fillBuffer)
  - [`close()`](#close)
- [function `randomHexString`](#function-randomhexstring)
  - [`randomHexString()`](#randomHexString)

## Imports

- `uranite.functions.builtin`
  - `ptrToString`
- `uranite.io.syscall`
  - `readByteAt`
  - `stringToPtr`
  - `sysClose`
  - `sysOpen`
  - `sysRead`
  - `writeByteAt`
- `uranite.memory.allocator`
  - `alloc`
  - `dealloc`
- `uranite.os.crypto.rng`
  - `Csprng`

## class `SecureRandom`

Cryptographically secure random number generator using hardware entropy sources (RDRAND/RDSEED on x86-64). Wraps the kernel Csprng with a safe, high-level API.

### Fields

| Name | Type | Access |
|------|------|--------|
| `generator` | `Csprng` | protect |

### Methods

#### `function SecureRandom( self ) -> Void`

Construct and seed the random generator from the kernel entropy pool via the getrandom syscall. Works on any Linux kernel >= 3.17 regardless of hardware RNG support.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function nextI64( self ) -> I64`

Generate a random 64-bit integer. 

#### `function nextBounded( self, I64 bound ) -> I64`

Generate a random integer in [0, bound). Uses modular reduction on the raw CSPRNG output.

#### `function nextInRange( self, I64 minimum, I64 maximum ) -> I64`

Generate a random integer in [minimum, maximum). Delegates to nextBounded with the range width.

#### `function fillRandom( self, I64 count ) -> I64`

Generate count random bytes and return a pointer to the filled buffer.

#### `function isInitialized( self ) -> Boolean`

Return whether the generator has been successfully seeded.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `randomBytes`

Fill a buffer with cryptographically secure random bytes using hardware-seeded xoshiro256** CSPRNG. Each generated 64-bit value provides 8 bytes; partial final values are written byte-by-byte.

**Parameters**:

- `bufferAddress` (`I64`)
- `Destination buffer address to fill with random data.`
- `byteCount` (`I64`)
- `Number of random bytes to write.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function randomBytes( I64 bufferAddress, I64 byteCount ) -> Void`

Fill a buffer with cryptographically secure random bytes using hardware-seeded xoshiro256** CSPRNG. Each generated 64-bit value provides 8 bytes; partial final values are written byte-by-byte.

**Parameters**:

- `bufferAddress` (`I64`)
- `Destination buffer address to fill with random data.`
- `byteCount` (`I64`)
- `Number of random bytes to write.`

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## const `O_RDONLY`

Read-only flag for open syscall.

## class `DevRandom`

Reads from /dev/random, the blocking true-entropy device. On modern Linux kernels (5.6+), /dev/random blocks only until the entropy pool is initialized, then behaves like /dev/urandom. On older kernels it may block when the entropy pool is depleted.

Use for key generation, seeding, and scenarios requiring maximum entropy guarantees.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fileDescriptor` | `I64` | protect |

### Methods

#### `function DevRandom( self ) -> Void`

Open /dev/random for reading.

**Raises**:

- `Error` — If /dev/random cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readBytes( self, I64 byteCount ) -> I64`

Read exactly byteCount bytes of entropy and return the buffer address. Caller owns the returned memory and must deallocate it.

**Parameters**:

- `byteCount` (`I64`)
- `Number of random bytes to read.`

**Returns**: — I64:
Address of allocated buffer containing random bytes.

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(n)`

#### `function readI64( self ) -> I64`

Read 8 bytes of entropy and return as a single I64 value.

**Returns**: — I64:
A random 64-bit integer from /dev/random.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fillBuffer( self, I64 destinationAddress, I64 byteCount ) -> Void`

Read exactly byteCount bytes into the provided buffer.

**Parameters**:

- `destinationAddress` (`I64`)
- `Address of the buffer to fill.`
- `byteCount` (`I64`)
- `Number of random bytes to write.`

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(1)`

#### `function close( self ) -> Void`

Close the /dev/random file descriptor.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `DevUrandom`

Reads from /dev/urandom, the non-blocking CSPRNG device. Never blocks after the kernel entropy pool is initialized (which happens very early in boot). Suitable for most cryptographic use cases including session tokens, nonces, and general random data.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fileDescriptor` | `I64` | protect |

### Methods

#### `function DevUrandom( self ) -> Void`

Open /dev/urandom for reading.

**Raises**:

- `Error` — If /dev/urandom cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readBytes( self, I64 byteCount ) -> I64`

Read exactly byteCount bytes of random data and return the buffer address. Caller owns the returned memory and must deallocate it.

**Parameters**:

- `byteCount` (`I64`)
- `Number of random bytes to read.`

**Returns**: — I64:
Address of allocated buffer containing random bytes.

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(n)`

#### `function readI64( self ) -> I64`

Read 8 bytes of random data and return as a single I64 value.

**Returns**: — I64:
A random 64-bit integer from /dev/urandom.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fillBuffer( self, I64 destinationAddress, I64 byteCount ) -> Void`

Read exactly byteCount bytes into the provided buffer.

**Parameters**:

- `destinationAddress` (`I64`)
- `Address of the buffer to fill.`
- `byteCount` (`I64`)
- `Number of random bytes to write.`

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(1)`

#### `function close( self ) -> Void`

Close the /dev/urandom file descriptor.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `randomHexString`

Generate a cryptographically secure random hex string using hardware CSPRNG. Returns a lowercase hexadecimal string of length byteCount * 2.

**Parameters**:

- `byteCount` (`I64`)
- `Number of random bytes` (`output string is 2x this length`)

**Returns**: — String:
Lowercase hex string of random data.

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(n)`

### Methods

#### `function randomHexString( I64 byteCount ) -> String`

Generate a cryptographically secure random hex string using hardware CSPRNG. Returns a lowercase hexadecimal string of length byteCount * 2.

**Parameters**:

- `byteCount` (`I64`)
- `Number of random bytes` (`output string is 2x this length`)

**Returns**: — String:
Lowercase hex string of random data.

**Complexity**:
- Time: `O(n) where n is byteCount`
- Space: `O(n)`

