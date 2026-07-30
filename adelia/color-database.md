# uranite.adelia.color-database

## Table of Contents

- [Imports](#imports)
- [class `ColorDatabase`](#class-colordatabase)
  - [`ColorDatabase()`](#ColorDatabase)
  - [`closestName()`](#closestName)
  - [`closestEntry()`](#closestEntry)
  - [`nameForHex()`](#nameForHex)
  - [`hexForName()`](#hexForName)
  - [`count()`](#count)

## Imports

- `uranite.adelia.color-distance`
  - `ColorDistance`
- `uranite.adelia.color-name-entry`
  - `ColorNameEntry`
- `uranite.adelia.rgb`
  - `RGB`
- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.language.boolean`
  - `Boolean`
- `uranite.language.string`
  - `String`

## class `ColorDatabase`

In-memory database of named colors populated with the 140 standard CSS/HTML named colors plus additional distinctive named colors. All color data is stored inline in the constructor as sequential add operations on an ArrayList, requiring no external file or network access. The database provides exact lookup by hex code or name, and nearest-match lookup using the perceptual redmean distance algorithm to find the closest named color for any arbitrary hex value.

### Fields

| Name | Type | Access |
|------|------|--------|
| `entries` | `ArrayList<ColorNameEntry>` | protect |

### Methods

#### `function ColorDatabase( self ) -> Void`

Construct a new color database and populate it with all named colors. The database is split into two population phases: basic CSS colors and extended named colors. After construction, the database is ready for immediate lookup operations.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function populateBasicColors( self ) -> Void`

Populate the database with the 140 standard CSS/HTML named colors. These colors are defined by the CSS Color Module Level 4 specification and are universally supported by web browsers.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function populateExtendedColors( self ) -> Void`

Populate the database with additional distinctive named colors beyond the CSS standard set. These include colors commonly found on color reference sites like color-hex.com with well-known or evocative names.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function closestName( self, I32 hex ) -> String`

Find the named color with the smallest perceptual distance to the given hex value and return its name. Uses the redmean weighted distance algorithm for better perceptual accuracy. If the database is empty, returns an empty string.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value to find the closest name for.`

**Returns**: `String` — The name of the closest matching color in the database.

#### `function closestEntry( self, I32 hex ) -> ColorNameEntry`

Find the named color entry with the smallest perceptual distance to the given hex value and return the full ColorNameEntry. Iterates through all entries computing the redmean distance and tracks the minimum.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value to find the closest entry for.`

**Returns**: — ColorNameEntry:
The database entry with the smallest perceptual distance.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function nameForHex( self, I32 hex ) -> String`

Look up the exact name for a given hex color value. Performs a linear scan through the database for an exact hex match. Returns an empty string if no exact match is found.

**Parameters**:

- `hex` (`I32`)
- `The packed 24-bit hex color value to look up.`

**Returns**: — String:
The name of the color if an exact match exists, or "" if not found.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function hexForName( self, String name ) -> I32`

Look up the hex color value for a given color name. Performs a linear scan through the database for an exact string match. Returns -1 if no match is found. Name matching is case-sensitive.

**Parameters**:

- `name` (`String`)
- `The color name to look up, such as "Deep Jungle Green".`

**Returns**: — I32:
The hex value if found, or -1 if no color with that name exists.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `property count( self ) -> I64`

`property` 

Return the total number of named color entries in this database.

**Returns**: — I64:
The number of entries in the database.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

