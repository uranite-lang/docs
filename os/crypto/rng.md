# uranite.os.crypto.rng

## Table of Contents

- [Imports](#imports)
- [class `Csprng`](#class-csprng)
  - [`Csprng()`](#Csprng)
  - [`seedFromHardware()`](#seedFromHardware)
  - [`seedFromGetrandom()`](#seedFromGetrandom)
  - [`seed()`](#seed)
  - [`next()`](#next)
  - [`nextBounded()`](#nextBounded)
  - [`fillRandom()`](#fillRandom)
  - [`isInitialized()`](#isInitialized)

## Imports

- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `Csprng`

Cryptographically Secure Pseudo-Random Number Generator implementing the xoshiro256** algorithm. The internal state consists of four 64-bit words (s0 through s3) providing a period of 2^256 - 1. Can be seeded from hardware RNG via the RDRAND instruction or manually for testing. All state mutations are protected by a spinlock for thread-safe concurrent access.

### Fields

| Name | Type | Access |
|------|------|--------|
| `s0` | `I64` | public |
| `s1` | `I64` | public |
| `s2` | `I64` | public |
| `s3` | `I64` | public |
| `initialized` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function Csprng( self ) -> Void`

Construct an uninitialized CSPRNG with all state words set to zero. Call seedFromHardware() or seed() before generating random values, as the all-zero state is a fixed point of the xoshiro256** algorithm.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function seedFromHardware( self ) -> Boolean`

Seed the CSPRNG from the hardware random number generator using the RDRAND instruction. Each of the four state words is seeded independently with a hardware-generated random value. Returns True if the seeding completed successfully.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function seedFromGetrandom( self ) -> Boolean`

Seed the CSPRNG from the kernel entropy pool via the getrandom syscall. This works on any Linux kernel >= 3.17 regardless of hardware RNG support. Reads 32 bytes (four I64 state words) with flags=0 (blocking, /dev/urandom quality). Returns True on success, False if the syscall returns an error.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function seed( self, I64 seed0, I64 seed1, I64 seed2, I64 seed3 ) -> Void`

Manually seed the CSPRNG with four 64-bit values. This is intended for testing or for platforms that do not support the RDRAND instruction. The provided values should have high entropy to ensure good random output.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function next( self ) -> I64`

Generate the next 64-bit pseudo-random value using the xoshiro256** algorithm. The result is computed as rotl(s1 * 5, 7) * 9. The internal state is advanced atomically under the spinlock to ensure thread safety.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function nextBounded( self, I64 bound ) -> I64`

Generate a pseudo-random value uniformly distributed in the range [0, bound). Returns 0 if bound is less than or equal to zero. The value is derived from next() with modulo reduction.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fillRandom( self, I64 count ) -> I64`

Generate the specified number of random bytes by calling next() repeatedly (each call produces 8 bytes). Returns the total number of bytes generated, which may exceed the requested count by up to 7.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function isInitialized( self ) -> Boolean`

Return whether this CSPRNG has been seeded and is ready for use. 

