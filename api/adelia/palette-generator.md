# uranite.adelia.palette-generator

## Table of Contents

- [Imports](#imports)
- [class `PaletteGenerator`](#class-palettegenerator)
  - [`complementary()`](#complementary)
  - [`triadic()`](#triadic)
  - [`analogous()`](#analogous)
  - [`splitComplementary()`](#splitComplementary)
  - [`tints()`](#tints)
  - [`shades()`](#shades)
  - [`monochromatic()`](#monochromatic)

## Imports

- `uranite.adelia.color`
  - `Color`
- `uranite.adelia.color-converter`
  - `ColorConverter`
- `uranite.adelia.color-palette`
  - `ColorPalette`
- `uranite.adelia.hsl`
  - `HSL`
- `uranite.math.math`
  - `fmod`

## class `PaletteGenerator`

Static utility class for generating harmonious color palettes from a single base color. All palette generation algorithms operate in HSL color space because hue rotation around the color wheel naturally produces aesthetically pleasing color combinations. The generator supports six standard palette types: complementary (opposite on the wheel), triadic (equilateral triangle), analogous (adjacent hues), split-complementary (flanking the complement), tints (lighter variations), and shades (darker variations). Each method returns a named ColorPalette containing the base color and the generated harmony colors.

### Methods

#### `function complementary( I32 baseHex ) -> ColorPalette`

`static` 

Generate a complementary palette containing the base color and its complement (hue rotated 180 degrees). Complementary colors provide maximum contrast and visual tension, making them ideal for call-to-action elements and attention-grabbing designs.

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`

**Returns**: — ColorPalette:
A palette named "Complementary" with 2 colors.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function triadic( I32 baseHex ) -> ColorPalette`

`static` 

Generate a triadic palette containing the base color and two colors at 120 and 240 degrees around the color wheel. Triadic palettes offer vibrant contrast while maintaining color harmony, forming an equilateral triangle on the color wheel.

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`

**Returns**: — ColorPalette:
A palette named "Triadic" with 3 colors.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function analogous( I32 baseHex ) -> ColorPalette`

`static` 

Generate an analogous palette containing the base color and two adjacent colors at +30 and -30 degrees on the color wheel. Analogous palettes create a sense of unity and harmony because the colors share similar underlying hues, making them ideal for cohesive, soothing designs.

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`

**Returns**: — ColorPalette:
A palette named "Analogous" with 3 colors.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function splitComplementary( I32 baseHex ) -> ColorPalette`

`static` 

Generate a split-complementary palette containing the base color and two colors flanking the complement at 150 and 210 degrees. This palette type provides strong visual contrast similar to complementary but with less tension, as the two accent colors are near-complements rather than direct opposites.

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`

**Returns**: — ColorPalette:
A palette named "Split Complementary" with 3 colors.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function tints( I32 baseHex, I64 count ) -> ColorPalette`

`static` 

Generate a palette of progressively lighter tints of the base color by increasing the lightness component in equal steps toward 1.0 (white) while preserving the hue and saturation. Tints are created by mixing a color with white.

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`
- `count` (`I64`)
- `The number of tints to generate` (`not including the base color`)

**Returns**: — ColorPalette:
A palette named "Tints" with count+1 colors from base to lightest.

**Complexity**:
- Time: `O(count)`
- Space: `O(count)`

#### `function shades( I32 baseHex, I64 count ) -> ColorPalette`

`static` 

Generate a palette of progressively darker shades of the base color by decreasing the lightness component in equal steps toward 0.0 (black) while preserving the hue and saturation. Shades are created by mixing a color with black.

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`
- `count` (`I64`)
- `The number of shades to generate` (`not including the base color`)

**Returns**: — ColorPalette:
A palette named "Shades" with count+1 colors from base to darkest.

**Complexity**:
- Time: `O(count)`
- Space: `O(count)`

#### `function monochromatic( I32 baseHex, I64 count ) -> ColorPalette`

`static` 

Generate a monochromatic palette containing tints and shades of the base color, arranged from darkest to lightest with the base color in the middle. The total palette size is count*2 + 1 (count shades + base + count tints).

**Parameters**:

- `baseHex` (`I32`)
- `The packed 24-bit hex value of the base color.`
- `count` (`I64`)
- `The number of shades and tints on each side of the base color.`

**Returns**: — ColorPalette:
A palette named "Monochromatic" with 2*count+1 colors.

**Complexity**:
- Time: `O(count)`
- Space: `O(count)`

#### `function rotateHue( F64 hue, F64 degrees ) -> F64`

`static` 

Rotate a hue angle by the given number of degrees, wrapping around the 0-360 degree range. Handles both positive (clockwise) and negative (counter-clockwise) rotations correctly by adding 360 before applying fmod to ensure a positive result.

**Parameters**:

- `hue` (`F64`)
- `The starting hue angle in degrees.`
- `degrees` (`F64`)
- `The number of degrees to rotate` (`positive or negative`)

**Returns**: — F64:
The rotated hue angle, normalized to the range 0.0-360.0.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

