# uranite.adelia.hsl

## Table of Contents

- [Imports](#imports)
- [struct `HSL`](#struct-hsl)
  - [`HSL()`](#HSL)
  - [`equals()`](#equals)
  - [`toString()`](#toString)

## Imports

- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`

## struct `HSL`

Represents a color in the Hue-Saturation-Lightness cylindrical color space. Hue is expressed as an angle in degrees (0.0 to 360.0) on the color wheel where 0 is red, 120 is green, and 240 is blue. Saturation ranges from 0.0 (completely desaturated gray) to 1.0 (fully saturated pure color). Lightness ranges from 0.0 (black) through 0.5 (pure color at full saturation) to 1.0 (white). This color space is ideal for generating harmonious color palettes through hue rotation and for adjusting perceived brightness independently of color tone.

### Fields

| Name | Type | Access |
|------|------|--------|
| `hue` | `F64` | public |
| `saturation` | `F64` | public |
| `lightness` | `F64` | public |

### Methods

#### `function HSL( self, F64 hue, F64 saturation, F64 lightness ) -> Void`

Construct a new HSL color from hue, saturation, and lightness components.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees, from 0.0 to 360.0. Values outside this`
- `range will wrap around the color wheel.`
- `saturation` (`F64`)
- `The color saturation from 0.0` (`gray`)
- `lightness` (`F64`)
- `The lightness from 0.0` (`black`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, Object other ) -> Boolean`

Compare this HSL color with another object for component-wise equality. Two HSL colors are considered equal if their hue, saturation, and lightness values are all identical. Returns false if the other object is not an HSL.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is an HSL with identical component values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a human-readable string representation of this HSL color in the format "HSL(hue, saturation, lightness)" showing the floating-point values of each component.

**Returns**: — String:
A string representation such as "HSL(178.0, 1.0, 0.15)".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

