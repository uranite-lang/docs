# uranite.language.object

## Table of Contents

- [class `Object`](#class-object)
  - [`toString()`](#toString)

## class `Object`

Root base class for all Uranite classes. Every class without an explicit base class implicitly extends Object. Subclasses should override toString to provide a meaningful string representation.

### Methods

#### `function toString( self ) -> String`

Return a string representation of this object. Subclasses should override this method to provide a meaningful description.

**Returns**: `String` — A String describing this object.

