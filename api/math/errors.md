# uranite.math.errors

## Table of Contents

- [Imports](#imports)
- [class `ArithmeticError`](#class-arithmeticerror)
  - [`ArithmeticError()`](#ArithmeticError)
- [class `ZeroDivisionError`](#class-zerodivisionerror)
  - [`ZeroDivisionError()`](#ZeroDivisionError)
- [class `OverflowError`](#class-overflowerror)
  - [`OverflowError()`](#OverflowError)
- [class `UnderflowError`](#class-underflowerror)
  - [`UnderflowError()`](#UnderflowError)

## Imports

- `uranite.errors.error`
  - `Error`
- `uranite.errors.throwable`
  - `Throwable`

## class `ArithmeticError`

**Extends**: `Error`

Base class for errors arising from failed arithmetic operations.

Subclasses identify specific failure modes: division by zero, integer overflow, and integer underflow. The compiler inserts runtime checks that raise these errors automatically.

### Methods

#### `function ArithmeticError( self, String message, I64 code, ?Throwable previous ) -> Void`

## class `ZeroDivisionError`

**Extends**: `ArithmeticError` → `Error`

Raised when a division or modulo operation has a zero divisor.

The compiler inserts a zero-check before every integer and float division and modulo operation. When the right operand is zero, this error is raised instead of producing undefined behavior.

### Methods

#### `function ZeroDivisionError( self, String message, I64 code, ?Throwable previous ) -> Void`

## class `OverflowError`

**Extends**: `ArithmeticError` → `Error`

Raised when an integer arithmetic operation exceeds the maximum representable value for the result type.

The compiler uses hardware overflow detection to check addition, subtraction, and multiplication on signed integer types. When overflow is detected, this error is raised instead of silently wrapping the result.

### Methods

#### `function OverflowError( self, String message, I64 code, ?Throwable previous ) -> Void`

## class `UnderflowError`

**Extends**: `ArithmeticError` → `Error`

Raised when an integer arithmetic operation falls below the minimum representable value for the result type.

Applies to signed integer subtraction and other operations that can produce a result smaller than the type's minimum value.

### Methods

#### `function UnderflowError( self, String message, I64 code, ?Throwable previous ) -> Void`

