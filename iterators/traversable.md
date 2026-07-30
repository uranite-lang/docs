# uranite.iterators.traversable

## Table of Contents

- [interface `Traversable`](#interface-traversable)

## interface `Traversable`

Marker interface for types that support some form of sequential element traversal. Implementing this interface signals that the type's elements can be visited in a defined order, serving as the root of the traversal type hierarchy. Concrete traversal contracts such as Iterable and Iterator extend this interface with specific method requirements for obtaining iterators or advancing through elements.

