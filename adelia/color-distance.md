# uranite.adelia.color-distance

## Table of Contents

- [Imports](#imports)
- [class `ColorDistance`](#class-colordistance)
  - [`euclidean()`](#euclidean)
  - [`redmean()`](#redmean)
  - [`similarity()`](#similarity)

## Imports

- `uranite.adelia.rgb`
  - `RGB`
- `uranite.math.math`
  - `sqrt`

## class `ColorDistance`

Static utility class providing distance and similarity metrics for comparing two RGB colors. The Euclidean distance treats RGB as a 3D Cartesian space and computes the straight-line distance between two color points. The redmean algorithm improves perceptual accuracy by weighting the red, green, and blue channel differences according to the average red value of the two colors, approximating how the human eye perceives color differences more heavily in greens and reds than in blues.

### Methods

#### `function euclidean( self, RGB first, RGB second ) -> F64`

Compute the standard Euclidean distance between two colors in RGB space, treating each color as a point in a 3D coordinate system where the axes are red (0-255), green (0-255), and blue (0-255). The maximum possible distance is approximately 441.67, which is the distance between black (0,0,0) and white (255,255,255).

**Parameters**:

- `first` (`RGB`)
- `The first color to compare.`
- `second` (`RGB`)
- `The second color to compare.`

**Returns**: — F64:
The Euclidean distance from 0.0 (identical) to approximately 441.67.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function redmean( self, RGB first, RGB second ) -> F64`

Compute a perceptually weighted color distance using the redmean algorithm. This method weights the red, green, and blue channel differences based on the average red value of the two colors being compared, producing results that better approximate how the human eye perceives color differences. The formula increases the weight of the red channel for colors with high red content and increases the weight of the blue channel for colors with low red content. Green always receives the highest weight because the human eye is most sensitive to green variations.

**Parameters**:

- `first` (`RGB`)
- `The first color to compare.`
- `second` (`RGB`)
- `The second color to compare.`

**Returns**: — F64:
The weighted perceptual distance between the two colors.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function similarity( self, RGB first, RGB second ) -> F64`

Compute a similarity percentage between two colors using the redmean distance algorithm. Returns 0.0 when the colors are identical and 100.0 when they are maximally different. The result is normalized against the maximum possible redmean distance (the distance between black and white).

**Parameters**:

- `first` (`RGB`)
- `The first color to compare.`
- `second` (`RGB`)
- `The second color to compare.`

**Returns**: — F64:
A percentage from 0.0 (identical) to 100.0 (maximally different).

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

