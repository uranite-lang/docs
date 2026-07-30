# uranite.threading.shared

## Table of Contents

- [Imports](#imports)
- [class `Shared`](#class-shared)
  - [`Shared()`](#Shared)
  - [`clone()`](#clone)
  - [`get()`](#get)
  - [`set()`](#set)
  - [`getRefCount()`](#getRefCount)
  - [`release()`](#release)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`
- `uranite.threading.atomic`
  - `AtomicI64`

## class `Shared`<T>

Thread-safe reference-counted shared ownership container. Wraps a value of type T with atomic reference counting and spinlock-protected read/write access. Multiple owners can hold clones of the same Shared instance, all pointing to the same underlying storage. The storage is automatically freed when the last reference is released.

Access to the value is serialized through a spinlock, making both get and set operations atomic with respect to concurrent threads.

### Fields

| Name | Type | Access |
|------|------|--------|
| `storage` | `Memory<T>` | protect |
| `refCount` | `AtomicI64` | protect |
| `accessLock` | `Spinlock` | protect |

### Methods

#### `function Shared( self, T value ) -> Void`

Create a new shared container with an initial reference count of 1.

**Parameters**:

- `value` (`T`)
- `The initial value to store in the shared container.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function clone( self ) -> Shared<T>`

Create a new Shared instance that shares the same underlying storage, reference count, and access lock. Increments the reference count atomically. The returned clone and this instance both point to the same data.

**Returns**: — Shared<T>:
A new Shared instance sharing ownership of the same value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function get( self ) -> T`

Read the shared value under spinlock protection.

**Returns**: `T` — The current value stored in the shared container.

#### `function set( self, T value ) -> Void`

Write a new value to the shared container under spinlock protection.

**Parameters**:

- `value` (`T`)
- `The new value to store.`

#### `function getRefCount( self ) -> I64`

Return the current reference count.

**Returns**: `I64` — The number of live owners sharing this container.

#### `function release( self ) -> Boolean`

Decrement the reference count and free the underlying storage if this was the last reference. After calling release, this instance must not be used again.

**Returns**: — Boolean:
True if this was the last reference and storage was freed,
False if other references remain.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release this reference. Equivalent to calling release().

