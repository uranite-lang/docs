# uranite.language.char

## Table of Contents

- [class `Char`](#class-char)
  - [`Char()`](#Char)
  - [`getValue()`](#getValue)
  - [`toString()`](#toString)
  - [`isAlpha()`](#isAlpha)
  - [`isDigit()`](#isDigit)
  - [`isAlphanumeric()`](#isAlphanumeric)
  - [`isWhitespace()`](#isWhitespace)
  - [`toUpper()`](#toUpper)
  - [`toLower()`](#toLower)

## class `Char`

Single Unicode character (32-bit). Provides character classification methods (alphabetic, digit, whitespace) and case conversion.

### Fields

| Name | Type | Access |
|------|------|--------|
| `value` | `Char` | protect |

### Methods

#### `function Char( self, Char value ) -> Void`

Construct a new Char wrapping the given character value. 

#### `function getValue( self ) -> Char`

Return the underlying character value. 

#### `function toString( self ) -> String`

Return this character as a single-character String. 

#### `function isAlpha( self ) -> Boolean`

Return True if this character is an alphabetic letter (a-z or A-Z). 

#### `function isDigit( self ) -> Boolean`

Return True if this character is a decimal digit (0-9). 

#### `function isAlphanumeric( self ) -> Boolean`

Return True if this character is either alphabetic or a digit. 

#### `function isWhitespace( self ) -> Boolean`

Return True if this character is a whitespace character (space, tab, newline, or carriage return). 

#### `function toUpper( self ) -> Char`

Return a new Char converted to uppercase, or the same character if not a lowercase letter.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toLower( self ) -> Char`

Return a new Char converted to lowercase, or the same character if not an uppercase letter.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

