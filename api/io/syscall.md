# uranite.io.syscall

## Table of Contents

- [Imports](#imports)
- [const `O_RDONLY`](#const-o-rdonly)
- [const `O_WRONLY`](#const-o-wronly)
- [const `O_RDWR`](#const-o-rdwr)
- [const `O_CREAT`](#const-o-creat)
- [const `O_EXCL`](#const-o-excl)
- [const `O_TRUNC`](#const-o-trunc)
- [const `O_APPEND`](#const-o-append)
- [const `O_CLOEXEC`](#const-o-cloexec)
- [const `O_DIRECTORY`](#const-o-directory)
- [const `SEEK_SET`](#const-seek-set)
- [const `SEEK_CUR`](#const-seek-cur)
- [const `SEEK_END`](#const-seek-end)
- [const `LOCK_SH`](#const-lock-sh)
- [const `LOCK_EX`](#const-lock-ex)
- [const `LOCK_UN`](#const-lock-un)
- [const `LOCK_NB`](#const-lock-nb)
- [const `F_OK`](#const-f-ok)
- [const `R_OK`](#const-r-ok)
- [const `W_OK`](#const-w-ok)
- [const `X_OK`](#const-x-ok)
- [const `S_IFMT`](#const-s-ifmt)
- [const `S_IFREG`](#const-s-ifreg)
- [const `S_IFDIR`](#const-s-ifdir)
- [const `S_IFLNK`](#const-s-iflnk)
- [const `S_IFIFO`](#const-s-ififo)
- [const `S_IFSOCK`](#const-s-ifsock)
- [const `S_IFCHR`](#const-s-ifchr)
- [const `S_IFBLK`](#const-s-ifblk)
- [const `TCGETS`](#const-tcgets)
- [const `TCSETS`](#const-tcsets)
- [const `TIOCGWINSZ`](#const-tiocgwinsz)
- [const `ECHO_FLAG`](#const-echo-flag)
- [const `ICANON_FLAG`](#const-icanon-flag)
- [const `ENOENT`](#const-enoent)
- [const `EACCES`](#const-eacces)
- [const `EEXIST`](#const-eexist)
- [const `EISDIR`](#const-eisdir)
- [const `ENOTDIR`](#const-enotdir)
- [const `ENOTEMPTY`](#const-enotempty)
- [const `EPIPE`](#const-epipe)
- [function `stringLen`](#function-stringlen)
  - [`stringLen()`](#stringLen)
- [function `raiseFromErrno`](#function-raisefromerrno)
  - [`raiseFromErrno()`](#raiseFromErrno)
- [function `sysOpen`](#function-sysopen)
  - [`sysOpen()`](#sysOpen)
- [function `sysClose`](#function-sysclose)
  - [`sysClose()`](#sysClose)
- [function `sysRead`](#function-sysread)
  - [`sysRead()`](#sysRead)
- [function `sysWrite`](#function-syswrite)
  - [`sysWrite()`](#sysWrite)
- [function `sysLseek`](#function-syslseek)
  - [`sysLseek()`](#sysLseek)
- [function `sysStat`](#function-sysstat)
  - [`sysStat()`](#sysStat)
- [function `sysFstat`](#function-sysfstat)
  - [`sysFstat()`](#sysFstat)
- [function `sysLstat`](#function-syslstat)
  - [`sysLstat()`](#sysLstat)
- [function `sysFtruncate`](#function-sysftruncate)
  - [`sysFtruncate()`](#sysFtruncate)
- [function `sysFsync`](#function-sysfsync)
  - [`sysFsync()`](#sysFsync)
- [function `sysMkdir`](#function-sysmkdir)
  - [`sysMkdir()`](#sysMkdir)
- [function `sysRmdir`](#function-sysrmdir)
  - [`sysRmdir()`](#sysRmdir)
- [function `sysUnlink`](#function-sysunlink)
  - [`sysUnlink()`](#sysUnlink)
- [function `sysRename`](#function-sysrename)
  - [`sysRename()`](#sysRename)
- [function `sysLink`](#function-syslink)
  - [`sysLink()`](#sysLink)
- [function `sysSymlink`](#function-syssymlink)
  - [`sysSymlink()`](#sysSymlink)
- [function `sysReadlink`](#function-sysreadlink)
  - [`sysReadlink()`](#sysReadlink)
- [function `sysChmod`](#function-syschmod)
  - [`sysChmod()`](#sysChmod)
- [function `sysFchmod`](#function-sysfchmod)
  - [`sysFchmod()`](#sysFchmod)
- [function `sysChown`](#function-syschown)
  - [`sysChown()`](#sysChown)
- [function `sysFchown`](#function-sysfchown)
  - [`sysFchown()`](#sysFchown)
- [function `sysAccess`](#function-sysaccess)
  - [`sysAccess()`](#sysAccess)
- [function `sysGetcwd`](#function-sysgetcwd)
  - [`sysGetcwd()`](#sysGetcwd)
- [function `sysChdir`](#function-syschdir)
  - [`sysChdir()`](#sysChdir)
- [function `sysGetdents64`](#function-sysgetdents64)
  - [`sysGetdents64()`](#sysGetdents64)
- [function `sysStatfs`](#function-sysstatfs)
  - [`sysStatfs()`](#sysStatfs)
- [function `sysFlock`](#function-sysflock)
  - [`sysFlock()`](#sysFlock)
- [function `sysIoctl`](#function-sysioctl)
  - [`sysIoctl()`](#sysIoctl)
- [function `sysPipe2`](#function-syspipe2)
  - [`sysPipe2()`](#sysPipe2)

## Imports

- `uranite.io.errors`
  - `BrokenPipeError`
  - `DirectoryNotEmptyError`
  - `FileExistsError`
  - `FileNotFoundError`
  - `IOError`
  - `IsADirectoryError`
  - `NotADirectoryError`
  - `PermissionError`
- `uranite.os.syscall.invoke`
  - `syscall1`
  - `syscall2`
  - `syscall3`
- `uranite.os.syscall.result`
  - `SyscallResult`
- `uranite.os.arch.native.memory`
  - `memoryToPtr`
  - `ptrToString`
  - `readByteAt`
  - `readI16At`
  - `readI32At`
  - `readI64At`
  - `stringToPtr`
  - `writeByteAt`
  - `writeI16At`
  - `writeI32At`
  - `writeI64At`
- `uranite.os.arch.native.syscall`
  - `SYS_ACCESS`
  - `SYS_CHDIR`
  - `SYS_CHMOD`
  - `SYS_CHOWN`
  - `SYS_CLOSE`
  - `SYS_FCHMOD`
  - `SYS_FCHOWN`
  - `SYS_FLOCK`
  - `SYS_FSTAT`
  - `SYS_FSYNC`
  - `SYS_FTRUNCATE`
  - `SYS_GETCWD`
  - `SYS_GETDENTS64`
  - `SYS_IOCTL`
  - `SYS_LINK`
  - `SYS_LSEEK`
  - `SYS_LSTAT`
  - `SYS_MKDIR`
  - `SYS_OPEN`
  - `SYS_PIPE2`
  - `SYS_READ`
  - `SYS_READLINK`
  - `SYS_RENAME`
  - `SYS_RMDIR`
  - `SYS_STAT`
  - `SYS_STATFS`
  - `SYS_SYMLINK`
  - `SYS_UNLINK`
  - `SYS_WRITE`
  - `scAccess`
  - `scChmod`
  - `scChown`
  - `scLink`
  - `scLstat`
  - `scMkdir`
  - `scOpen`
  - `scReadlink`
  - `scRename`
  - `scRmdir`
  - `scStat`
  - `scSymlink`
  - `scUnlink`

## const `O_RDONLY`

Open flag for read-only access.

## const `O_WRONLY`

Open flag for write-only access.

## const `O_RDWR`

Open flag for read-write access.

## const `O_CREAT`

Open flag to create the file if it does not exist.

## const `O_EXCL`

Open flag to fail if O_CREAT is set and the file already exists.

## const `O_TRUNC`

Open flag to truncate the file to zero length on open.

## const `O_APPEND`

Open flag to append writes to the end of the file.

## const `O_CLOEXEC`

Open flag to set close-on-exec for the file descriptor.

## const `O_DIRECTORY`

Open flag to fail if the path is not a directory.

## const `SEEK_SET`

Seek origin: absolute position from file start.

## const `SEEK_CUR`

Seek origin: relative to current position.

## const `SEEK_END`

Seek origin: relative to end of file.

## const `LOCK_SH`

File lock operation: shared (read) lock.

## const `LOCK_EX`

File lock operation: exclusive (write) lock.

## const `LOCK_UN`

File lock operation: unlock.

## const `LOCK_NB`

File lock modifier: non-blocking, can be OR'd with LOCK_SH or LOCK_EX.

## const `F_OK`

Access check mode: test for existence.

## const `R_OK`

Access check mode: test for read permission.

## const `W_OK`

Access check mode: test for write permission.

## const `X_OK`

Access check mode: test for execute permission.

## const `S_IFMT`

Bitmask for the file type field in st_mode (0xF000).

## const `S_IFREG`

File type constant for regular file (0x8000).

## const `S_IFDIR`

File type constant for directory (0x4000).

## const `S_IFLNK`

File type constant for symbolic link (0xA000).

## const `S_IFIFO`

File type constant for named pipe / FIFO (0x1000).

## const `S_IFSOCK`

File type constant for socket (0xC000).

## const `S_IFCHR`

File type constant for character device (0x2000).

## const `S_IFBLK`

File type constant for block device (0x6000).

## const `TCGETS`

Ioctl request code to get terminal attributes.

## const `TCSETS`

Ioctl request code to set terminal attributes.

## const `TIOCGWINSZ`

Ioctl request code to get terminal window size.

## const `ECHO_FLAG`

Terminal flag bit for echo mode.

## const `ICANON_FLAG`

Terminal flag bit for canonical (line-buffered) input mode.

## const `ENOENT`

Errno: no such file or directory.

## const `EACCES`

Errno: permission denied.

## const `EEXIST`

Errno: file already exists.

## const `EISDIR`

Errno: is a directory.

## const `ENOTDIR`

Errno: not a directory.

## const `ENOTEMPTY`

Errno: directory not empty.

## const `EPIPE`

Errno: broken pipe.

## function `stringLen`

Compute the length of a null-terminated string by scanning for the zero byte.

**Parameters**:

- `content` (`String`)
- `The string to measure.`

**Returns**: — I64:
The number of bytes before the null terminator.

**Complexity**:
- Time: `O(n) where n is the string length.`

### Methods

#### `function stringLen( String content ) -> I64`

Compute the length of a null-terminated string by scanning for the zero byte.

**Parameters**:

- `content` (`String`)
- `The string to measure.`

**Returns**: — I64:
The number of bytes before the null terminator.

**Complexity**:
- Time: `O(n) where n is the string length.`

## function `raiseFromErrno`

Translate a Linux errno value into the appropriate Uranite IOError subclass and raise it. Falls back to a generic IOError for unrecognized errno values.

**Parameters**:

- `errCode` (`I64`)
- `The Linux errno value returned by a failed syscall.`
- `context` (`String`)
- `A human-readable description of the operation that failed.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function raiseFromErrno( I64 errCode, String context ) -> Void`

Translate a Linux errno value into the appropriate Uranite IOError subclass and raise it. Falls back to a generic IOError for unrecognized errno values.

**Parameters**:

- `errCode` (`I64`)
- `The Linux errno value returned by a failed syscall.`
- `context` (`String`)
- `A human-readable description of the operation that failed.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `sysOpen`

Open a file. Uses the open syscall on x86-64 and openat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails, with a subclass matching the errno.

### Methods

#### `function sysOpen( I64 pathPtr, I64 flags, I64 mode ) -> I64`

Open a file. Uses the open syscall on x86-64 and openat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails, with a subclass matching the errno.

## function `sysClose`

Close a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysClose( I64 fd ) -> Void`

Close a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysRead`

Read bytes from a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysRead( I64 fd, I64 bufPtr, I64 count ) -> I64`

Read bytes from a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysWrite`

Write bytes to a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysWrite( I64 fd, I64 bufPtr, I64 count ) -> I64`

Write bytes to a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysLseek`

Reposition the file offset of a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysLseek( I64 fd, I64 offset, I64 whence ) -> I64`

Reposition the file offset of a file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysStat`

Obtain file status for a path, following symbolic links. Uses stat on x86-64 and newfstatat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysStat( I64 pathPtr, I64 statBufPtr ) -> Void`

Obtain file status for a path, following symbolic links. Uses stat on x86-64 and newfstatat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysFstat`

Obtain file status for an open file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysFstat( I64 fd, I64 statBufPtr ) -> Void`

Obtain file status for an open file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysLstat`

Obtain file status for a path without following symbolic links. Uses lstat on x86-64 and newfstatat with AT_SYMLINK_NOFOLLOW on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysLstat( I64 pathPtr, I64 statBufPtr ) -> Void`

Obtain file status for a path without following symbolic links. Uses lstat on x86-64 and newfstatat with AT_SYMLINK_NOFOLLOW on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysFtruncate`

Truncate or extend a file to a specified length.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysFtruncate( I64 fd, I64 length ) -> Void`

Truncate or extend a file to a specified length.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysFsync`

Synchronize a file's in-core state with its storage device.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysFsync( I64 fd ) -> Void`

Synchronize a file's in-core state with its storage device.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysMkdir`

Create a directory. Uses mkdir on x86-64 and mkdirat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysMkdir( I64 pathPtr, I64 mode ) -> Void`

Create a directory. Uses mkdir on x86-64 and mkdirat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysRmdir`

Remove an empty directory. Uses rmdir on x86-64 and unlinkat with AT_REMOVEDIR on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysRmdir( I64 pathPtr ) -> Void`

Remove an empty directory. Uses rmdir on x86-64 and unlinkat with AT_REMOVEDIR on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysUnlink`

Delete a file. Uses unlink on x86-64 and unlinkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysUnlink( I64 pathPtr ) -> Void`

Delete a file. Uses unlink on x86-64 and unlinkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysRename`

Rename a file or directory. Uses rename on x86-64 and renameat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysRename( I64 oldPtr, I64 newPtr ) -> Void`

Rename a file or directory. Uses rename on x86-64 and renameat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysLink`

Create a hard link. Uses link on x86-64 and linkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysLink( I64 oldPtr, I64 newPtr ) -> Void`

Create a hard link. Uses link on x86-64 and linkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysSymlink`

Create a symbolic link. Uses symlink on x86-64 and symlinkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysSymlink( I64 targetPtr, I64 linkPtr ) -> Void`

Create a symbolic link. Uses symlink on x86-64 and symlinkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysReadlink`

Read the target of a symbolic link. Uses readlink on x86-64 and readlinkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysReadlink( I64 pathPtr, I64 bufPtr, I64 bufSize ) -> I64`

Read the target of a symbolic link. Uses readlink on x86-64 and readlinkat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysChmod`

Change file permissions. Uses chmod on x86-64 and fchmodat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysChmod( I64 pathPtr, I64 mode ) -> Void`

Change file permissions. Uses chmod on x86-64 and fchmodat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysFchmod`

Change file permissions for an open file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysFchmod( I64 fd, I64 mode ) -> Void`

Change file permissions for an open file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysChown`

Change file ownership. Uses chown on x86-64 and fchownat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysChown( I64 pathPtr, I64 uid, I64 gid ) -> Void`

Change file ownership. Uses chown on x86-64 and fchownat on aarch64.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysFchown`

Change file ownership for an open file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysFchown( I64 fd, I64 uid, I64 gid ) -> Void`

Change file ownership for an open file descriptor.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysAccess`

Check file accessibility. Uses access on x86-64 and faccessat on aarch64. Returns a Boolean rather than raising.

**Returns**: `Boolean` — True if the access check passed, False otherwise.

### Methods

#### `function sysAccess( I64 pathPtr, I64 mode ) -> Boolean`

Check file accessibility. Uses access on x86-64 and faccessat on aarch64. Returns a Boolean rather than raising.

**Returns**: `Boolean` — True if the access check passed, False otherwise.

## function `sysGetcwd`

Get the current working directory.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysGetcwd( I64 bufPtr, I64 size ) -> I64`

Get the current working directory.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysChdir`

Change the current working directory.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysChdir( I64 pathPtr ) -> Void`

Change the current working directory.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysGetdents64`

Read directory entries.

**Returns**: `I64` — Bytes read into the buffer, or 0 at end of directory.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysGetdents64( I64 fd, I64 bufPtr, I64 bufSize ) -> I64`

Read directory entries.

**Returns**: `I64` — Bytes read into the buffer, or 0 at end of directory.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysStatfs`

Obtain filesystem statistics.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysStatfs( I64 pathPtr, I64 bufPtr ) -> Void`

Obtain filesystem statistics.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysFlock`

Apply or remove an advisory lock on a file.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysFlock( I64 fd, I64 operation ) -> Void`

Apply or remove an advisory lock on a file.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysIoctl`

Perform a device-specific I/O control operation.

**Returns**: `I64` — The return value from the ioctl call.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

### Methods

#### `function sysIoctl( I64 fd, I64 request, I64 argPtr ) -> I64`

Perform a device-specific I/O control operation.

**Returns**: `I64` — The return value from the ioctl call.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

## function `sysPipe2`

Create a pipe with flags. The two file descriptors (read end, write end) are written to the memory at pipefdPtr.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function sysPipe2( I64 pipefdPtr, I64 flags ) -> Void`

Create a pipe with flags. The two file descriptors (read end, write end) are written to the memory at pipefdPtr.

**Raises**:

- `IOError` → `Error` — If the syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

