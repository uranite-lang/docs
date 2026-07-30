# uranite.adelia.hsv

## Table of Contents

- [Imports](#imports)
- [struct `HSV`](#struct-hsv)
  - [`HSV()`](#HSV)
  - [`equals()`](#equals)
  - [`toString()`](#toString)

## Imports

- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`

## struct `HSV`

Represents a color in the Hue-Saturation-Value cylindrical color space. Hue is expressed as an angle in degrees (0.0 to 360.0) on the color wheel where 0 is red, 120 is green, and 240 is blue. Saturation ranges from 0.0 (completely desaturated white/gray) to 1.0 (fully saturated pure color). Value ranges from 0.0 (black, regardless of hue and saturation) to 1.0 (the brightest possible shade of the given hue). Unlike HSL where maximum chroma occurs at lightness 0.5, in HSV maximum chroma occurs at value 1.0, making it more intuitive for color picker interfaces.

### Fields

| Name | Type | Access |
|------|------|--------|
| `hue` | `F64` | public |
| `saturation` | `F64` | public |
| `value` | `F64` | public |

### Methods

#### `function HSV( self, F64 hue, F64 saturation, F64 value ) -> Void`

Construct a new HSV color from hue, saturation, and value components.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees, from 0.0 to 360.0. Values outside this`
- `range will wrap around the color wheel.`
- `saturation` (`F64`)
- `The color saturation from 0.0` (`white/gray`)
- `value` (`F64`)
- `The brightness from 0.0` (`black`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, Object other ) -> Boolean`

Compare this HSV color with another object for component-wise equality. Two HSV colors are considered equal if their hue, saturation, and value components are all identical. Returns false if the other object is not an HSV.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is an HSV with identical component values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a human-readable string representation of this HSV color in the format "HSV(hue, saturation, value)" showing the floating-point values of each component.

**Returns**: — String:
A string representation such as "HSV(178.0, 1.0, 0.29)".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

