# uranite.coroutine.task

## Table of Contents

- [Imports](#imports)
- [class `TaskError`](#class-taskerror)
  - [`TaskError()`](#TaskError)
- [enum `TaskState`](#enum-taskstate)
- [interface `Task`](#interface-task)
  - [`id()`](#id)
  - [`state()`](#state)
  - [`cancel()`](#cancel)
  - [`result()`](#result)
- [class `SimpleTask`](#class-simpletask)
  - [`SimpleTask()`](#SimpleTask)
  - [`SimpleTask()`](#SimpleTask)
  - [`id()`](#id)
  - [`state()`](#state)
  - [`cancel()`](#cancel)
  - [`result()`](#result)
  - [`block()`](#block)
  - [`destroy()`](#destroy)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.fiber.fiber`
  - `FIBER_CREATED`
  - `FIBER_RUNNING`
  - `FIBER_SUSPENDED`
  - `FIBER_TERMINATED`
  - `F_ERROR`
  - `F_STATE`
  - `Fiber`
  - `fiberTrampoline`

## class `TaskError`

**Extends**: `Error`

### Methods

#### `function TaskError( self, String message ) -> Void`

## enum `TaskState`

## interface `Task`<T>

### Methods

#### `function id( self ) -> I64`

Return unique task identifier. 

#### `function state( self ) -> TaskState`

Return current lifecycle state. 

#### `function cancel( self ) -> Void`

Cancel task execution. 

#### `function result( self ) -> T`

Get result value. Raises if not completed. 

## class `SimpleTask`

**Implements**: `Task<I64>`

### Fields

| Name | Type | Access |
|------|------|--------|
| `fiber` | `Fiber` | public |
| `taskId` | `I64` | public |
| `started` | `Boolean` | public |

### Methods

#### `function SimpleTask( self, I64 taskId, <type> entryFn ) -> Void`

Create a task with the given identifier and callable entry function.

**Parameters**:

- `taskId` (`I64`)
- `Unique identifier for this task.`
- `entryFn` (`Callable<I64, <>>`)
- `The callable to execute as the task body.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function SimpleTask( self, I64 taskId, I64 entryFn ) -> Void`

Create a task from a raw function address.

**Parameters**:

- `taskId` (`I64`)
- `Unique identifier for this task.`
- `entryFn` (`I64`)
- `Raw address of the entry function.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function id( self ) -> I64`

#### `function state( self ) -> TaskState`

#### `function cancel( self ) -> Void`

#### `function result( self ) -> I64`

#### `function block( self ) -> I64`

#### `function destroy( self ) -> Void`

