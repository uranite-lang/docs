# uranite.adelia.ansi-formatter

## Table of Contents

- [Imports](#imports)
- [class `AnsiFormatter`](#class-ansiformatter)
  - [`foreground()`](#foreground)
  - [`background()`](#background)
  - [`colorize()`](#colorize)
  - [`highlight()`](#highlight)
  - [`style()`](#style)
  - [`reset()`](#reset)
  - [`foregroundHex()`](#foregroundHex)
  - [`backgroundHex()`](#backgroundHex)
  - [`colorizeHex()`](#colorizeHex)

## Imports

- `uranite.language.i32`
  - `I32`
- `uranite.language.string`
  - `String`

## class `AnsiFormatter`

Static utility class for generating ANSI 24-bit true color escape sequences used to colorize text output in terminal emulators. The SGR (Select Graphic Rendition) escape sequence format uses the pattern ESC[38;2;R;G;Bm for foreground colors and ESC[48;2;R;G;Bm for background colors, where R, G, and B are decimal values from 0 to 255. The reset sequence ESC[0m restores the terminal to its default colors. All methods are static (no self parameter) and return formatted escape code strings.

### Methods

#### `function foreground( I32 red, I32 green, I32 blue ) -> String`

`static` 

Generate an ANSI escape sequence that sets the terminal foreground (text) color to the specified RGB value using the 24-bit true color format.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `String` — The ANSI escape sequence string such as "\x1b[38;2;255;0;0m".

#### `function background( I32 red, I32 green, I32 blue ) -> String`

`static` 

Generate an ANSI escape sequence that sets the terminal background color to the specified RGB value using the 24-bit true color format.

**Parameters**:

- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `String` — The ANSI escape sequence string such as "\x1b[48;2;0;75;73m".

#### `function colorize( String text, I32 red, I32 green, I32 blue ) -> String`

`static` 

Wrap the given text string with a foreground color escape sequence and an automatic reset sequence, so only the specified text is colored and the terminal returns to default colors afterward.

**Parameters**:

- `text` (`String`)
- `The text to colorize.`
- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `String` — The text wrapped with foreground color and reset escape sequences.

#### `function highlight( String text, I32 red, I32 green, I32 blue ) -> String`

`static` 

Wrap the given text string with a background color escape sequence and an automatic reset sequence, highlighting the text with the specified background color.

**Parameters**:

- `text` (`String`)
- `The text to highlight.`
- `red` (`I32`)
- `The red channel value from 0 to 255.`
- `green` (`I32`)
- `The green channel value from 0 to 255.`
- `blue` (`I32`)
- `The blue channel value from 0 to 255.`

**Returns**: `String` — The text wrapped with background color and reset escape sequences.

#### `function style( String text, I32 fgRed, I32 fgGreen, I32 fgBlue, I32 bgRed, I32 bgGreen, I32 bgBlue ) -> String`

`static` 

Wrap the given text string with both a foreground and background color escape sequence, followed by an automatic reset. This applies both text color and highlight color simultaneously.

**Parameters**:

- `text` (`String`)
- `The text to style.`
- `fgRed` (`I32`)
- `The foreground red channel value from 0 to 255.`
- `fgGreen` (`I32`)
- `The foreground green channel value from 0 to 255.`
- `fgBlue` (`I32`)
- `The foreground blue channel value from 0 to 255.`
- `bgRed` (`I32`)
- `The background red channel value from 0 to 255.`
- `bgGreen` (`I32`)
- `The background green channel value from 0 to 255.`
- `bgBlue` (`I32`)
- `The background blue channel value from 0 to 255.`

**Returns**: `String` — The text wrapped with both foreground and background color sequences.

#### `function reset(  ) -> String`

`static` 

Return the ANSI reset escape sequence that restores the terminal to its default foreground and background colors and removes any text attributes.

**Returns**: `String` — The reset escape sequence "\x1b[0m".

#### `function foregroundHex( I32 hex ) -> String`

`static` 

Generate a foreground color escape sequence from a packed 24-bit hex value.

**Parameters**:

- `hex` (`I32`)
- `The packed hex color value in 0xRRGGBB format.`

**Returns**: — String:
The ANSI foreground escape sequence for the given hex color.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function backgroundHex( I32 hex ) -> String`

`static` 

Generate a background color escape sequence from a packed 24-bit hex value.

**Parameters**:

- `hex` (`I32`)
- `The packed hex color value in 0xRRGGBB format.`

**Returns**: — String:
The ANSI background escape sequence for the given hex color.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function colorizeHex( String text, I32 hex ) -> String`

`static` 

Wrap text with a foreground color and reset using a packed 24-bit hex value.

**Parameters**:

- `text` (`String`)
- `The text to colorize.`
- `hex` (`I32`)
- `The packed hex color value in 0xRRGGBB format.`

**Returns**: — String:
The text wrapped with foreground color and reset escape sequences.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

