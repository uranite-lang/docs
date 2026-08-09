# uranite.adelia.color

## Table of Contents

- [Imports](#imports)
- [class `Color`](#class-color)
  - [`Color()`](#Color)
  - [`red()`](#red)
  - [`green()`](#green)
  - [`blue()`](#blue)
  - [`hexValue()`](#hexValue)
  - [`toRgb()`](#toRgb)
  - [`toHsl()`](#toHsl)
  - [`toHsv()`](#toHsv)
  - [`toCmyk()`](#toCmyk)
  - [`cssHex()`](#cssHex)
  - [`cssRgb()`](#cssRgb)
  - [`cssHsl()`](#cssHsl)
  - [`ansiForeground()`](#ansiForeground)
  - [`ansiBackground()`](#ansiBackground)
  - [`ansiColorize()`](#ansiColorize)
  - [`complementary()`](#complementary)
  - [`webSafe()`](#webSafe)
  - [`distanceTo()`](#distanceTo)
  - [`equals()`](#equals)
  - [`toString()`](#toString)
  - [`fromRgb()`](#fromRgb)
  - [`fromHsl()`](#fromHsl)
  - [`fromHsv()`](#fromHsv)
  - [`fromCmyk()`](#fromCmyk)

## Imports

- `uranite.adelia.ansi-formatter`
  - `AnsiFormatter`
- `uranite.adelia.cmyk`
  - `CMYK`
- `uranite.adelia.color-converter`
  - `ColorConverter`
- `uranite.adelia.color-distance`
  - `ColorDistance`
- `uranite.adelia.css-formatter`
  - `CssFormatter`
- `uranite.adelia.hsl`
  - `HSL`
- `uranite.adelia.hsv`
  - `HSV`
- `uranite.adelia.rgb`
  - `RGB`
- `uranite.adelia.web-safe`
  - `WebSafe`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`

## class `Color`

Central color representation wrapping a packed 24-bit hexadecimal value with red in bits 16-23, green in bits 8-15, and blue in bits 0-7. Provides a rich API for extracting individual RGB channels, converting to other color spaces (HSL, HSV, CMYK), generating CSS and ANSI terminal color strings, computing complementary and web-safe variants, and measuring perceptual distance to other colors. Static factory methods allow constructing Color instances from RGB, HSL, HSV, or CMYK values without manual hex packing.

### Fields

| Name | Type | Access |
|------|------|--------|
| `hex` | `I32` | protect |

### Methods

#### `function Color( self, I32 hex ) -> Void`

Construct a Color from a packed 24-bit hexadecimal value where bits 16-23 encode the red channel, bits 8-15 encode the green channel, and bits 0-7 encode the blue channel.

**Parameters**:

- `hex` (`I32`)
- `The packed hex color value in 0xRRGGBB format.`

#### `property red( self ) -> I32`

`property` 

Extract and return the red channel value (bits 16-23) from the packed hex representation, yielding a value from 0 to 255.

**Returns**: `I32` — The red channel intensity.

#### `property green( self ) -> I32`

`property` 

Extract and return the green channel value (bits 8-15) from the packed hex representation, yielding a value from 0 to 255.

**Returns**: `I32` — The green channel intensity.

#### `property blue( self ) -> I32`

`property` 

Extract and return the blue channel value (bits 0-7) from the packed hex representation, yielding a value from 0 to 255.

**Returns**: `I32` — The blue channel intensity.

#### `property hexValue( self ) -> I32`

`property` 

Return the raw packed 24-bit hexadecimal color value.

**Returns**: `I32` — The packed hex value in 0xRRGGBB format.

#### `property toRgb( self ) -> RGB`

`property` 

Convert this color to an RGB struct with separate integer channel values.

**Returns**: `RGB` — An RGB struct with red, green, and blue components.

#### `property toHsl( self ) -> HSL`

`property` 

Convert this color to the HSL color space with hue in degrees (0-360), saturation (0.0-1.0), and lightness (0.0-1.0).

**Returns**: `HSL` — The equivalent color in the HSL color space.

#### `property toHsv( self ) -> HSV`

`property` 

Convert this color to the HSV color space with hue in degrees (0-360), saturation (0.0-1.0), and value (0.0-1.0).

**Returns**: `HSV` — The equivalent color in the HSV color space.

#### `property toCmyk( self ) -> CMYK`

`property` 

Convert this color to the CMYK subtractive color space with cyan, magenta, yellow, and key (black) components each from 0.0 to 1.0.

**Returns**: `CMYK` — The equivalent color in the CMYK color space.

#### `property cssHex( self ) -> String`

`property` 

Return this color as a CSS hex string in the format "#rrggbb" with a leading hash symbol and six lowercase hexadecimal digits.

**Returns**: `String` — A CSS hex color string such as "#004b49".

#### `property cssRgb( self ) -> String`

`property` 

Return this color as a CSS rgb() functional notation string with integer decimal channel values.

**Returns**: `String` — A CSS rgb string such as "rgb(0, 75, 73)".

#### `property cssHsl( self ) -> String`

`property` 

Return this color as a CSS hsl() functional notation string with the hue in degrees and saturation and lightness as percentages.

**Returns**: `String` — A CSS hsl string such as "hsl(178, 100%, 15%)".

#### `property ansiForeground( self ) -> String`

`property` 

Return an ANSI 24-bit true color escape sequence that sets the terminal foreground text color to this color.

**Returns**: `String` — An ANSI escape sequence such as "\x1b[38;2;0;75;73m".

#### `property ansiBackground( self ) -> String`

`property` 

Return an ANSI 24-bit true color escape sequence that sets the terminal background color to this color.

**Returns**: `String` — An ANSI escape sequence such as "\x1b[48;2;0;75;73m".

#### `function ansiColorize( self, String text ) -> String`

Wrap the given text with ANSI foreground color escape sequences using this color, automatically appending a reset code so only the specified text is colored.

**Parameters**:

- `text` (`String`)
- `The text to colorize.`

**Returns**: `String` — The text wrapped with this color's foreground escape and reset.

#### `property complementary( self ) -> Color`

`property` 

Compute and return the complementary color by rotating the hue 180 degrees in HSL space while preserving saturation and lightness. The complementary color is the color directly opposite on the color wheel, providing maximum visual contrast.

**Returns**: — Color:
The complementary color.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `property webSafe( self ) -> Color`

`property` 

Return the nearest web-safe color by snapping each RGB channel to the closest multiple of 51 (the six web-safe intensity levels 0x00, 0x33, 0x66, 0x99, 0xCC, 0xFF).

**Returns**: — Color:
The nearest web-safe color.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function distanceTo( self, Color other ) -> F64`

Compute the perceptual distance between this color and another using the redmean weighted Euclidean algorithm, which approximates human color perception more accurately than simple Euclidean RGB distance.

**Parameters**:

- `other` (`Color`)
- `The other color to measure distance to.`

**Returns**: — F64:
The perceptual distance between the two colors.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function equals( self, Object other ) -> Boolean`

Compare this color with another object for equality based on hex value. Two colors are equal if and only if their packed hex values are identical.

**Parameters**:

- `other` (`Object`)
- `The object to compare against.`

**Returns**: — Boolean:
True if the other object is a Color with the same hex value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function toString( self ) -> String`

Return a human-readable string representation of this color showing the CSS hex format.

**Returns**: `String` — A string such as "Color(#004b49)".

#### `function fromRgb( I32 red, I32 green, I32 blue ) -> Color`

`static` 

Create a Color from individual RGB channel values by packing them into a 24-bit hex integer.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: — Color:
A new Color instance with the given RGB values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fromHsl( F64 hue, F64 saturation, F64 lightness ) -> Color`

`static` 

Create a Color from HSL components by converting to RGB and packing into a hex value.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `lightness` (`F64`)
- `The lightness from 0.0 to 1.0.`

**Returns**: — Color:
A new Color instance with the equivalent hex value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fromHsv( F64 hue, F64 saturation, F64 value ) -> Color`

`static` 

Create a Color from HSV components by converting to RGB and packing into a hex value.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `value` (`F64`)
- `The value` (`brightness`)

**Returns**: — Color:
A new Color instance with the equivalent hex value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function fromCmyk( F64 cyan, F64 magenta, F64 yellow, F64 key ) -> Color`

`static` 

Create a Color from CMYK components by converting to RGB and packing into a hex value.

**Parameters**:

- `cyan` (`F64`)
- `The cyan ink density from 0.0 to 1.0.`
- `magenta` (`F64`)
- `The magenta ink density from 0.0 to 1.0.`
- `yellow` (`F64`)
- `The yellow ink density from 0.0 to 1.0.`
- `key` (`F64`)
- `The black ink density from 0.0 to 1.0.`

**Returns**: — Color:
A new Color instance with the equivalent hex value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

