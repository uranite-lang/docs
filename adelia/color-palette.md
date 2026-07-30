# uranite.adelia.color-palette

## Table of Contents

- [Imports](#imports)
- [class `ColorPalette`](#class-colorpalette)
  - [`ColorPalette()`](#ColorPalette)
  - [`addColor()`](#addColor)
  - [`get()`](#get)
  - [`length()`](#length)
  - [`toString()`](#toString)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.language.string`
  - `String`

## class `ColorPalette`

A named collection of hexadecimal color values representing a themed color palette. Palettes are used to group harmonious colors together under a descriptive name, such as "Tropical Daydream" or "Complementary of Teal". Colors are stored as packed 24-bit I32 hex values in an ArrayList, typically containing between 2 and 6 colors. The palette can be built incrementally using the addColor method which returns self for method chaining.

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `String` | public |
| `colors` | `ArrayList<I32>` | protect |

### Methods

#### `function ColorPalette( self, String name ) -> Void`

Construct an empty color palette with the given name and no colors. Colors can be added afterward using the addColor method.

**Parameters**:

- `name` (`String`)
- `The descriptive name for this palette.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function addColor( self, I32 hex ) -> ColorPalette`

Append a hex color value to this palette and return self for method chaining, allowing fluent palette construction such as palette.addColor(0xFF0000).addColor(0x00FF00).addColor(0x0000FF).

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value to add.`

**Returns**: `ColorPalette` — This palette instance for chaining.

#### `function get( self, I64 index ) -> I32`

Return the hex color value at the given index in this palette.

**Parameters**:

- `index` (`I64`)
- `The zero-based index of the color to retrieve.`

**Returns**: `I32` — The packed 24-bit hex color value at the given index.

#### `property length( self ) -> I64`

`property` 

Return the number of colors currently in this palette.

**Returns**: `I64` — The number of colors in the palette.

#### `function toString( self ) -> String`

Return a human-readable string representation of this palette showing the palette name followed by the hex values of all contained colors.

**Returns**: — String:
A string such as "Palette(Tropical Daydream, [3 colors])".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

