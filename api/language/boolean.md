# uranite.language.boolean

## Table of Contents

- [class `Boolean`](#class-boolean)
  - [`Boolean()`](#Boolean)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`negate()`](#negate)
  - [`logicalAnd()`](#logicalAnd)
  - [`logicalOr()`](#logicalOr)

## class `Boolean`

Truth value type representing True or False. Provides logical operations such as negation, conjunction, and disjunction.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `Boolean` | protect |

### Methods

#### `function Boolean( self, Boolean value ) -> Void`

Construct a new Boolean wrapping the given truth value. 

#### `function getValue( self ) -> Boolean`

Return the underlying truth value. 

#### `function toString( self ) -> String`

Return the string representation of this Boolean, either "True" or "False". 

#### `function negate( self ) -> Boolean`

Return a new Boolean with the opposite truth value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function logicalAnd( self, Boolean other ) -> Boolean`

Return a new Boolean representing the logical conjunction of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function logicalOr( self, Boolean other ) -> Boolean`

Return a new Boolean representing the logical disjunction of self and other.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

