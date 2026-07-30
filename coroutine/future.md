# uranite.coroutine.future

## Table of Contents

- [Imports](#imports)
- [class `FutureError`](#class-futureerror)
  - [`FutureError()`](#FutureError)
- [const `FUTURE_PENDING`](#const-future-pending)
- [const `FUTURE_RESOLVED`](#const-future-resolved)
- [const `FUTURE_REJECTED`](#const-future-rejected)
- [enum `FutureState`](#enum-futurestate)
- [interface `Future`](#interface-future)
  - [`state()`](#state)
  - [`resolved()`](#resolved)
  - [`value()`](#value)
  - [`block()`](#block)
- [class `SimpleFuture`](#class-simplefuture)
  - [`SimpleFuture()`](#SimpleFuture)
  - [`SimpleFuture()`](#SimpleFuture)
  - [`state()`](#state)
  - [`resolved()`](#resolved)
  - [`value()`](#value)
  - [`block()`](#block)
  - [`evaluate()`](#evaluate)
  - [`destroy()`](#destroy)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.fiber.fiber`
  - `FIBER_TERMINATED`
  - `F_ERROR`
  - `F_STATE`
  - `Fiber`
  - `fiberTrampoline`

## class `FutureError`

**Extends**: `Error`

### Methods

#### `function FutureError( self, String message ) -> Void`

## const `FUTURE_PENDING`

## const `FUTURE_RESOLVED`

## const `FUTURE_REJECTED`

## enum `FutureState`

## interface `Future`<T>

### Methods

#### `function state( self ) -> FutureState`

Return current lifecycle state. 

#### `function resolved( self ) -> Boolean`

Check whether resolved with a value. 

#### `function value( self ) -> T`

Get resolved value. Raises if pending or rejected. 

#### `function block( self ) -> T`

Block until resolved and return value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `SimpleFuture`

**Implements**: `Future<I64>`

### Fields

| Name | Type | Access |
|------|------|--------|
| `fiber` | `Fiber` | public |
| `futureState` | `I64` | public |
| `resolvedValue` | `I64` | public |
| `evaluated` | `Boolean` | public |

### Methods

#### `function SimpleFuture( self, <type> entryFn ) -> Void`

Create a future that will lazily evaluate the given callable.

**Parameters**:

- `entryFn` (`Callable<I64, <>>`)
- `The callable to execute when the future is evaluated.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function SimpleFuture( self, I64 entryFn ) -> Void`

Create a future from a raw function address.

**Parameters**:

- `entryFn` (`I64`)
- `Raw address of the entry function.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function state( self ) -> FutureState`

#### `function resolved( self ) -> Boolean`

#### `function value( self ) -> I64`

#### `function block( self ) -> I64`

#### `function evaluate( self ) -> Void`

#### `function destroy( self ) -> Void`

