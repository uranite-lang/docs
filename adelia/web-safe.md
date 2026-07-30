# uranite.adelia.web-safe

## Table of Contents

- [Imports](#imports)
- [class `WebSafe`](#class-websafe)
  - [`snapComponent()`](#snapComponent)
  - [`snap()`](#snap)
  - [`snapHex()`](#snapHex)
  - [`isWebSafe()`](#isWebSafe)

## Imports

- `uranite.adelia.rgb`
  - `RGB`
- `uranite.language.boolean`
  - `Boolean`

## class `WebSafe`

Static utility class for snapping arbitrary RGB colors to the nearest web-safe color value. The web-safe palette consists of 216 colors formed by taking all combinations of six intensity levels (0x00, 0x33, 0x66, 0x99, 0xCC, 0xFF) for each of the red, green, and blue channels. Each level is separated by exactly 51 in decimal. The snapping algorithm rounds each channel independently to the nearest multiple of 51.

### Methods

#### `function snapComponent( self, I32 component ) -> I32`

Snap a single RGB channel value (0-255) to the nearest web-safe value by dividing by 51, rounding to the nearest integer, and multiplying back by 51. The six possible output values are 0, 51, 102, 153, 204, and 255.

**Parameters**:

- `component` (`I32`)
- `A single channel value from 0 to 255.`

**Returns**: `I32` — The nearest web-safe value for this channel.

#### `function snap( self, I32 red, I32 green, I32 blue ) -> RGB`

Snap all three RGB channels independently to the nearest web-safe values, returning the resulting web-safe color as an RGB struct.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: — RGB:
An RGB struct with each channel snapped to web-safe values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function snapHex( self, I32 hex ) -> I32`

Snap a packed 24-bit hex color to the nearest web-safe color and return the result as a packed hex integer.

**Parameters**:

- `hex` (`I32`)
- `The packed hex color value in 0xRRGGBB format.`

**Returns**: — I32:
The nearest web-safe color as a packed hex value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isWebSafe( self, I32 red, I32 green, I32 blue ) -> Boolean`

Check whether the given RGB color is already a web-safe color, meaning each channel value is exactly one of 0, 51, 102, 153, 204, or 255.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `Boolean` — True if all three channels are web-safe values.

#### `function isComponentWebSafe( self, I32 component ) -> Boolean`

Check whether a single channel value is a web-safe value by verifying it is evenly divisible by 51.

**Parameters**:

- `component` (`I32`)
- `A single channel value from 0 to 255.`

**Returns**: — Boolean:
True if the component is a multiple of 51.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

