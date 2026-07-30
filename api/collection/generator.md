# uranite.collection.generator

## Table of Contents

- [Imports](#imports)
- [interface `Generator`](#interface-generator)

## Imports

- `uranite.iterators`
  - `Iterator`

## interface `Generator`<T>

**Implements**: `Iterator<T>`

Lazy iterator that yields values on demand via coroutine-style execution.

A Generator extends the Iterator protocol, allowing elements to be produced lazily rather than computed eagerly. This enables efficient processing of potentially infinite or expensive-to-compute sequences.

