# uranite.os.signal

## Table of Contents

- [Imports](#imports)
- [const `SIGINT`](#const-sigint)
- [const `SIGTERM`](#const-sigterm)
- [const `SIGKILL`](#const-sigkill)
- [const `SYS_RT_SIGACTION`](#const-sys-rt-sigaction)
- [const `SYS_MMAP`](#const-sys-mmap)
- [const `SA_RESTORER`](#const-sa-restorer)
- [const `SIGSET_SIZE`](#const-sigset-size)
- [const `SIGACTION_FIELDS`](#const-sigaction-fields)
- [const `MMAP_PAGE_SIZE`](#const-mmap-page-size)
- [const `PROT_RWX`](#const-prot-rwx)
- [const `MAP_PRIVATE_ANON`](#const-map-private-anon)
- [function `sigintHandler`](#function-siginthandler)
- [function `createSignalRestorer`](#function-createsignalrestorer)
- [function `registerSignalHandlers`](#function-registersignalhandlers)
  - [`registerSignalHandlers()`](#registerSignalHandlers)

## Imports

- `uranite.errors.interrupt`
  - `KeyboardInterruptError`
- `uranite.os.syscall.invoke`
  - `syscall4`
  - `syscall6`
- `uranite.os.syscall.result`
  - `SyscallResult`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.arch.native.memory`
  - `memoryToPtr`

## const `SIGINT`

## const `SIGTERM`

## const `SIGKILL`

## const `SYS_RT_SIGACTION`

## const `SYS_MMAP`

## const `SA_RESTORER`

## const `SIGSET_SIZE`

## const `SIGACTION_FIELDS`

## const `MMAP_PAGE_SIZE`

## const `PROT_RWX`

## const `MAP_PRIVATE_ANON`

## function `sigintHandler`

Signal handler invoked by the kernel on SIGINT delivery. Raises KeyboardInterruptError through the standard exception mechanism.

**Parameters**:

- `signum` (`I64`)
- `The signal number delivered by the kernel.`

### Methods

#### `function sigintHandler( I64 signum ) -> Void`

Signal handler invoked by the kernel on SIGINT delivery. Raises KeyboardInterruptError through the standard exception mechanism.

**Parameters**:

- `signum` (`I64`)
- `The signal number delivered by the kernel.`

## function `createSignalRestorer`

Allocate executable memory containing a minimal rt_sigreturn trampoline (mov $15, %rax; syscall). Returns the raw address of the trampoline code for use as sa_restorer in sigaction.

The trampoline is raw machine code without any Uranite function prologue, allowing libunwind to recognize the signal frame and unwind through it for exception propagation into try/except.

**Returns**: — The memory address of the executable rt_sigreturn trampoline,
or 0 if allocation failed.

### Methods

#### `function createSignalRestorer(  ) -> I64`

Allocate executable memory containing a minimal rt_sigreturn trampoline (mov $15, %rax; syscall). Returns the raw address of the trampoline code for use as sa_restorer in sigaction.

The trampoline is raw machine code without any Uranite function prologue, allowing libunwind to recognize the signal frame and unwind through it for exception propagation into try/except.

**Returns**: — The memory address of the executable rt_sigreturn trampoline,
or 0 if allocation failed.

## function `registerSignalHandlers`

Register the default signal handlers for the Uranite runtime. Installs a SIGINT handler that raises KeyboardInterruptError. Called automatically at program startup before user code executes.

The sigaction struct is laid out to match the kernel ABI on x86_64: offset 0: sa_handler  (function pointer, 8 bytes) offset 1: sa_flags    (SA_RESTORER, 8 bytes) offset 2: sa_restorer (restorer function pointer, 8 bytes) offset 3: sa_mask     (empty signal mask, 8 bytes)

### Methods

#### `function registerSignalHandlers(  ) -> Void`

Register the default signal handlers for the Uranite runtime. Installs a SIGINT handler that raises KeyboardInterruptError. Called automatically at program startup before user code executes.

The sigaction struct is laid out to match the kernel ABI on x86_64: offset 0: sa_handler  (function pointer, 8 bytes) offset 1: sa_flags    (SA_RESTORER, 8 bytes) offset 2: sa_restorer (restorer function pointer, 8 bytes) offset 3: sa_mask     (empty signal mask, 8 bytes)

