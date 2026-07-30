# uranite.os.syscall.error

## Table of Contents

- [enum `SyscallError`](#enum-syscallerror)

## enum `SyscallError`

Kernel error codes returned by system calls. By convention, a syscall returns a negative value in RAX on error, where the absolute value is the error code from this enum. For example, Permission (1) is returned as -1 in RAX. A return value of zero or positive indicates success.

