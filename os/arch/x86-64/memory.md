# uranite.os.arch.x86-64.memory

## Table of Contents

- [Imports](#imports)
- [function `stringToPtr`](#function-stringtoptr)
  - [`stringToPtr()`](#stringToPtr)
- [function `memoryToPtr`](#function-memorytoptr)
  - [`memoryToPtr()`](#memoryToPtr)
- [function `ptrToString`](#function-ptrtostring)
  - [`ptrToString()`](#ptrToString)
- [function `readByteAt`](#function-readbyteat)
  - [`readByteAt()`](#readByteAt)
- [function `readI16At`](#function-readi16at)
  - [`readI16At()`](#readI16At)
- [function `readI32At`](#function-readi32at)
  - [`readI32At()`](#readI32At)
- [function `readI64At`](#function-readi64at)
  - [`readI64At()`](#readI64At)
- [function `writeByteAt`](#function-writebyteat)
  - [`writeByteAt()`](#writeByteAt)
- [function `writeI16At`](#function-writei16at)
  - [`writeI16At()`](#writeI16At)
- [function `writeI32At`](#function-writei32at)
  - [`writeI32At()`](#writeI32At)
- [function `writeI64At`](#function-writei64at)
  - [`writeI64At()`](#writeI64At)

## Imports

- `uranite.memory.memory`
  - `Memory`

## function `stringToPtr`

Convert a String value to its raw memory address by moving the underlying pointer into an integer register.

### Methods

#### `function stringToPtr( String content ) -> I64`

Convert a String value to its raw memory address by moving the underlying pointer into an integer register.

## function `memoryToPtr`

Convert a Memory<I64> value to its raw memory address by moving the underlying pointer into an integer register.

### Methods

#### `function memoryToPtr( Memory<I64> m ) -> I64`

Convert a Memory<I64> value to its raw memory address by moving the underlying pointer into an integer register.

## function `ptrToString`

Reinterpret a raw memory address as a String value (the inverse of stringToPtr) by moving the address into the result register.

### Methods

#### `function ptrToString( I64 addr ) -> String`

Reinterpret a raw memory address as a String value (the inverse of stringToPtr) by moving the address into the result register.

## function `readByteAt`

Read a single unsigned byte at addr+offset and zero-extend it to I64.

### Methods

#### `function readByteAt( I64 addr, I64 offset ) -> I64`

Read a single unsigned byte at addr+offset and zero-extend it to I64.

## function `readI16At`

Read an unsigned 16-bit integer at addr+offset and zero-extend it to I64.

### Methods

#### `function readI16At( I64 addr, I64 offset ) -> I64`

Read an unsigned 16-bit integer at addr+offset and zero-extend it to I64.

## function `readI32At`

Read a 32-bit integer at addr+offset and zero-extend it to I64.

### Methods

#### `function readI32At( I64 addr, I64 offset ) -> I64`

Read a 32-bit integer at addr+offset and zero-extend it to I64.

## function `readI64At`

Read a 64-bit integer at addr+offset.

### Methods

#### `function readI64At( I64 addr, I64 offset ) -> I64`

Read a 64-bit integer at addr+offset.

## function `writeByteAt`

Write the low byte of val to memory at addr+offset.

### Methods

#### `function writeByteAt( I64 addr, I64 offset, I64 val ) -> Void`

Write the low byte of val to memory at addr+offset.

## function `writeI16At`

Write the low 16 bits of val to memory at addr+offset.

### Methods

#### `function writeI16At( I64 addr, I64 offset, I64 val ) -> Void`

Write the low 16 bits of val to memory at addr+offset.

## function `writeI32At`

Write the low 32 bits of val to memory at addr+offset.

### Methods

#### `function writeI32At( I64 addr, I64 offset, I64 val ) -> Void`

Write the low 32 bits of val to memory at addr+offset.

## function `writeI64At`

Write a full 64-bit value to memory at addr+offset.

### Methods

#### `function writeI64At( I64 addr, I64 offset, I64 val ) -> Void`

Write a full 64-bit value to memory at addr+offset.

