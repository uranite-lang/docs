# uranite.adelia.rgb

## Table of Contents

- [Imports](#imports)
- [struct `RGB`](#struct-rgb)
  - [`RGB()`](#RGB)
  - [`redPercent()`](#redPercent)
  - [`greenPercent()`](#greenPercent)
  - [`bluePercent()`](#bluePercent)
  - [`equals()`](#equals)
  - [`toString()`](#toString)

## Imports

- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`

## struct `RGB`

Represents a color in the Red-Green-Blue color space where each channel is stored as a 32-bit integer value in the range 0 to 255. RGB is the most common color representation used by displays, image formats, and graphics APIs. Red, green, and blue are the three additive primary colors that combine at full intensity to produce white light.

### Fields

| Name | Type | Access |
|------|------|--------|
| `red` | `I32` | public |
| `green` | `I32` | public |
| `blue` | `I32` | public |

### Methods

#### `function RGB( self, I32 red, I32 green, I32 blue ) -> Void`

Construct a new RGB color from individual red, green, and blue channel values. Each component should be in the range 0 to 255, where 0 represents no intensity and 255 represents full intensity for that channel.

**Parameters**:

- `red` (`I32`)
- `The red channel intensity, from 0` (`no red`)
- `green` (`I32`)
- `The green channel intensity, from 0` (`no green`)
- `blue` (`I32`)
- `The blue channel intensity, from 0` (`no blue`)

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property redPercent( self ) -> F64`

`property` 

Return the red channel value as a percentage of the maximum intensity, computed as red divided by 255.0 multiplied by 100.0.

**Returns**: `F64` — The red channel as a percentage from 0.0 to 100.0.

#### `property greenPercent( self ) -> F64`

`property` 

Return the green channel value as a percentage of the maximum intensity, computed as green divided by 255.0 multiplied by 100.0.

**Returns**: `F64` — The green channel as a percentage from 0.0 to 100.0.

#### `property bluePercent( self ) -> F64`

`property` 

Return the blue channel value as a percentage of the maximum intensity, computed as blue divided by 255.0 multiplied by 100.0.

**Returns**: `F64` — The blue channel as a percentage from 0.0 to 100.0.

#### `function equals( self, Object other ) -> Boolean`

Compare this RGB color with another object for component-wise equality. Two RGB colors are considered equal if their red, green, and blue channel values are all identical. Returns false if the other object is not an RGB.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is an RGB with identical channel values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a human-readable string representation of this RGB color in the format "RGB(red, green, blue)" showing the integer values of each channel.

**Returns**: — String:
A string representation such as "RGB(0, 75, 73)".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

