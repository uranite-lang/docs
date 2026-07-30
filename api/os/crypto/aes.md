# uranite.os.crypto.aes

## Table of Contents

- [Imports](#imports)
- [const `AES_128`](#const-aes-128)
- [const `AES_256`](#const-aes-256)
- [const `AES_BLOCK_SIZE`](#const-aes-block-size)
- [class `AesContext`](#class-aescontext)
  - [`AesContext()`](#AesContext)
  - [`init()`](#init)
  - [`hasHardwareSupport()`](#hasHardwareSupport)
  - [`getRounds()`](#getRounds)
  - [`getKeySize()`](#getKeySize)
  - [`isInitialized()`](#isInitialized)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## const `AES_128`

The AES-128 key size in bits. 

## const `AES_256`

The AES-256 key size in bits. 

## const `AES_BLOCK_SIZE`

The AES block size in bytes (128 bits). 

## class `AesContext`

AES encryption context that holds the expanded round keys for AES-128 or AES-256 operations. AES-128 uses 10 rounds and AES-256 uses 14 rounds. Each round key is 128 bits (2 I64 slots), so the total storage is (rounds + 1) * 2 I64 values for the key schedule.

### Fields

| Name | Type | Access |
|------|------|--------|
| `roundKeys` | `Memory<I64>` | public |
| `keySize` | `I64` | public |
| `rounds` | `I64` | public |
| `initialized` | `I64` | public |

### Methods

#### `function AesContext( self, I64 keyBits ) -> Void`

Construct an AES context for the specified key size in bits. Determines the number of rounds (10 for AES-128, 14 for AES-256) and allocates storage for the expanded round keys, initializing all slots to zero.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function init( self, I64 k0, I64 k1, I64 k2, I64 k3 ) -> Void`

Initialize the context with raw key material. For AES-128, only k0 and k1 are used (128 bits total as 2 I64 values). For AES-256, all four parameters k0 through k3 are stored (256 bits total as 4 I64 values). Marks the context as initialized after storing the key material.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function hasHardwareSupport( self ) -> Boolean`

Check if AES-NI hardware acceleration is available on the current CPU by executing CPUID with leaf 1 and testing bit 25 of the ECX register. Returns True if the processor supports AES-NI instructions.

#### `function getRounds( self ) -> I64`

Return the number of AES rounds for this context (10 for AES-128, 14 for AES-256). 

#### `function getKeySize( self ) -> I64`

Return the key size in bits for this context (128 or 256). 

#### `function isInitialized( self ) -> Boolean`

Return whether this context has been initialized with key material. 

#### `function destroy( self ) -> Void`

Securely destroy the AES context by zeroing all round key material before freeing the underlying memory. This prevents key material from remaining in memory after the context is no longer needed.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

