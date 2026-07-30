# uranite.adelia.css-formatter

## Table of Contents

- [Imports](#imports)
- [class `CssFormatter`](#class-cssformatter)
  - [`hex()`](#hex)
  - [`rgb()`](#rgb)
  - [`hsl()`](#hsl)
  - [`hexFromRgb()`](#hexFromRgb)

## Imports

- `uranite.language.string`
  - `String`
- `uranite.math.math`
  - `round`

## class `CssFormatter`

Static utility class for generating CSS-compatible color value strings in three standard formats: hexadecimal (#RRGGBB), functional RGB notation (rgb(R, G, B)), and functional HSL notation (hsl(H, S%, L%)). All output strings conform to the CSS Color Module Level 4 specification and can be used directly in stylesheets, inline styles, or any context expecting a CSS color value. All methods are static (no self parameter).

### Methods

#### `function hex( self, I32 hexValue ) -> String`

Format a packed 24-bit hex color value as a CSS hex color string with a leading hash symbol and six lowercase hexadecimal digits.

**Parameters**:

- `hexValue` (`I32`)
- `The packed hex color value in 0xRRGGBB format.`

**Returns**: — String:
A CSS hex color string such as "#004b49".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function rgb( self, I32 red, I32 green, I32 blue ) -> String`

Format RGB channel values as a CSS rgb() functional notation string with integer decimal values separated by commas.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `String` — A CSS rgb string such as "rgb(0, 75, 73)".

#### `function hsl( self, F64 hue, F64 saturation, F64 lightness ) -> String`

Format HSL component values as a CSS hsl() functional notation string with the hue in degrees (no unit suffix) and saturation and lightness as percentages with the percent symbol.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `lightness` (`F64`)
- `The lightness from 0.0 to 1.0.`

**Returns**: — String:
A CSS hsl string such as "hsl(178, 100%, 15%)".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function hexFromRgb( self, I32 red, I32 green, I32 blue ) -> String`

Format RGB channel values as a CSS hex color string by first packing them into a 24-bit integer and then formatting with a leading hash.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `String` — A CSS hex color string such as "#004b49".

#### `function byteToHex( self, I32 value ) -> String`

Convert a single byte value (0-255) to a two-character lowercase hexadecimal string. Values less than 16 are zero-padded to maintain the two-character width.

**Parameters**:

- `value` (`I32`)
- `The byte value from 0 to 255.`

**Returns**: — String:
A two-character hex string such as "00", "4b", or "ff".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

