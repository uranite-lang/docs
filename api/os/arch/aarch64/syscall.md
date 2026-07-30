# uranite.os.arch.aarch64.syscall

## Table of Contents

- [Imports](#imports)
- [const `AT_FDCWD`](#const-at-fdcwd)
- [const `AT_SYMLINK_NOFOLLOW`](#const-at-symlink-nofollow)
- [const `AT_REMOVEDIR`](#const-at-removedir)
- [function `syscall0`](#function-syscall0)
  - [`syscall0()`](#syscall0)
- [function `syscall1`](#function-syscall1)
  - [`syscall1()`](#syscall1)
- [function `syscall2`](#function-syscall2)
  - [`syscall2()`](#syscall2)
- [function `syscall3`](#function-syscall3)
  - [`syscall3()`](#syscall3)
- [function `syscall4`](#function-syscall4)
  - [`syscall4()`](#syscall4)
- [function `syscall5`](#function-syscall5)
  - [`syscall5()`](#syscall5)
- [function `syscall6`](#function-syscall6)
  - [`syscall6()`](#syscall6)
- [const `SYS_READ`](#const-sys-read)
- [const `SYS_WRITE`](#const-sys-write)
- [const `SYS_MMAP`](#const-sys-mmap)
- [const `SYS_MUNMAP`](#const-sys-munmap)
- [const `SYS_CLOSE`](#const-sys-close)
- [const `SYS_LSEEK`](#const-sys-lseek)
- [const `SYS_FSTAT`](#const-sys-fstat)
- [const `SYS_FTRUNCATE`](#const-sys-ftruncate)
- [const `SYS_FSYNC`](#const-sys-fsync)
- [const `SYS_FCHMOD`](#const-sys-fchmod)
- [const `SYS_FCHOWN`](#const-sys-fchown)
- [const `SYS_GETCWD`](#const-sys-getcwd)
- [const `SYS_CHDIR`](#const-sys-chdir)
- [const `SYS_GETDENTS64`](#const-sys-getdents64)
- [const `SYS_STATFS`](#const-sys-statfs)
- [const `SYS_FLOCK`](#const-sys-flock)
- [const `SYS_IOCTL`](#const-sys-ioctl)
- [const `SYS_PIPE2`](#const-sys-pipe2)
- [const `SYS_OPENAT`](#const-sys-openat)
- [const `SYS_NEWFSTATAT`](#const-sys-newfstatat)
- [const `SYS_FACCESSAT`](#const-sys-faccessat)
- [const `SYS_MKDIRAT`](#const-sys-mkdirat)
- [const `SYS_UNLINKAT`](#const-sys-unlinkat)
- [const `SYS_LINKAT`](#const-sys-linkat)
- [const `SYS_SYMLINKAT`](#const-sys-symlinkat)
- [const `SYS_READLINKAT`](#const-sys-readlinkat)
- [const `SYS_FCHMODAT`](#const-sys-fchmodat)
- [const `SYS_FCHOWNAT`](#const-sys-fchownat)
- [const `SYS_RENAMEAT`](#const-sys-renameat)
- [const `SYS_OPEN`](#const-sys-open)
- [const `SYS_STAT`](#const-sys-stat)
- [const `SYS_LSTAT`](#const-sys-lstat)
- [const `SYS_ACCESS`](#const-sys-access)
- [const `SYS_MKDIR`](#const-sys-mkdir)
- [const `SYS_RMDIR`](#const-sys-rmdir)
- [const `SYS_UNLINK`](#const-sys-unlink)
- [const `SYS_LINK`](#const-sys-link)
- [const `SYS_SYMLINK`](#const-sys-symlink)
- [const `SYS_READLINK`](#const-sys-readlink)
- [const `SYS_CHMOD`](#const-sys-chmod)
- [const `SYS_CHOWN`](#const-sys-chown)
- [const `SYS_RENAME`](#const-sys-rename)
- [function `scOpen`](#function-scopen)
  - [`scOpen()`](#scOpen)
- [function `scStat`](#function-scstat)
  - [`scStat()`](#scStat)
- [function `scLstat`](#function-sclstat)
  - [`scLstat()`](#scLstat)
- [function `scAccess`](#function-scaccess)
  - [`scAccess()`](#scAccess)
- [function `scMkdir`](#function-scmkdir)
  - [`scMkdir()`](#scMkdir)
- [function `scRmdir`](#function-scrmdir)
  - [`scRmdir()`](#scRmdir)
- [function `scUnlink`](#function-scunlink)
  - [`scUnlink()`](#scUnlink)
- [function `scRename`](#function-screname)
  - [`scRename()`](#scRename)
- [function `scLink`](#function-sclink)
  - [`scLink()`](#scLink)
- [function `scSymlink`](#function-scsymlink)
  - [`scSymlink()`](#scSymlink)
- [function `scReadlink`](#function-screadlink)
  - [`scReadlink()`](#scReadlink)
- [function `scChmod`](#function-scchmod)
  - [`scChmod()`](#scChmod)
- [function `scChown`](#function-scchown)
  - [`scChown()`](#scChown)

## Imports

- `uranite.os.syscall.result`
  - `SyscallResult`

## const `AT_FDCWD`

Pass as the dirfd argument so *at calls resolve paths against the CWD.

## const `AT_SYMLINK_NOFOLLOW`

newfstatat flag (0x100): do not dereference a final symbolic link.

## const `AT_REMOVEDIR`

unlinkat flag (0x200): remove a directory instead of a file.

## function `syscall0`

### Methods

#### `function syscall0( I64 number ) -> SyscallResult`

## function `syscall1`

### Methods

#### `function syscall1( I64 number, I64 arg1 ) -> SyscallResult`

## function `syscall2`

### Methods

#### `function syscall2( I64 number, I64 arg1, I64 arg2 ) -> SyscallResult`

## function `syscall3`

### Methods

#### `function syscall3( I64 number, I64 arg1, I64 arg2, I64 arg3 ) -> SyscallResult`

## function `syscall4`

### Methods

#### `function syscall4( I64 number, I64 arg1, I64 arg2, I64 arg3, I64 arg4 ) -> SyscallResult`

## function `syscall5`

### Methods

#### `function syscall5( I64 number, I64 arg1, I64 arg2, I64 arg3, I64 arg4, I64 arg5 ) -> SyscallResult`

## function `syscall6`

### Methods

#### `function syscall6( I64 number, I64 arg1, I64 arg2, I64 arg3, I64 arg4, I64 arg5, I64 arg6 ) -> SyscallResult`

## const `SYS_READ`

## const `SYS_WRITE`

## const `SYS_MMAP`

## const `SYS_MUNMAP`

## const `SYS_CLOSE`

## const `SYS_LSEEK`

## const `SYS_FSTAT`

## const `SYS_FTRUNCATE`

## const `SYS_FSYNC`

## const `SYS_FCHMOD`

## const `SYS_FCHOWN`

## const `SYS_GETCWD`

## const `SYS_CHDIR`

## const `SYS_GETDENTS64`

## const `SYS_STATFS`

## const `SYS_FLOCK`

## const `SYS_IOCTL`

## const `SYS_PIPE2`

## const `SYS_OPENAT`

## const `SYS_NEWFSTATAT`

## const `SYS_FACCESSAT`

## const `SYS_MKDIRAT`

## const `SYS_UNLINKAT`

## const `SYS_LINKAT`

## const `SYS_SYMLINKAT`

## const `SYS_READLINKAT`

## const `SYS_FCHMODAT`

## const `SYS_FCHOWNAT`

## const `SYS_RENAMEAT`

## const `SYS_OPEN`

## const `SYS_STAT`

## const `SYS_LSTAT`

## const `SYS_ACCESS`

## const `SYS_MKDIR`

## const `SYS_RMDIR`

## const `SYS_UNLINK`

## const `SYS_LINK`

## const `SYS_SYMLINK`

## const `SYS_READLINK`

## const `SYS_CHMOD`

## const `SYS_CHOWN`

## const `SYS_RENAME`

## function `scOpen`

### Methods

#### `function scOpen( I64 pathPtr, I64 flags, I64 mode ) -> SyscallResult`

## function `scStat`

### Methods

#### `function scStat( I64 pathPtr, I64 statBufPtr ) -> SyscallResult`

## function `scLstat`

### Methods

#### `function scLstat( I64 pathPtr, I64 statBufPtr ) -> SyscallResult`

## function `scAccess`

### Methods

#### `function scAccess( I64 pathPtr, I64 mode ) -> SyscallResult`

## function `scMkdir`

### Methods

#### `function scMkdir( I64 pathPtr, I64 mode ) -> SyscallResult`

## function `scRmdir`

### Methods

#### `function scRmdir( I64 pathPtr ) -> SyscallResult`

## function `scUnlink`

### Methods

#### `function scUnlink( I64 pathPtr ) -> SyscallResult`

## function `scRename`

### Methods

#### `function scRename( I64 oldPtr, I64 newPtr ) -> SyscallResult`

## function `scLink`

### Methods

#### `function scLink( I64 oldPtr, I64 newPtr ) -> SyscallResult`

## function `scSymlink`

### Methods

#### `function scSymlink( I64 targetPtr, I64 linkPtr ) -> SyscallResult`

## function `scReadlink`

### Methods

#### `function scReadlink( I64 pathPtr, I64 bufPtr, I64 bufSize ) -> SyscallResult`

## function `scChmod`

### Methods

#### `function scChmod( I64 pathPtr, I64 mode ) -> SyscallResult`

## function `scChown`

### Methods

#### `function scChown( I64 pathPtr, I64 uid, I64 gid ) -> SyscallResult`

