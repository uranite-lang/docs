# uranite.math.math

## Table of Contents

- [Imports](#imports)
- [const `PI`](#const-pi)
- [const `E`](#const-e)
- [const `TAU`](#const-tau)
- [const `SQRT2`](#const-sqrt2)
- [const `LN2`](#const-ln2)
- [const `LN10`](#const-ln10)
- [const `I64_MAX`](#const-i64-max)
- [const `I64_MIN`](#const-i64-min)
- [const `POSITIVE_INF`](#const-positive-inf)
- [const `NEGATIVE_INF`](#const-negative-inf)
- [function `abs`](#function-abs)
  - [`abs()`](#abs)
- [function `absI64`](#function-absi64)
  - [`absI64()`](#absI64)
- [function `sqrt`](#function-sqrt)
  - [`sqrt()`](#sqrt)
- [function `truncate`](#function-truncate)
  - [`truncate()`](#truncate)
- [function `floor`](#function-floor)
  - [`floor()`](#floor)
- [function `ceil`](#function-ceil)
  - [`ceil()`](#ceil)
- [function `round`](#function-round)
  - [`round()`](#round)
- [function `fmod`](#function-fmod)
  - [`fmod()`](#fmod)
- [function `fabs`](#function-fabs)
  - [`fabs()`](#fabs)
- [function `pow`](#function-pow)
  - [`pow()`](#pow)
- [function `powF64`](#function-powf64)
  - [`powF64()`](#powF64)
- [function `ln`](#function-ln)
  - [`ln()`](#ln)
- [function `expF64`](#function-expf64)
  - [`expF64()`](#expF64)
- [function `log10`](#function-log10)
  - [`log10()`](#log10)
- [function `log2`](#function-log2)
  - [`log2()`](#log2)
- [function `min`](#function-min)
  - [`min()`](#min)
- [function `max`](#function-max)
  - [`max()`](#max)
- [function `minF64`](#function-minf64)
  - [`minF64()`](#minF64)
- [function `maxF64`](#function-maxf64)
  - [`maxF64()`](#maxF64)
- [function `clamp`](#function-clamp)
  - [`clamp()`](#clamp)
- [function `clampF64`](#function-clampf64)
  - [`clampF64()`](#clampF64)
- [function `sign`](#function-sign)
  - [`sign()`](#sign)
- [function `signF64`](#function-signf64)
  - [`signF64()`](#signF64)
- [function `gcd`](#function-gcd)
  - [`gcd()`](#gcd)
- [function `lcm`](#function-lcm)
  - [`lcm()`](#lcm)
- [function `powI64`](#function-powi64)
  - [`powI64()`](#powI64)

## Imports

- `uranite.errors.value`
  - `ValueError`
- `uranite.math.errors`
  - `ArithmeticError`

## const `PI`

## const `E`

## const `TAU`

## const `SQRT2`

## const `LN2`

## const `LN10`

## const `I64_MAX`

## const `I64_MIN`

## const `POSITIVE_INF`

## const `NEGATIVE_INF`

## function `abs`

Compute the absolute value of a double-precision floating-point number. Returns the magnitude of value without regard to its sign.

### Methods

#### `function abs( F64 value ) -> F64`

Compute the absolute value of a double-precision floating-point number. Returns the magnitude of value without regard to its sign.

## function `absI64`

Compute the absolute value of a 64-bit signed integer. Returns the magnitude of value without regard to its sign.

### Methods

#### `function absI64( I64 value ) -> I64`

Compute the absolute value of a 64-bit signed integer. Returns the magnitude of value without regard to its sign.

## function `sqrt`

Compute the square root of a non-negative double-precision value using the Newton-Raphson iterative method. Converges to full double precision within 20 iterations. Returns 0.0 for negative inputs.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function sqrt( F64 value ) -> F64`

Compute the square root of a non-negative double-precision value using the Newton-Raphson iterative method. Converges to full double precision within 20 iterations. Returns 0.0 for negative inputs.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `truncate`

Truncate a double-precision value toward zero, removing the fractional part via narrowing conversion to I64 and back to F64.

### Methods

#### `function truncate( F64 value ) -> F64`

Truncate a double-precision value toward zero, removing the fractional part via narrowing conversion to I64 and back to F64.

## function `floor`

Round a double-precision value downward to the nearest integer not greater than value.

### Methods

#### `function floor( F64 value ) -> F64`

Round a double-precision value downward to the nearest integer not greater than value.

## function `ceil`

Round a double-precision value upward to the nearest integer not less than value.

### Methods

#### `function ceil( F64 value ) -> F64`

Round a double-precision value upward to the nearest integer not less than value.

## function `round`

Round a double-precision value to the nearest integer, with halfway cases rounded away from zero.

### Methods

#### `function round( F64 value ) -> F64`

Round a double-precision value to the nearest integer, with halfway cases rounded away from zero.

## function `fmod`

Compute the floating-point remainder of dividend / divisor, where the result has the same sign as dividend. Returns 0.0 if divisor is zero.

### Methods

#### `function fmod( F64 dividend, F64 divisor ) -> F64`

Compute the floating-point remainder of dividend / divisor, where the result has the same sign as dividend. Returns 0.0 if divisor is zero.

## function `fabs`

Compute the absolute value of a double-precision floating-point value. Alias for abs() following the C standard library naming convention.

### Methods

#### `function fabs( F64 value ) -> F64`

Compute the absolute value of a double-precision floating-point value. Alias for abs() following the C standard library naming convention.

## function `pow`

Compute base raised to an integer exponent using exponentiation by squaring. Handles negative exponents by inverting the result. This provides O(log n) performance for integer powers.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function pow( F64 base, I64 exponent ) -> F64`

Compute base raised to an integer exponent using exponentiation by squaring. Handles negative exponents by inverting the result. This provides O(log n) performance for integer powers.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `powF64`

Compute base raised to a floating-point exponent. For integer exponents delegates to the fast integer path. For fractional exponents, uses the identity base^exp = e^(exp * ln(base)) with iterative ln and exp approximations.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function powF64( F64 base, F64 exponent ) -> F64`

Compute base raised to a floating-point exponent. For integer exponents delegates to the fast integer path. For fractional exponents, uses the identity base^exp = e^(exp * ln(base)) with iterative ln and exp approximations.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `ln`

Compute the natural logarithm of value using the series expansion ln(x) = 2 * sum( ((x-1)/(x+1))^(2k+1) / (2k+1) ) for k=0..N. Reduces the argument to [1,2) range using the identity ln(x * 2^n) = ln(x) + n*ln(2) for faster convergence.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function ln( F64 value ) -> F64`

Compute the natural logarithm of value using the series expansion ln(x) = 2 * sum( ((x-1)/(x+1))^(2k+1) / (2k+1) ) for k=0..N. Reduces the argument to [1,2) range using the identity ln(x * 2^n) = ln(x) + n*ln(2) for faster convergence.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `expF64`

Compute e^value using the Taylor series expansion: e^x = sum( x^k / k! ) for k=0..N. Reduces the argument to a small range using the identity e^(n+f) = e^n * e^f for faster convergence.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function expF64( F64 value ) -> F64`

Compute e^value using the Taylor series expansion: e^x = sum( x^k / k! ) for k=0..N. Reduces the argument to a small range using the identity e^(n+f) = e^n * e^f for faster convergence.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `log10`

Compute the base-10 logarithm of value using the change of base formula log10(x) = ln(x) / ln(10).

### Methods

#### `function log10( F64 value ) -> F64`

Compute the base-10 logarithm of value using the change of base formula log10(x) = ln(x) / ln(10).

## function `log2`

Compute the base-2 logarithm of value using the change of base formula log2(x) = ln(x) / ln(2).

### Methods

#### `function log2( F64 value ) -> F64`

Compute the base-2 logarithm of value using the change of base formula log2(x) = ln(x) / ln(2).

## function `min`

Return the smaller of two 64-bit signed integer values.

### Methods

#### `function min( I64 first, I64 second ) -> I64`

Return the smaller of two 64-bit signed integer values.

## function `max`

Return the larger of two 64-bit signed integer values.

### Methods

#### `function max( I64 first, I64 second ) -> I64`

Return the larger of two 64-bit signed integer values.

## function `minF64`

Return the smaller of two double-precision floating-point values.

### Methods

#### `function minF64( F64 first, F64 second ) -> F64`

Return the smaller of two double-precision floating-point values.

## function `maxF64`

Return the larger of two double-precision floating-point values.

### Methods

#### `function maxF64( F64 first, F64 second ) -> F64`

Return the larger of two double-precision floating-point values.

## function `clamp`

Constrain a 64-bit integer value to the range [lower, upper]. Returns lower if value < lower, upper if value > upper, otherwise value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function clamp( I64 value, I64 lower, I64 upper ) -> I64`

Constrain a 64-bit integer value to the range [lower, upper]. Returns lower if value < lower, upper if value > upper, otherwise value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `clampF64`

Constrain a double-precision value to the range [lower, upper]. Returns lower if value < lower, upper if value > upper, otherwise value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function clampF64( F64 value, F64 lower, F64 upper ) -> F64`

Constrain a double-precision value to the range [lower, upper]. Returns lower if value < lower, upper if value > upper, otherwise value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `sign`

Return the sign of a 64-bit integer: -1 for negative, 0 for zero, 1 for positive.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function sign( I64 value ) -> I64`

Return the sign of a 64-bit integer: -1 for negative, 0 for zero, 1 for positive.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `signF64`

Return the sign of a double-precision value: -1.0 for negative, 0.0 for zero, 1.0 for positive.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function signF64( F64 value ) -> F64`

Return the sign of a double-precision value: -1.0 for negative, 0.0 for zero, 1.0 for positive.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `gcd`

Compute the greatest common divisor of two integers using the Euclidean algorithm. Returns 0 if both inputs are 0.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

### Methods

#### `function gcd( I64 first, I64 second ) -> I64`

Compute the greatest common divisor of two integers using the Euclidean algorithm. Returns 0 if both inputs are 0.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## function `lcm`

Compute the least common multiple of two integers using the identity lcm(a,b) = |a*b| / gcd(a,b). Returns 0 if either input is 0.

### Methods

#### `function lcm( I64 first, I64 second ) -> I64`

Compute the least common multiple of two integers using the identity lcm(a,b) = |a*b| / gcd(a,b). Returns 0 if either input is 0.

## function `powI64`

Integer exponentiation via repeated squaring.

Computes base raised to the power of exponent as a 64-bit integer.

**Parameters**:

- `base` (`I64`)
- `The base value.`
- `exponent` (`I64`)
- `The non-negative exponent.`

**Returns**: `I64` — The result of base^exponent.

**Raises**:

- `ValueError` → `Error` — If exponent is negative.

**Complexity**:
- Time: `O(log exponent)`
- Space: `O(1)`

### Methods

#### `function powI64( I64 base, I64 exponent ) -> I64`

Integer exponentiation via repeated squaring.

Computes base raised to the power of exponent as a 64-bit integer.

**Parameters**:

- `base` (`I64`)
- `The base value.`
- `exponent` (`I64`)
- `The non-negative exponent.`

**Returns**: `I64` — The result of base^exponent.

**Raises**:

- `ValueError` → `Error` — If exponent is negative.

**Complexity**:
- Time: `O(log exponent)`
- Space: `O(1)`

