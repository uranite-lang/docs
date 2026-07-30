# uranite.os.syscall.number

## Table of Contents

- [enum `SyscallNumber`](#enum-syscallnumber)

## enum `SyscallNumber`

Syscall dispatch identifiers for the Uranite kernel. Each value is placed in RAX before executing the syscall instruction. On x86_64, arguments are passed in RDI, RSI, RDX, R10 (not RCX, which is clobbered), R8, and R9. The return value is placed in RAX, where negative values indicate error codes. On RISC-V (future), a7 holds the syscall number, a0-a5 hold arguments, a0 holds the return value, and the ecall instruction is used. Numbers are grouped by category with gaps reserved for future expansion.

