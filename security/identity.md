# uranite.security.identity

## Table of Contents

- [Imports](#imports)
- [class `UserIdentity`](#class-useridentity)
  - [`UserIdentity()`](#UserIdentity)
  - [`isRoot()`](#isRoot)
  - [`getUid()`](#getUid)
  - [`getGid()`](#getGid)
  - [`getEffectiveUid()`](#getEffectiveUid)
  - [`getEffectiveGid()`](#getEffectiveGid)
  - [`setEffectiveUid()`](#setEffectiveUid)
  - [`setEffectiveGid()`](#setEffectiveGid)
  - [`canAccessAs()`](#canAccessAs)
  - [`matchesIdentity()`](#matchesIdentity)
  - [`inGroup()`](#inGroup)
  - [`verifyToken()`](#verifyToken)
  - [`isNobody()`](#isNobody)
  - [`dropToNobody()`](#dropToNobody)

## Imports

- `uranite.os.identity.identity`
  - `NOBODY_GID`
  - `NOBODY_UID`
  - `ProcessIdentity`
  - `ROOT_GID`
  - `ROOT_UID`

## class `UserIdentity`

User and group identity for access control decisions. Wraps the kernel ProcessIdentity with high-level privilege queries, identity matching, and token verification. Supports effective UID/GID escalation and de-escalation.

### Fields

| Name | Type | Access |
|------|------|--------|
| `identity` | `ProcessIdentity` | protect |

### Methods

#### `function UserIdentity( self, I64 userId, I64 groupId, I64 authToken ) -> Void`

Construct an identity with real user/group IDs and an authentication token. Effective IDs initially equal real IDs.

**Parameters**:

- `userId` (`I64`)
- `Real user identifier for this identity.`
- `groupId` (`I64`)
- `Real group identifier for this identity.`
- `authToken` (`I64`)
- `Kernel-assigned unforgeable authentication token.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isRoot( self ) -> Boolean`

Return whether this identity has root (superuser) privileges. 

#### `function getUid( self ) -> I64`

Return the real user identifier. 

#### `function getGid( self ) -> I64`

Return the real group identifier. 

#### `function getEffectiveUid( self ) -> I64`

Return the effective user identifier used for permission checks. 

#### `function getEffectiveGid( self ) -> I64`

Return the effective group identifier used for permission checks. 

#### `function setEffectiveUid( self, I64 effectiveUid ) -> Void`

Set effective user identifier for privilege escalation or de-escalation.

**Parameters**:

- `effectiveUid` (`I64`)
- `New effective user identifier.`

#### `function setEffectiveGid( self, I64 effectiveGid ) -> Void`

Set effective group identifier for privilege escalation or de-escalation.

**Parameters**:

- `effectiveGid` (`I64`)
- `New effective group identifier.`

#### `function canAccessAs( self, I64 targetUid ) -> Boolean`

Check whether this identity can access resources owned by targetUid. Root can access anything; non-root can only access own resources.

**Parameters**:

- `targetUid` (`I64`)
- `User identifier of the resource owner.`

**Returns**: `Boolean` — True if access is permitted.

#### `function matchesIdentity( self, I64 otherUid, I64 otherGid ) -> Boolean`

Check whether this identity matches a given user/group pair by comparing effective identifiers.

**Parameters**:

- `otherUid` (`I64`)
- `User identifier to compare against.`
- `otherGid` (`I64`)
- `Group identifier to compare against.`

**Returns**: `Boolean` — True if both effective UID and GID match.

#### `function inGroup( self, I64 groupId ) -> Boolean`

Check whether this identity belongs to the specified group.

**Parameters**:

- `groupId` (`I64`)
- `Group identifier to check membership against.`

**Returns**: `Boolean` — True if effective GID matches.

#### `function verifyToken( self, I64 expectedToken ) -> Boolean`

Verify authentication token against an expected value.

**Parameters**:

- `expectedToken` (`I64`)
- `Expected token value.`

**Returns**: `Boolean` — True if stored token matches.

#### `function isNobody( self ) -> Boolean`

Return whether this identity is the unprivileged nobody user. 

#### `function dropToNobody( self ) -> Void`

De-escalate privileges to the nobody user/group.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

