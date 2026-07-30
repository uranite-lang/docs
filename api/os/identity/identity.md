# uranite.os.identity.identity

## Table of Contents

- [Imports](#imports)
- [class `ProcessIdentity`](#class-processidentity)
  - [`ProcessIdentity()`](#ProcessIdentity)
  - [`getUid()`](#getUid)
  - [`getGid()`](#getGid)
  - [`getToken()`](#getToken)
  - [`getEffectiveUid()`](#getEffectiveUid)
  - [`getEffectiveGid()`](#getEffectiveGid)
  - [`setEffectiveUid()`](#setEffectiveUid)
  - [`setEffectiveGid()`](#setEffectiveGid)
  - [`isRoot()`](#isRoot)
  - [`matches()`](#matches)
  - [`inGroup()`](#inGroup)
  - [`verifyToken()`](#verifyToken)
- [const `ROOT_UID`](#const-root-uid)
- [const `ROOT_GID`](#const-root-gid)
- [const `NOBODY_UID`](#const-nobody-uid)
- [const `NOBODY_GID`](#const-nobody-gid)

## Imports

- `uranite.os.security.capability`
  - `Capability`

## class `ProcessIdentity`

Represents the identity of a running task within the kernel, tracking who the task is running as. Integrated with the capability system so that identity determines the default capability set. Uses token-based verification with unforgeable kernel-assigned credentials to prevent identity spoofing.

### Fields

| Name | Type | Access |
|------|------|--------|
| `uid` | `I64` | protect |
| `gid` | `I64` | protect |
| `token` | `I64` | protect |
| `effectiveUid` | `I64` | protect |
| `effectiveGid` | `I64` | protect |

### Methods

#### `function ProcessIdentity( self, I64 userId, I64 groupId, I64 authToken ) -> Void`

Construct a new process identity with the given user identifier, group identifier, and authentication token. The effective user and group identifiers are initially set equal to the real identifiers.

**Parameters**:

- `userId` (`I64`)
- `The real user identifier assigned to this process.`
- `groupId` (`I64`)
- `The real group identifier assigned to this process.`
- `authToken` (`I64`)
- `The kernel-assigned unforgeable authentication token.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getUid( self ) -> I64`

Return the real user identifier of this process identity. 

#### `function getGid( self ) -> I64`

Return the real group identifier of this process identity. 

#### `function getToken( self ) -> I64`

Return the kernel-assigned authentication token for this identity. 

#### `function getEffectiveUid( self ) -> I64`

Return the effective user identifier used for permission checks. 

#### `function getEffectiveGid( self ) -> I64`

Return the effective group identifier used for permission checks. 

#### `function setEffectiveUid( self, I64 euid ) -> Void`

Set the effective user identifier for privilege escalation or de-escalation. This changes the identity used for subsequent permission checks without altering the real user identifier.

**Parameters**:

- `euid` (`I64`)
- `The new effective user identifier to apply.`

#### `function setEffectiveGid( self, I64 egid ) -> Void`

Set the effective group identifier for privilege escalation or de-escalation. This changes the group identity used for subsequent permission checks without altering the real group identifier.

**Parameters**:

- `egid` (`I64`)
- `The new effective group identifier to apply.`

#### `function isRoot( self ) -> Boolean`

Check whether this identity is running with root privileges by testing if the effective user identifier is zero.

**Returns**: `Boolean` — True if the effective user identifier equals zero, indicating superuser privileges.

#### `function matches( self, I64 otherUid, I64 otherGid ) -> Boolean`

Check whether this identity matches a given user and group identifier pair by comparing against the effective identifiers.

**Parameters**:

- `otherUid` (`I64`)
- `The user identifier to compare against.`
- `otherGid` (`I64`)
- `The group identifier to compare against.`

**Returns**: `Boolean` — True if both the effective user and group identifiers match.

#### `function inGroup( self, I64 groupId ) -> Boolean`

Check whether this identity belongs to a specific group by comparing the effective group identifier.

**Parameters**:

- `groupId` (`I64`)
- `The group identifier to check membership against.`

**Returns**: `Boolean` — True if the effective group identifier matches the given group.

#### `function verifyToken( self, I64 expectedToken ) -> Boolean`

Verify that the authentication token matches an expected value. Used by the kernel to validate identity claims during capability-gated operations.

**Parameters**:

- `expectedToken` (`I64`)
- `The expected token value to compare against.`

**Returns**: — Boolean:
True if the stored token matches the expected token.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## const `ROOT_UID`

The well-known user identifier for the root superuser. 

## const `ROOT_GID`

The well-known group identifier for the root group. 

## const `NOBODY_UID`

The well-known user identifier for the unprivileged nobody user. 

## const `NOBODY_GID`

The well-known group identifier for the unprivileged nobody group. 

