# uranite.adelia.cmyk

## Table of Contents

- [Imports](#imports)
- [struct `CMYK`](#struct-cmyk)
  - [`CMYK()`](#CMYK)
  - [`equals()`](#equals)
  - [`toString()`](#toString)

## Imports

- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.f64`
  - `F64`
- `uranite.language.string`
  - `String`
- `uranite.language.void`
  - `Void`

## struct `CMYK`

Represents a color in the Cyan-Magenta-Yellow-Key (black) subtractive color space. Each component is a floating-point value from 0.0 to 1.0 representing the ink density for that channel, where 0.0 means no ink and 1.0 means full ink coverage. Cyan absorbs red light, magenta absorbs green light, and yellow absorbs blue light. The key (black) channel provides true black ink rather than relying on mixing the three color inks, which improves print quality and reduces ink consumption for dark areas.

### Fields

| Name | Type | Access |
|------|------|--------|
| `cyan` | `F64` | public |
| `magenta` | `F64` | public |
| `yellow` | `F64` | public |
| `key` | `F64` | public |

### Methods

#### `function CMYK( self, F64 cyan, F64 magenta, F64 yellow, F64 key ) -> Void`

Construct a new CMYK color from individual ink density values for each channel. All values should be in the range 0.0 to 1.0.

**Parameters**:

- `cyan` (`F64`)
- `The cyan ink density from 0.0` (`no cyan`)
- `magenta` (`F64`)
- `The magenta ink density from 0.0` (`no magenta`)
- `yellow` (`F64`)
- `The yellow ink density from 0.0` (`no yellow`)
- `key` (`F64`)
- `The black ink density from 0.0` (`no black`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, Object other ) -> Boolean`

Compare this CMYK color with another object for component-wise equality. Two CMYK colors are considered equal if all four channel values are identical. Returns false if the other object is not a CMYK.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is a CMYK with identical channel values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a human-readable string representation of this CMYK color in the format "CMYK(cyan, magenta, yellow, key)" showing the floating-point values of each ink density channel.

**Returns**: — String:
A string representation such as "CMYK(1.0, 0.0, 0.03, 0.71)".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

