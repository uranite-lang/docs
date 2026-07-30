# uranite.io.file

## Table of Contents

- [Imports](#imports)
- [const `DEFAULT_FILE_PERMISSIONS`](#const-default-file-permissions)
- [class `File`](#class-file)
  - [`read()`](#read)
  - [`write()`](#write)
  - [`flush()`](#flush)
  - [`readBytes()`](#readBytes)
  - [`readAll()`](#readAll)
  - [`readString()`](#readString)
  - [`writeString()`](#writeString)
  - [`seekStart()`](#seekStart)
  - [`seekEnd()`](#seekEnd)
  - [`seekCurrent()`](#seekCurrent)
  - [`tell()`](#tell)
  - [`truncate()`](#truncate)
  - [`stat()`](#stat)
  - [`getSize()`](#getSize)
  - [`getFd()`](#getFd)
  - [`getPath()`](#getPath)
  - [`isClosed()`](#isClosed)
  - [`close()`](#close)
  - [`readLines()`](#readLines)
  - [`writeLines()`](#writeLines)
  - [`copyTo()`](#copyTo)
  - [`lockShared()`](#lockShared)
  - [`lockExclusive()`](#lockExclusive)
  - [`unlock()`](#unlock)
  - [`tryLockExclusive()`](#tryLockExclusive)
  - [`setRawMode()`](#setRawMode)
  - [`restoreMode()`](#restoreMode)
  - [`setPermissions()`](#setPermissions)
  - [`setOwner()`](#setOwner)
  - [`destroy()`](#destroy)
- [function `openFile`](#function-openfile)
  - [`openFile()`](#openFile)
- [function `createFile`](#function-createfile)
  - [`createFile()`](#createFile)
- [function `createFile`](#function-createfile)
  - [`createFile()`](#createFile)
- [function `appendFile`](#function-appendfile)
  - [`appendFile()`](#appendFile)
- [function `readFileString`](#function-readfilestring)
  - [`readFileString()`](#readFileString)
- [function `writeFileString`](#function-writefilestring)
  - [`writeFileString()`](#writeFileString)
- [function `openForReadWrite`](#function-openforreadwrite)
  - [`openForReadWrite()`](#openForReadWrite)
- [function `openOrCreate`](#function-openorcreate)
  - [`openOrCreate()`](#openOrCreate)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.io.errors`
  - `IOError`
- `uranite.io.stat`
  - `FileStat`
  - `fstatFd`
- `uranite.io.stream`
  - `InputStream`
  - `OutputStream`
- `uranite.io.syscall`
  - `ECHO_FLAG`
  - `ICANON_FLAG`
  - `LOCK_EX`
  - `LOCK_NB`
  - `LOCK_SH`
  - `LOCK_UN`
  - `O_APPEND`
  - `O_CREAT`
  - `O_RDONLY`
  - `O_RDWR`
  - `O_TRUNC`
  - `O_WRONLY`
  - `SEEK_CUR`
  - `SEEK_END`
  - `SEEK_SET`
  - `SYS_FLOCK`
  - `TCGETS`
  - `TCSETS`
  - `memoryToPtr`
  - `readByteAt`
  - `readI32At`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `sysClose`
  - `sysFchmod`
  - `sysFchown`
  - `sysFlock`
  - `sysFstat`
  - `sysFsync`
  - `sysFtruncate`
  - `sysIoctl`
  - `sysLseek`
  - `sysOpen`
  - `sysRead`
  - `sysWrite`
  - `writeByteAt`
  - `writeI32At`
- `uranite.memory.memory`
  - `Memory`
- `uranite.os.syscall.invoke`
  - `syscall2`
- `uranite.os.syscall.result`
  - `SyscallResult`

## const `DEFAULT_FILE_PERMISSIONS`

Default file permission mode (octal 0644: owner read/write, group/other read).

## class `File`

**Implements**: `InputStream`, `OutputStream`

A file handle that implements both InputStream and OutputStream interfaces. Wraps a Linux file descriptor with read, write, seek, truncate, stat, and close operations. All operations raise IOError if the file has been closed.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fd` | `I64` | protect |
| `filePath` | `String` | protect |
| `closed` | `Boolean` | protect |

### Methods

#### `function File( self, String path, I64 flags, I64 mode ) -> Void`

Open a file at the given path with the specified flags and permission mode.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open.`
- `flags` (`I64`)
- `Bitwise OR of open flags (O_RDONLY, O_WRONLY, O_RDWR, O_CREAT,`
- `O_TRUNC, O_APPEND, etc.).`
- `mode` (`I64`)
- `The permission mode bits used when creating a new file`

**Raises**:

- `IOError` → `Error` — If the file cannot be opened.

#### `function read( self, Memory<I64> buffer, I64 offset, I64 length ) -> I64`

Read up to the specified number of bytes from the file into a Memory buffer. Bytes are read into a temporary buffer first, then copied element-by-element into the destination.

**Parameters**:

- `buffer` (`Memory<I64>`)
- `The destination buffer to read bytes into.`
- `offset` (`I64`)
- `The starting offset within the destination buffer.`
- `length` (`I64`)
- `The maximum number of bytes to read.`

**Returns**: — The actual number of bytes read, which may be less than length at
end of file.

**Raises**:

- `IOError` → `Error` — If the file is closed or the read fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function write( self, Memory<I64> buffer, I64 offset, I64 length ) -> I64`

Write the specified number of bytes from a Memory buffer to the file. Bytes are copied from the source buffer into a temporary buffer first, then written to the file descriptor. Handles partial writes by retrying.

**Parameters**:

- `buffer` (`Memory<I64>`)
- `The source buffer containing bytes to write.`
- `offset` (`I64`)
- `The starting offset within the source buffer.`
- `length` (`I64`)
- `The number of bytes to write.`

**Returns**: — The total number of bytes written.

**Raises**:

- `IOError` → `Error` — If the file is closed or the write fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function flush( self ) -> Void`

Flush all buffered data to the underlying storage device by calling fsync on the file descriptor.

**Raises**:

- `IOError` → `Error` — If the file is closed or the fsync fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function readBytes( self, I64 count ) -> Memory<I64>`

Read up to the specified number of bytes from the file, returning them in a newly allocated Memory buffer sized to the actual number of bytes read.

**Parameters**:

- `count` (`I64`)
- `The maximum number of bytes to read.`

**Returns**: — A newly allocated Memory buffer containing the bytes that were read.
The buffer size matches the actual byte count, not the requested count.

**Raises**:

- `IOError` → `Error` — If the file is closed or the read fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function readAll( self ) -> Memory<I64>`

Read all remaining bytes from the current file position to the end of the file. Uses fstat to determine the file size and calculates the remaining byte count.

**Returns**: — A newly allocated Memory buffer containing all remaining file contents.

**Raises**:

- `IOError` → `Error` — If the file is closed or any operation fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function readString( self ) -> String`

Read all remaining bytes from the current file position to the end of the file and return them as a null-terminated String. Uses fstat to determine the file size.

**Returns**: — The remaining file contents as a String.

**Raises**:

- `IOError` → `Error` — If the file is closed or any operation fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function writeString( self, String data ) -> I64`

Write all bytes of a string to the file. Handles partial writes by retrying until the full string is written.

**Parameters**:

- `data` (`String`)
- `The string to write to the file.`

**Returns**: — The total number of bytes written.

**Raises**:

- `IOError` → `Error` — If the file is closed or the write fails.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function seek( self, I64 offset, I64 whence ) -> I64`

Reposition the file offset within the open file.

**Parameters**:

- `offset` (`I64`)
- `The byte offset relative to the whence position.`
- `whence` (`I64`)
- `The reference point for the offset: SEEK_SET` (`0`)
- `SEEK_CUR` (`1`)

**Returns**: — The resulting absolute byte offset from the start of the file.

**Raises**:

- `IOError` → `Error` — If the file is closed or the seek fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function seekStart( self, I64 offset ) -> I64`

Seek to an absolute byte offset from the start of the file.

**Parameters**:

- `offset` (`I64`)
- `The absolute byte offset from the beginning of the file.`

**Returns**: — The resulting absolute byte offset from the start of the file.

#### `function seekEnd( self, I64 offset ) -> I64`

Seek to a byte offset relative to the end of the file.

**Parameters**:

- `offset` (`I64`)
- `The byte offset relative to the end of the file (typically`
- `zero or negative).`

**Returns**: — The resulting absolute byte offset from the start of the file.

#### `function seekCurrent( self, I64 offset ) -> I64`

Seek to a byte offset relative to the current file position.

**Parameters**:

- `offset` (`I64`)
- `The byte offset relative to the current position (positive`
- `moves forward, negative moves backward).`

**Returns**: — The resulting absolute byte offset from the start of the file.

#### `function tell( self ) -> I64`

Return the current byte offset within the file.

**Returns**: — The current absolute byte offset from the start of the file.

#### `function truncate( self, I64 length ) -> Void`

Truncate the file to the specified length in bytes. If the file is larger than the given length, the extra data is lost. If smaller, the file is extended with zero bytes.

**Parameters**:

- `length` (`I64`)
- `The desired file size in bytes after truncation.`

**Raises**:

- `IOError` → `Error` — If the file is closed or the truncation fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function stat( self ) -> FileStat`

Retrieve file status information for this open file using fstat.

**Returns**: — A FileStat object containing the file metadata (size, mode,
timestamps, etc.).

**Raises**:

- `IOError` → `Error` — If the file is closed or the fstat fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getSize( self ) -> I64`

Return the current size of the file in bytes.

**Returns**: — The file size in bytes as reported by fstat.

#### `function getFd( self ) -> I64`

Return the underlying Linux file descriptor number.

**Returns**: — The raw file descriptor integer.

#### `function getPath( self ) -> String`

Return the filesystem path this file was opened from.

**Returns**: — The original path string passed to the constructor.

#### `function isClosed( self ) -> Boolean`

Check whether this file has been closed.

**Returns**: — True if the file has been closed, False otherwise.

#### `function close( self ) -> Void`

Close this file, releasing the underlying file descriptor. Subsequent calls after the first close are no-ops.

#### `function readLines( self ) -> ArrayList<String>`

Read all remaining content and split into a list of lines.

Newline characters are stripped from each line. Resets the file position to the beginning before reading.

**Returns**: `ArrayList<String>` — List of lines in the file.

**Raises**:

- `IOError` → `Error` — If the file has been closed or cannot be read.

**Complexity**:
- Time: `O(n) where n is the file size`
- Space: `O(n)`

#### `function writeLines( self, ArrayList<String> lines ) -> Void`

Write all lines to the file, separating each with a newline character.

**Parameters**:

- `lines` (`ArrayList<String>`)
- `The lines to write.`

**Raises**:

- `IOError` → `Error` — If the file has been closed or cannot be written.

**Complexity**:
- Time: `O(n) where n is the total length of all lines`

#### `function copyTo( self, String destinationPath ) -> Void`

Copy the entire contents of this file to a new file at the given path.

Creates or truncates the destination file. Resets the source file position to the beginning before reading.

**Parameters**:

- `destinationPath` (`String`)
- `The path where the copy should be written.`

**Raises**:

- `IOError` → `Error` — If either file cannot be read or written.

**Complexity**:
- Time: `O(n) where n is the file size`

#### `function lockShared( self ) -> Void`

Acquire a shared (read) lock on this file. Multiple processes may hold a shared lock simultaneously. Blocks until acquired.

**Raises**:

- `IOError` → `Error` — If the lock operation fails.

**Complexity**:
- Time: `O(1)`

#### `function lockExclusive( self ) -> Void`

Acquire an exclusive (write) lock on this file. Only one process may hold an exclusive lock at a time. Blocks until acquired.

**Raises**:

- `IOError` → `Error` — If the lock operation fails.

**Complexity**:
- Time: `O(1)`

#### `function unlock( self ) -> Void`

Release any lock (shared or exclusive) held on this file.

**Raises**:

- `IOError` → `Error` — If the unlock operation fails.

**Complexity**:
- Time: `O(1)`

#### `function tryLockExclusive( self ) -> Boolean`

Attempt to acquire an exclusive lock without blocking. Returns True if the lock was acquired, False if another process holds a conflicting lock.

**Complexity**:
- Time: `O(1)`

#### `function setRawMode( self ) -> Memory<I64>`

Set the terminal associated with this file to raw mode by disabling ECHO and ICANON flags in the termios local flags. Returns saved termios settings that must be passed to restoreMode to undo.

**Returns**: — Memory<I64>:
Saved original termios settings buffer.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function restoreMode( self, Memory<I64> saved ) -> Void`

Restore terminal settings from a previously saved termios buffer, undoing the effect of setRawMode. Frees the saved buffer after restoring.

**Parameters**:

- `saved` (`Memory<I64>`)
- `The saved termios buffer returned by setRawMode. This buffer`
- `is freed after use and must not be used again.`

**Complexity**:
- Time: `O(1)`

#### `function setPermissions( self, I64 mode ) -> Void`

Change the permission mode of this file.

**Parameters**:

- `mode` (`I64`)
- `The new permission mode bits` (`e.g. 420 for octal 0644`)

**Raises**:

- `IOError` → `Error` — If the fchmod operation fails.

**Complexity**:
- Time: `O(1)`

#### `function setOwner( self, I64 uid, I64 gid ) -> Void`

Change the owner and group of this file.

**Parameters**:

- `uid` (`I64`)
- `The new owner user ID.`
- `gid` (`I64`)
- `The new group ID.`

**Raises**:

- `IOError` → `Error` — If the fchown operation fails.

**Complexity**:
- Time: `O(1)`

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

## function `openFile`

Open an existing file for reading only.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open.`

**Returns**: — A File handle opened in read-only mode.

**Raises**:

- `FileNotFoundError` → `IOError` → `Error` — If the file does not exist.
- `IOError` → `Error` — If the file cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function openFile( String path ) -> File`

Open an existing file for reading only.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open.`

**Returns**: — A File handle opened in read-only mode.

**Raises**:

- `FileNotFoundError` → `IOError` → `Error` — If the file does not exist.
- `IOError` → `Error` — If the file cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `createFile`

Create a new file or truncate an existing one, opened for writing only.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to create.`
- `mode` (`I64`)
- `The permission mode bits for the new file` (`e.g., 420 for 0644`)

**Returns**: — A File handle opened in write-only mode with create and truncate flags.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function createFile( String path, I64 mode ) -> File`

Create a new file or truncate an existing one, opened for writing only.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to create.`
- `mode` (`I64`)
- `The permission mode bits for the new file` (`e.g., 420 for 0644`)

**Returns**: — A File handle opened in write-only mode with create and truncate flags.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `createFile`

Create a new file or truncate an existing one with default permissions (0644: owner read/write, group/other read). Opened for writing only.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to create.`

**Returns**: — File:
A File handle opened in write-only mode.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function createFile( String path ) -> File`

Create a new file or truncate an existing one with default permissions (0644: owner read/write, group/other read). Opened for writing only.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to create.`

**Returns**: — File:
A File handle opened in write-only mode.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `appendFile`

Open a file for appending. Creates the file if it does not exist. All writes are appended to the end of the file.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open for appending.`

**Returns**: — A File handle opened in write-only append mode with permission 0644.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function appendFile( String path ) -> File`

Open a file for appending. Creates the file if it does not exist. All writes are appended to the end of the file.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open for appending.`

**Returns**: — A File handle opened in write-only append mode with permission 0644.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readFileString`

Read the entire contents of a file as a String. Opens the file, reads all content, closes the file, and returns the content.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to read.`

**Returns**: — The entire file contents as a String.

**Raises**:

- `FileNotFoundError` → `IOError` → `Error` — If the file does not exist.
- `IOError` → `Error` — If the file cannot be read.

### Methods

#### `function readFileString( String path ) -> String`

Read the entire contents of a file as a String. Opens the file, reads all content, closes the file, and returns the content.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to read.`

**Returns**: — The entire file contents as a String.

**Raises**:

- `FileNotFoundError` → `IOError` → `Error` — If the file does not exist.
- `IOError` → `Error` — If the file cannot be read.

## function `writeFileString`

Write a string to a file, creating or truncating it. Opens the file in write mode with create and truncate flags, writes the content, and closes the file.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to write.`
- `content` (`String`)
- `The string content to write to the file.`

### Methods

#### `function writeFileString( String path, String content ) -> Void`

Write a string to a file, creating or truncating it. Opens the file in write mode with create and truncate flags, writes the content, and closes the file.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to write.`
- `content` (`String`)
- `The string content to write to the file.`

## function `openForReadWrite`

Open an existing file for both reading and writing.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open.`

**Returns**: `File` — A File handle opened in read-write mode.

**Raises**:

- `IOError` → `Error` — If the file does not exist or cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function openForReadWrite( String path ) -> File`

Open an existing file for both reading and writing.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file to open.`

**Returns**: `File` — A File handle opened in read-write mode.

**Raises**:

- `IOError` → `Error` — If the file does not exist or cannot be opened.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `openOrCreate`

Open an existing file for read-write, creating it if absent.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file.`
- `mode` (`I64`)
- `The permission mode bits for the new file if created`

**Returns**: `File` — A File handle opened in read-write mode.

**Raises**:

- `IOError` → `Error` — If the file cannot be opened or created.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function openOrCreate( String path, I64 mode ) -> File`

Open an existing file for read-write, creating it if absent.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the file.`
- `mode` (`I64`)
- `The permission mode bits for the new file if created`

**Returns**: `File` — A File handle opened in read-write mode.

**Raises**:

- `IOError` → `Error` — If the file cannot be opened or created.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

