# uranite.os.crypto.keystore

## Table of Contents

- [Imports](#imports)
- [enum `KeyType`](#enum-keytype)
- [enum `KeyState`](#enum-keystate)
- [class `KeyStore`](#class-keystore)
  - [`KeyStore()`](#KeyStore)
  - [`store()`](#store)
  - [`revoke()`](#revoke)
  - [`destroyKey()`](#destroyKey)
  - [`isOwner()`](#isOwner)
  - [`getKeyType()`](#getKeyType)
  - [`getKeyState()`](#getKeyState)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## enum `KeyType`

Enumerates the categories of cryptographic keys supported by the key store. 

## enum `KeyState`

Enumerates the lifecycle states of a cryptographic key in the store. 

## class `KeyStore`

Kernel-space cryptographic key store that holds keys in protected memory. Keys are stored as four I64 values (up to 256 bits of key material) and are identified by slot index. Each key tracks its type, lifecycle state, and owning process ID. All mutations are protected by a spinlock for interrupt-safe concurrent access.

### Fields

| Name | Type | Access |
|------|------|--------|
| `keyData0` | `Memory<I64>` | public |
| `keyData1` | `Memory<I64>` | public |
| `keyData2` | `Memory<I64>` | public |
| `keyData3` | `Memory<I64>` | public |
| `keyTypes` | `Memory<I64>` | public |
| `keyStates` | `Memory<I64>` | public |
| `ownerIds` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |
| `nextId` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function KeyStore( self, I64 maxKeys ) -> Void`

Construct a key store with capacity for the specified maximum number of keys. Allocates parallel arrays for key data, types, states, and owner IDs, initializing all slots to the destroyed state with zeroed key material.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function store( self, I64 keyType, I64 ownerId, I64 d0, I64 d1, I64 d2, I64 d3 ) -> I64`

Store a new cryptographic key in the first available destroyed slot. The key material is provided as four I64 values (d0 through d3), along with the key type and owning process ID. Returns a unique key ID on success, or -1 if the store is full and no slots are available.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function findSlot( self, I64 slot ) -> Boolean`

Check whether the given slot index is valid and contains an active key. Returns True if the slot is within bounds and its state is active, False otherwise.

#### `function revoke( self, I64 slot ) -> Boolean`

Revoke the key at the given slot, marking it as unusable for cryptographic operations without zeroizing its material. The key material remains in memory until destroyKey() is called. Returns True if the key was active and successfully revoked, False otherwise.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroyKey( self, I64 slot ) -> Boolean`

Destroy the key at the given slot by zeroizing all key material, clearing the key type, state, and owner ID, and freeing the slot for reuse. This operation is protected by a spinlock to prevent concurrent access during zeroization. Returns True if the key was successfully destroyed, False if the slot was already empty or invalid.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isOwner( self, I64 slot, I64 ownerId ) -> Boolean`

Check if the specified process owns the key at the given slot. Returns True if the slot is valid and the owner ID matches, False otherwise.

#### `function getKeyType( self, I64 slot ) -> I64`

Return the key type at the given slot, or 0 if the slot is out of range. 

#### `function getKeyState( self, I64 slot ) -> I64`

Return the key state at the given slot, or 0 if the slot is out of range. 

#### `function getCount( self ) -> I64`

Return the number of keys currently stored in the key store. 

#### `function destroy( self ) -> Void`

Securely destroy the entire key store by zeroizing all key material across all slots before freeing the underlying memory arrays. This ensures no residual key data remains in memory after the store is deallocated.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

