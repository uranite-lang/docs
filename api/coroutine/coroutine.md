# uranite.coroutine.coroutine

## Table of Contents

- [Imports](#imports)
- [class `CoroutineError`](#class-coroutineerror)
  - [`CoroutineError()`](#CoroutineError)
- [const `COROUTINE_CREATED`](#const-coroutine-created)
- [const `COROUTINE_RUNNING`](#const-coroutine-running)
- [const `COROUTINE_SUSPENDED`](#const-coroutine-suspended)
- [const `COROUTINE_COMPLETED`](#const-coroutine-completed)
- [const `RESULT_YIELD`](#const-result-yield)
- [const `RESULT_COMPLETED`](#const-result-completed)
- [interface `Coroutine`](#interface-coroutine)
  - [`resume()`](#resume)
  - [`value()`](#value)
- [class `SimpleCoroutine`](#class-simplecoroutine)
  - [`SimpleCoroutine()`](#SimpleCoroutine)
  - [`SimpleCoroutine()`](#SimpleCoroutine)
  - [`resume()`](#resume)
  - [`value()`](#value)
  - [`result()`](#result)
  - [`isCompleted()`](#isCompleted)
  - [`isSuspended()`](#isSuspended)
  - [`isCreated()`](#isCreated)
  - [`destroy()`](#destroy)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.fiber.fiber`
  - `FIBER_SUSPENDED`
  - `FIBER_TERMINATED`
  - `F_STATE`
  - `Fiber`
  - `fiberSuspend`
  - `fiberTrampoline`

## class `CoroutineError`

**Extends**: `Error`

### Methods

#### `function CoroutineError( self, String message ) -> Void`

## const `COROUTINE_CREATED`

## const `COROUTINE_RUNNING`

## const `COROUTINE_SUSPENDED`

## const `COROUTINE_COMPLETED`

## const `RESULT_YIELD`

## const `RESULT_COMPLETED`

## interface `Coroutine`<In, Out>

### Methods

#### `function resume( self, In value ) -> I64`

Resume coroutine with value. Returns RESULT_YIELD or RESULT_COMPLETED. 

#### `function value( self ) -> Out`

Get last yielded value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `SimpleCoroutine`

**Implements**: `Coroutine<I64, I64>`

### Fields

| Name | Type | Access |
|------|------|--------|
| `fiber` | `Fiber` | public |
| `state` | `I64` | public |
| `yieldedValue` | `I64` | public |
| `returnValue` | `I64` | public |
| `hasYield` | `Boolean` | public |
| `hasReturn` | `Boolean` | public |

### Methods

#### `function SimpleCoroutine( self, <type> entryFn ) -> Void`

Create a coroutine that will execute the given callable.

**Parameters**:

- `entryFn` (`Callable<I64, <I64>>`)
- `The coroutine body function. Receives a value from resume`
- `and returns a value via fiberSuspend`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function SimpleCoroutine( self, I64 entryFn ) -> Void`

Create a coroutine from a raw function address.

**Parameters**:

- `entryFn` (`I64`)
- `Raw address of the coroutine body function.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function resume( self, I64 value ) -> I64`

#### `function value( self ) -> I64`

#### `function result( self ) -> I64`

#### `function isCompleted( self ) -> Boolean`

#### `function isSuspended( self ) -> Boolean`

#### `function isCreated( self ) -> Boolean`

#### `function destroy( self ) -> Void`

