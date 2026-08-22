# uranite.errors.traceback.traceback

## Table of Contents

- [Imports](#imports)
- [class `Traceback`](#class-traceback)
  - [`Traceback()`](#Traceback)
  - [`length()`](#length)
  - [`get()`](#get)
  - [`iterator()`](#iterator)
  - [`toString()`](#toString)
- [class `TracebackIterator`](#class-tracebackiterator)
  - [`TracebackIterator()`](#TracebackIterator)
  - [`has()`](#has)
  - [`next()`](#next)

## Imports

- `uranite.errors.traceback.frame`
  - `Frame`
- `uranite.iterators.iterable`
  - `Iterable`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.memory.memory`
  - `Memory`

## class `Traceback`

**Implements**: `Iterable<Frame>`

Immutable ordered sequence of stack frames captured at the point where a throwable was raised. Frames are ordered from outermost caller to the raise site. Backed by Memory<Frame> with a fixed size determined at capture time.

### Fields

| Name | Type | Access |
|------|------|--------|
| `frames` | `Memory<Frame>` | protect |
| `size` | `I64` | protect |

### Methods

#### `function Traceback( self, Memory<Frame> frames, I64 size ) -> Void`

Create a new traceback from a pre-populated frame buffer.

**Parameters**:

- `frames` (`Memory<Frame>`)
- `The buffer containing captured stack frame entries.`
- `size` (`I64`)
- `The number of valid frames in the buffer.`

#### `function length( self ) -> I64`

Return the number of stack frames in this traceback. 

#### `function get( self, I64 index ) -> Frame`

Return the stack frame at the given index. 

#### `property iterator( self ) -> Iterator<Frame>`

`property` 

Return a fresh iterator over frames in this traceback. 

#### `function toString( self ) -> String`

Return a string representation identifying this as a traceback with its frame count. 

## class `TracebackIterator`

**Implements**: `Iterator<Frame>`

Forward-only iterator over the stack frames in a Traceback.

### Fields

| Name | Type | Access |
|------|------|--------|
| `source` | `Traceback` | private |
| `position` | `I64` | private |

### Methods

#### `function TracebackIterator( self, Traceback source ) -> Void`

Create a new iterator positioned at the start of the given traceback.

**Parameters**:

- `source` (`Traceback`)
- `The traceback to iterate over.`

#### `property has( self ) -> Boolean`

`property` 

Check whether more frames remain to yield. 

#### `property next( self ) -> Frame`

`property` 

Return the current frame and advance to the next position. 

