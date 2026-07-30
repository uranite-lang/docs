# uranite.threading.atomic

## Table of Contents

- [Imports](#imports)
- [class `AtomicI64`](#class-atomici64)
  - [`AtomicI64()`](#AtomicI64)
  - [`load()`](#load)
  - [`store()`](#store)
  - [`fetchAdd()`](#fetchAdd)
  - [`fetchSub()`](#fetchSub)
  - [`compareExchange()`](#compareExchange)
  - [`exchange()`](#exchange)
  - [`increment()`](#increment)
  - [`decrement()`](#decrement)
  - [`destroy()`](#destroy)
- [class `AtomicBoolean`](#class-atomicboolean)
  - [`AtomicBoolean()`](#AtomicBoolean)
  - [`load()`](#load)
  - [`store()`](#store)
  - [`compareExchange()`](#compareExchange)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`

## class `AtomicI64`

Lock-free atomic 64-bit integer backed by x86-64 atomic instructions. All operations provide sequential consistency through hardware-level atomicity (xchg, lock xadd, lock cmpxchg). Uses heap-allocated Memory<I64> storage for stable addressing from inline assembly.

### Fields

| Name | Type | Access |
|------|------|--------|
| `storage` | `Memory<I64>` | protect |

### Methods

#### `function AtomicI64( self, I64 initial ) -> Void`

Construct an atomic integer with the given initial value.

**Parameters**:

- `initial` (`I64`)
- `The initial value to store.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function load( self ) -> I64`

Atomically read the current value with a memory barrier.

**Returns**: `I64` — The current value of the atomic integer.

#### `function store( self, I64 value ) -> Void`

Atomically store a new value, using xchg for implicit full barrier.

**Parameters**:

- `value` (`I64`)
- `The value to store.`

#### `function fetchAdd( self, I64 delta ) -> I64`

Atomically add a delta to the current value and return the previous value. Uses the lock xadd instruction for hardware atomicity.

**Parameters**:

- `delta` (`I64`)
- `The value to add.`

**Returns**: `I64` — The value held before the addition.

#### `function fetchSub( self, I64 delta ) -> I64`

Atomically subtract a delta from the current value and return the previous value. Implemented by negating the delta and calling fetchAdd.

**Parameters**:

- `delta` (`I64`)
- `The value to subtract.`

**Returns**: `I64` — The value held before the subtraction.

#### `function compareExchange( self, I64 expected, I64 desired ) -> Boolean`

Atomically compare the current value with expected, and if equal, replace it with desired. Uses the lock cmpxchg instruction.

**Parameters**:

- `expected` (`I64`)
- `The value expected to be currently stored.`
- `desired` (`I64`)
- `The value to store if the current value matches expected.`

**Returns**: `Boolean` — True if the exchange was performed (value was equal to expected), False if the current value differed from expected.

#### `function exchange( self, I64 newValue ) -> I64`

Atomically replace the current value with a new value and return the old value. Uses xchg for implicit full barrier.

**Parameters**:

- `newValue` (`I64`)
- `The value to store.`

**Returns**: `I64` — The value that was replaced.

#### `function increment( self ) -> I64`

Atomically increment the value by one and return the previous value.

**Returns**: `I64` — The value held before incrementing.

#### `function decrement( self ) -> I64`

Atomically decrement the value by one and return the previous value.

**Returns**: `I64` — The value held before decrementing.

#### `function destroy( self ) -> Void`

Release the heap-allocated storage backing this atomic integer. The atomic must not be used after this call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `AtomicBoolean`

Lock-free atomic boolean built on top of AtomicI64. Stores True as 1 and False as 0. All operations inherit the sequential consistency guarantees of the underlying atomic integer.

### Fields

| Name | Type | Access |
|------|------|--------|
| `inner` | `AtomicI64` | protect |

### Methods

#### `function AtomicBoolean( self, Boolean initial ) -> Void`

Construct an atomic boolean with the given initial value.

**Parameters**:

- `initial` (`Boolean`)
- `The initial boolean value to store.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function load( self ) -> Boolean`

Atomically read the current boolean value.

**Returns**: `Boolean` — The current value of the atomic boolean.

#### `function store( self, Boolean value ) -> Void`

Atomically store a new boolean value.

**Parameters**:

- `value` (`Boolean`)
- `The boolean value to store.`

#### `function compareExchange( self, Boolean expected, Boolean desired ) -> Boolean`

Atomically compare the current value with expected, and if equal, replace it with desired.

**Parameters**:

- `expected` (`Boolean`)
- `The boolean value expected to be currently stored.`
- `desired` (`Boolean`)
- `The boolean value to store if the current value matches expected.`

**Returns**: — Boolean:
True if the exchange was performed, False otherwise.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function destroy( self ) -> Void`

Release the storage backing this atomic boolean. The atomic must not be used after this call.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

