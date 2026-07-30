# uranite.adelia.color-name-entry

## Table of Contents

- [Imports](#imports)
- [struct `ColorNameEntry`](#struct-colornameentry)
  - [`ColorNameEntry()`](#ColorNameEntry)
  - [`toString()`](#toString)

## Imports

- `uranite.language.string`
  - `String`

## struct `ColorNameEntry`

Associates a human-readable color name with its hexadecimal color value. For example, the name "Deep Jungle Green" is associated with the hex value 0x004B49. This struct serves as the fundamental data element in the color name database, enabling bidirectional lookups between color names and their numeric representations.

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `String` | public |
| `hex` | `I32` | public |

### Methods

#### `function ColorNameEntry( self, String name, I32 hex ) -> Void`

Construct a new color name entry binding the given name string to a hexadecimal color value.

**Parameters**:

- `name` (`String`)
- `The human-readable name for this color, such as "Alice Blue"`
- `or "Deep Jungle Green".`
- `hex` (`I32`)
- `The 24-bit hexadecimal color value where bits 16-23 are red,`
- `bits 8-15 are green, and bits 0-7 are blue.`

#### `function toString( self ) -> String`

Return a human-readable string representation of this color name entry in the format "ColorNameEntry(name, hex)" showing the name and its associated hexadecimal value.

**Returns**: — String:
A string representation such as "ColorNameEntry(Deep Jungle Green, 19273)".

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

