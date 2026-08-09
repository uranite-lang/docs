# uranite.adelia.color-converter

## Table of Contents

- [Imports](#imports)
- [class `ColorConverter`](#class-colorconverter)
  - [`rgbToHex()`](#rgbToHex)
  - [`hexToRgb()`](#hexToRgb)
  - [`rgbToHsl()`](#rgbToHsl)
  - [`hslToRgb()`](#hslToRgb)
  - [`rgbToHsv()`](#rgbToHsv)
  - [`hsvToRgb()`](#hsvToRgb)
  - [`rgbToCmyk()`](#rgbToCmyk)
  - [`cmykToRgb()`](#cmykToRgb)
  - [`hexToHsl()`](#hexToHsl)
  - [`hslToHex()`](#hslToHex)
  - [`hexToHsv()`](#hexToHsv)
  - [`hsvToHex()`](#hsvToHex)
  - [`hexToCmyk()`](#hexToCmyk)
  - [`cmykToHex()`](#cmykToHex)

## Imports

- `uranite.adelia.cmyk`
  - `CMYK`
- `uranite.adelia.hsl`
  - `HSL`
- `uranite.adelia.hsv`
  - `HSV`
- `uranite.adelia.rgb`
  - `RGB`
- `uranite.math.math`
  - `abs`
  - `fmod`
  - `round`

## class `ColorConverter`

Static utility class providing pure functions for converting colors between different color space representations. All methods are static (no self parameter) and produce no side effects. The supported color spaces are RGB (red-green-blue with 0-255 integer channels), HSL (hue-saturation-lightness with floating-point components), HSV (hue-saturation-value with floating-point components), CMYK (cyan-magenta-yellow-key with 0.0-1.0 ink densities), and packed hexadecimal (24-bit integer with red in bits 16-23, green in bits 8-15, blue in bits 0-7).

### Methods

#### `function rgbToHex( I32 red, I32 green, I32 blue ) -> I32`

`static` 

Pack three RGB channel values into a single 24-bit hexadecimal integer with red in the most significant byte, green in the middle byte, and blue in the least significant byte.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `I32` — The packed 24-bit hex value such that 0xRRGGBB.

#### `function hexToRgb( I32 hex ) -> RGB`

`static` 

Extract individual red, green, and blue channel values from a packed 24-bit hexadecimal color integer.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value in 0xRRGGBB format.`

**Returns**: — RGB:
An RGB struct with the extracted channel values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function rgbToHsl( I32 red, I32 green, I32 blue ) -> HSL`

`static` 

Convert an RGB color to the HSL color space using the standard algorithm. The hue is computed from the dominant channel and expressed in degrees (0-360), saturation measures color purity relative to lightness, and lightness is the average of the maximum and minimum channel intensities.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: — HSL:
The equivalent color in the HSL color space.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function hslToRgb( F64 hue, F64 saturation, F64 lightness ) -> RGB`

`static` 

Convert an HSL color back to the RGB color space. The algorithm computes chroma from saturation and lightness, determines which 60-degree hue sector the color falls in, and calculates the intermediate RGB values before scaling to the 0-255 integer range.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `lightness` (`F64`)
- `The lightness from 0.0 to 1.0.`

**Returns**: — RGB:
The equivalent color in the RGB color space with rounded integer channels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function rgbToHsv( I32 red, I32 green, I32 blue ) -> HSV`

`static` 

Convert an RGB color to the HSV color space. The hue is computed identically to HSL, but saturation is defined as chroma divided by the maximum channel value (not relative to lightness), and value is simply the maximum channel intensity.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: — HSV:
The equivalent color in the HSV color space.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function hsvToRgb( F64 hue, F64 saturation, F64 value ) -> RGB`

`static` 

Convert an HSV color back to the RGB color space. The algorithm computes chroma from value and saturation, determines the hue sector, and calculates the RGB channel values before scaling to the 0-255 integer range.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `value` (`F64`)
- `The value` (`brightness`)

**Returns**: — RGB:
The equivalent color in the RGB color space with rounded integer channels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function rgbToCmyk( I32 red, I32 green, I32 blue ) -> CMYK`

`static` 

Convert an RGB color to the CMYK subtractive color space. The key (black) component is derived from the maximum RGB channel, and the remaining CMY values represent how much of each subtractive primary is needed after accounting for the black ink contribution.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: — CMYK:
The equivalent color in the CMYK color space.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function cmykToRgb( F64 cyan, F64 magenta, F64 yellow, F64 key ) -> RGB`

`static` 

Convert a CMYK color back to the RGB color space. Each RGB channel is computed by applying both the color ink density and the black ink density to the maximum intensity of 255.

**Parameters**:

- `cyan` (`F64`)
- `The cyan ink density from 0.0 to 1.0.`
- `magenta` (`F64`)
- `The magenta ink density from 0.0 to 1.0.`
- `yellow` (`F64`)
- `The yellow ink density from 0.0 to 1.0.`
- `key` (`F64`)
- `The black ink density from 0.0 to 1.0.`

**Returns**: — RGB:
The equivalent color in the RGB color space with rounded integer channels.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function hexToHsl( I32 hex ) -> HSL`

`static` 

Convert a packed hexadecimal color to HSL by first extracting the RGB channels and then applying the RGB-to-HSL conversion.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value in 0xRRGGBB format.`

**Returns**: `HSL` — The equivalent color in the HSL color space.

#### `function hslToHex( F64 hue, F64 saturation, F64 lightness ) -> I32`

`static` 

Convert an HSL color to a packed hexadecimal integer by first converting to RGB and then packing the channels.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `lightness` (`F64`)
- `The lightness from 0.0 to 1.0.`

**Returns**: `I32` — The packed 24-bit hex color value.

#### `function hexToHsv( I32 hex ) -> HSV`

`static` 

Convert a packed hexadecimal color to HSV by first extracting the RGB channels and then applying the RGB-to-HSV conversion.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value in 0xRRGGBB format.`

**Returns**: `HSV` — The equivalent color in the HSV color space.

#### `function hsvToHex( F64 hue, F64 saturation, F64 value ) -> I32`

`static` 

Convert an HSV color to a packed hexadecimal integer by first converting to RGB and then packing the channels.

**Parameters**:

- `hue` (`F64`)
- `The hue angle in degrees from 0.0 to 360.0.`
- `saturation` (`F64`)
- `The saturation from 0.0 to 1.0.`
- `value` (`F64`)
- `The value` (`brightness`)

**Returns**: `I32` — The packed 24-bit hex color value.

#### `function hexToCmyk( I32 hex ) -> CMYK`

`static` 

Convert a packed hexadecimal color to CMYK by first extracting the RGB channels and then applying the RGB-to-CMYK conversion.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value in 0xRRGGBB format.`

**Returns**: `CMYK` — The equivalent color in the CMYK color space.

#### `function cmykToHex( F64 cyan, F64 magenta, F64 yellow, F64 key ) -> I32`

`static` 

Convert a CMYK color to a packed hexadecimal integer by first converting to RGB and then packing the channels.

**Parameters**:

- `cyan` (`F64`)
- `The cyan ink density from 0.0 to 1.0.`
- `magenta` (`F64`)
- `The magenta ink density from 0.0 to 1.0.`
- `yellow` (`F64`)
- `The yellow ink density from 0.0 to 1.0.`
- `key` (`F64`)
- `The black ink density from 0.0 to 1.0.`

**Returns**: `I32` — The packed 24-bit hex color value.

#### `function minThree( F64 a, F64 b, F64 c ) -> F64`

`static` 

Return the minimum of three floating-point values.

**Parameters**:

- `a` (`F64`)
- `The first value.`
- `b` (`F64`)
- `The second value.`
- `c` (`F64`)
- `The third value.`

**Returns**: — F64:
The smallest of the three values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function maxThree( F64 a, F64 b, F64 c ) -> F64`

`static` 

Return the maximum of three floating-point values.

**Parameters**:

- `a` (`F64`)
- `The first value.`
- `b` (`F64`)
- `The second value.`
- `c` (`F64`)
- `The third value.`

**Returns**: — F64:
The largest of the three values.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

