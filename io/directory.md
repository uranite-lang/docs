# uranite.io.directory

## Table of Contents

- [Imports](#imports)
- [enum `EntryType`](#enum-entrytype)
- [class `DirectoryEntry`](#class-directoryentry)
  - [`DirectoryEntry()`](#DirectoryEntry)
  - [`getName()`](#getName)
  - [`getType()`](#getType)
  - [`getInodeNumber()`](#getInodeNumber)
  - [`isFile()`](#isFile)
  - [`isDirectory()`](#isDirectory)
  - [`isSymlink()`](#isSymlink)
- [function `dtypeToEntryType`](#function-dtypetoentrytype)
- [function `extractName`](#function-extractname)
- [function `isDotEntry`](#function-isdotentry)
- [class `DirectoryIterator`](#class-directoryiterator)
  - [`DirectoryIterator()`](#DirectoryIterator)
  - [`has()`](#has)
  - [`next()`](#next)
  - [`close()`](#close)
  - [`destroy()`](#destroy)
- [function `mkdir`](#function-mkdir)
  - [`mkdir()`](#mkdir)
- [function `mkdir`](#function-mkdir)
  - [`mkdir()`](#mkdir)
- [function `makedirs`](#function-makedirs)
  - [`makedirs()`](#makedirs)
- [function `makedirs`](#function-makedirs)
  - [`makedirs()`](#makedirs)
- [function `rmdir`](#function-rmdir)
  - [`rmdir()`](#rmdir)
- [function `removetree`](#function-removetree)
  - [`removetree()`](#removetree)
- [function `listdir`](#function-listdir)
  - [`listdir()`](#listdir)
- [function `countContents`](#function-countcontents)
  - [`countContents()`](#countContents)
- [function `walkDirectory`](#function-walkdirectory)
  - [`walkDirectory()`](#walkDirectory)

## Imports

- `uranite.collection.array-list`
  - `ArrayList`
- `uranite.io.errors`
  - `IOError`
- `uranite.io.path`
  - `dirname`
  - `exists`
  - `join`
- `uranite.io.stat`
  - `FileStat`
  - `statPath`
- `uranite.io.syscall`
  - `O_DIRECTORY`
  - `O_RDONLY`
  - `memoryToPtr`
  - `readByteAt`
  - `readI16At`
  - `readI64At`
  - `stringLen`
  - `stringToPtr`
  - `sysClose`
  - `sysGetdents64`
  - `sysMkdir`
  - `sysOpen`
  - `sysRmdir`
  - `sysUnlink`
  - `writeByteAt`
- `uranite.iterators.iterator`
  - `Iterator`
- `uranite.language.callable`
  - `Callable`
- `uranite.memory.memory`
  - `Memory`

## enum `EntryType`

Enumeration of filesystem entry types as reported by the Linux getdents64 syscall d_type field.

## class `DirectoryEntry`

Represents a single entry within a directory, containing its name, filesystem type, and inode number as returned by the getdents64 syscall.

### Fields

| Name | Type | Access |
|------|------|--------|
| `name` | `String` | public |
| `entryType` | `EntryType` | public |
| `inodeNumber` | `I64` | public |

### Methods

#### `function DirectoryEntry( self, String name, EntryType entryType, I64 inodeNumber ) -> Void`

Construct a new DirectoryEntry with the given name, type, and inode number.

**Parameters**:

- `name` (`String`)
- `The filename of this entry` (`without directory path`)
- `entryType` (`EntryType`)
- `The filesystem type classification of this entry.`
- `inodeNumber` (`I64`)
- `The inode number as reported by the filesystem.`

#### `function getName( self ) -> String`

Return the name of this directory entry.

**Returns**: — The filename without any directory path components.

#### `function getType( self ) -> EntryType`

Return the filesystem type of this directory entry.

**Returns**: — The EntryType classification of this entry.

#### `function getInodeNumber( self ) -> I64`

Return the inode number of this directory entry.

**Returns**: — The inode number as reported by the filesystem.

#### `function isFile( self ) -> Boolean`

Check whether this entry is a regular file.

**Returns**: — True if this entry is a regular file, False otherwise.

#### `function isDirectory( self ) -> Boolean`

Check whether this entry is a directory.

**Returns**: — True if this entry is a directory, False otherwise.

#### `function isSymlink( self ) -> Boolean`

Check whether this entry is a symbolic link.

**Returns**: — True if this entry is a symbolic link, False otherwise.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `dtypeToEntryType`

### Methods

#### `function dtypeToEntryType( I64 dtype ) -> EntryType`

## function `extractName`

### Methods

#### `function extractName( I64 bufAddr, I64 nameOffset ) -> String`

## function `isDotEntry`

### Methods

#### `function isDotEntry( String name ) -> Boolean`

## class `DirectoryIterator`

**Implements**: `Iterator<DirectoryEntry>`

Lazy iterator over directory entries using the Linux getdents64 syscall. Reads directory entries in 4096-byte chunks and yields them one at a time, automatically skipping the "." and ".." dot entries. Implements the Iterator<DirectoryEntry> interface for use with standard iteration patterns.

### Fields

| Name | Type | Access |
|------|------|--------|
| `fd` | `I64` | protect |
| `buffer` | `Memory<I64>` | protect |
| `bufferSize` | `I64` | protect |
| `bytesRead` | `I64` | protect |
| `position` | `I64` | protect |
| `exhausted` | `Boolean` | protect |
| `closed` | `Boolean` | protect |
| `hasPending` | `Boolean` | protect |
| `pendingName` | `String` | protect |
| `pendingType` | `EntryType` | protect |
| `pendingIno` | `I64` | protect |

### Methods

#### `function DirectoryIterator( self, String path ) -> Void`

Construct a new DirectoryIterator that lazily scans the directory at the given path. Opens the directory and reads the first batch of entries immediately, advancing past any dot entries.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to iterate over.`

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function advance( self ) -> Void`

#### `property has( self ) -> Boolean`

`property` 

Check whether there are more directory entries available.

**Returns**: — True if a pending entry is ready to be yielded, False if the
directory has been fully scanned.

#### `property next( self ) -> DirectoryEntry`

`property` 

Return the next directory entry and advance the iterator. Must only be called when has is True.

**Returns**: — The next DirectoryEntry in the directory scan.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function close( self ) -> Void`

Close this iterator, releasing the directory file descriptor and freeing the internal buffer. Subsequent calls are no-ops.

#### `function destroy( self ) -> Void`

Destructor that delegates to close to ensure resource cleanup.

## function `mkdir`

Create a single directory with default permissions (0755). The parent directory must already exist.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to create.`

**Raises**:

- `IOError` → `Error` — If the directory cannot be created.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function mkdir( String path ) -> Void`

Create a single directory with default permissions (0755). The parent directory must already exist.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to create.`

**Raises**:

- `IOError` → `Error` — If the directory cannot be created.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `mkdir`

Create a single directory at the specified path with the given permission mode. The parent directory must already exist.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to create.`
- `mode` (`I64`)
- `The permission mode bits for the new directory` (`e.g., 493 for 0755`)

**Raises**:

- `IOError` → `Error` — If the directory cannot be created.

### Methods

#### `function mkdir( String path, I64 mode ) -> Void`

Create a single directory at the specified path with the given permission mode. The parent directory must already exist.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to create.`
- `mode` (`I64`)
- `The permission mode bits for the new directory` (`e.g., 493 for 0755`)

**Raises**:

- `IOError` → `Error` — If the directory cannot be created.

## function `makedirs`

Recursively create a directory tree with default permissions (0755). If the directory already exists, this function is a no-op.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory tree to create.`

**Complexity**:
- Time: `O(d) where d is directory depth`
- Space: `O(d)`

### Methods

#### `function makedirs( String path ) -> Void`

Recursively create a directory tree with default permissions (0755). If the directory already exists, this function is a no-op.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory tree to create.`

**Complexity**:
- Time: `O(d) where d is directory depth`
- Space: `O(d)`

## function `makedirs`

Recursively create a directory and all missing parent directories. If the directory already exists, this function is a no-op. Equivalent to the POSIX "mkdir -p" behavior.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory tree to create.`
- `mode` (`I64`)
- `The permission mode bits for each newly created directory.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function makedirs( String path, I64 mode ) -> Void`

Recursively create a directory and all missing parent directories. If the directory already exists, this function is a no-op. Equivalent to the POSIX "mkdir -p" behavior.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory tree to create.`
- `mode` (`I64`)
- `The permission mode bits for each newly created directory.`

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `rmdir`

Remove an empty directory at the specified path.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the empty directory to remove.`

**Raises**:

- `DirectoryNotEmptyError` → `IOError` → `Error` — If the directory is not empty.
- `IOError` → `Error` — If the directory cannot be removed.

### Methods

#### `function rmdir( String path ) -> Void`

Remove an empty directory at the specified path.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the empty directory to remove.`

**Raises**:

- `DirectoryNotEmptyError` → `IOError` → `Error` — If the directory is not empty.
- `IOError` → `Error` — If the directory cannot be removed.

## function `removetree`

Recursively remove a directory and all of its contents, including subdirectories and files. Equivalent to the POSIX "rm -rf" behavior.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory tree to remove.`

**Raises**:

- `IOError` → `Error` — If any file or directory cannot be removed.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function removetree( String path ) -> Void`

Recursively remove a directory and all of its contents, including subdirectories and files. Equivalent to the POSIX "rm -rf" behavior.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory tree to remove.`

**Raises**:

- `IOError` → `Error` — If any file or directory cannot be removed.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `listdir`

List all entries in a directory, returning them as an ArrayList. Dot entries ("." and "..") are excluded from the results.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to list.`

**Returns**: — An ArrayList containing a DirectoryEntry for each entry in the directory.

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened or read.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function listdir( String path ) -> ArrayList<DirectoryEntry>`

List all entries in a directory, returning them as an ArrayList. Dot entries ("." and "..") are excluded from the results.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to list.`

**Returns**: — An ArrayList containing a DirectoryEntry for each entry in the directory.

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened or read.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `countContents`

Count the number of entries in a directory, excluding dot entries.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to count.`

**Returns**: — The number of entries in the directory.

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened or read.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

### Methods

#### `function countContents( String path ) -> I64`

Count the number of entries in a directory, excluding dot entries.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the directory to count.`

**Returns**: — The number of entries in the directory.

**Raises**:

- `IOError` → `Error` — If the directory cannot be opened or read.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

## function `walkDirectory`

Recursively visit every file in a directory tree, invoking the visitor callback with the full path of each regular file encountered.

**Parameters**:

- `path` (`String`)
- `The root directory path to walk.`
- `visitor` (`Callable<Void, <String>>`)
- `A callback invoked once per file with the file's full path.`

**Raises**:

- `IOError` → `Error` — If any directory in the tree cannot be opened or read.

**Complexity**:
- Time: `O(n) where n is total entries in the tree`
- Space: `O(d) where d is maximum directory depth`

### Methods

#### `function walkDirectory( String path, <type> visitor ) -> Void`

Recursively visit every file in a directory tree, invoking the visitor callback with the full path of each regular file encountered.

**Parameters**:

- `path` (`String`)
- `The root directory path to walk.`
- `visitor` (`Callable<Void, <String>>`)
- `A callback invoked once per file with the file's full path.`

**Raises**:

- `IOError` → `Error` — If any directory in the tree cannot be opened or read.

**Complexity**:
- Time: `O(n) where n is total entries in the tree`
- Space: `O(d) where d is maximum directory depth`

