# uranite.crypto.keystore

## Table of Contents

- [Imports](#imports)
- [class `SecureKeyStore`](#class-securekeystore)
  - [`SecureKeyStore()`](#SecureKeyStore)
  - [`storeKey()`](#storeKey)
  - [`revokeKey()`](#revokeKey)
  - [`destroyKey()`](#destroyKey)
  - [`isOwner()`](#isOwner)
  - [`getKeyType()`](#getKeyType)
  - [`getKeyState()`](#getKeyState)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.os.crypto.keystore`
  - `KeyState`
  - `KeyStore`
  - `KeyType`

## class `SecureKeyStore`

Managed cryptographic key storage with lifecycle tracking. Keys transition through Active → Revoked → Destroyed states. Ownership is tracked per-key via owner IDs.

### Fields

| Name | Type | Access |
|------|------|--------|
| `store` | `KeyStore` | protect |

### Methods

#### `function SecureKeyStore( self, I64 capacity ) -> Void`

Construct a key store that can hold up to capacity keys.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function storeKey( self, I64 keyType, I64 ownerId, I64 d0, I64 d1, I64 d2, I64 d3 ) -> I64`

Store a new key with the given type and owner. Key material is specified as four 64-bit words. Returns the storage slot index.

#### `function revokeKey( self, I64 slot ) -> Boolean`

Revoke the key at the given slot. Returns True on success. 

#### `function destroyKey( self, I64 slot ) -> Boolean`

Permanently destroy the key at the given slot. Returns True on success. 

#### `function isOwner( self, I64 slot, I64 ownerId ) -> Boolean`

Check whether ownerId is the owner of the key at slot. 

#### `function getKeyType( self, I64 slot ) -> I64`

Return the type of the key at the given slot. 

#### `function getKeyState( self, I64 slot ) -> I64`

Return the lifecycle state of the key at the given slot. 

#### `function getCount( self ) -> I64`

Return the number of keys currently stored. 

#### `function destroy( self ) -> Void`

Release all resources held by the key store.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

