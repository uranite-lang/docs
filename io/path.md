# uranite.io.path

## Table of Contents

- [Imports](#imports)
- [function `join`](#function-join)
  - [`join()`](#join)
- [function `dirname`](#function-dirname)
  - [`dirname()`](#dirname)
- [function `basename`](#function-basename)
  - [`basename()`](#basename)
- [function `extension`](#function-extension)
  - [`extension()`](#extension)
- [function `stem`](#function-stem)
  - [`stem()`](#stem)
- [function `exists`](#function-exists)
  - [`exists()`](#exists)
- [function `isFile`](#function-isfile)
  - [`isFile()`](#isFile)
- [function `isDir`](#function-isdir)
  - [`isDir()`](#isDir)
- [function `isSymlink`](#function-issymlink)
  - [`isSymlink()`](#isSymlink)
- [function `readlink`](#function-readlink)
  - [`readlink()`](#readlink)
- [function `absolute`](#function-absolute)
  - [`absolute()`](#absolute)
- [function `parentDir`](#function-parentdir)
  - [`parentDir()`](#parentDir)
- [function `resolve`](#function-resolve)
  - [`resolve()`](#resolve)

## Imports

- `uranite.io.stat`
  - `FileStat`
  - `lstatPath`
  - `statPath`
- `uranite.io.syscall`
  - `F_OK`
  - `memoryToPtr`
  - `readByteAt`
  - `stringLen`
  - `stringToPtr`
  - `sysAccess`
  - `sysGetcwd`
  - `sysReadlink`
  - `writeByteAt`
- `uranite.memory.memory`
  - `Memory`

## function `join`

Join two path segments with a forward slash separator. If the first segment already ends with a slash, no extra slash is inserted.

**Parameters**:

- `basePath` (`String`)
- `The leading path segment.`
- `childPath` (`String`)
- `The trailing path segment to append.`

**Returns**: — String:
The combined path with exactly one slash between segments.

**Complexity**:
- Time: `O(n + m) where n and m are the lengths of basePath and childPath.`

### Methods

#### `function join( String basePath, String childPath ) -> String`

Join two path segments with a forward slash separator. If the first segment already ends with a slash, no extra slash is inserted.

**Parameters**:

- `basePath` (`String`)
- `The leading path segment.`
- `childPath` (`String`)
- `The trailing path segment to append.`

**Returns**: — String:
The combined path with exactly one slash between segments.

**Complexity**:
- Time: `O(n + m) where n and m are the lengths of basePath and childPath.`

## function `dirname`

Return the directory portion of a path by finding the last forward slash. Returns "." if no slash is found, "/" if the only slash is at position zero.

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the directory from.`

**Returns**: — String:
The directory component of the path.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

### Methods

#### `function dirname( String path ) -> String`

Return the directory portion of a path by finding the last forward slash. Returns "." if no slash is found, "/" if the only slash is at position zero.

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the directory from.`

**Returns**: — String:
The directory component of the path.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

## function `basename`

Return the final component of a path after the last forward slash. If the path contains no slash, the entire path is returned. Returns an empty string if the path ends with a slash.

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the base name from.`

**Returns**: — String:
The filename component of the path.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

### Methods

#### `function basename( String path ) -> String`

Return the final component of a path after the last forward slash. If the path contains no slash, the entire path is returned. Returns an empty string if the path ends with a slash.

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the base name from.`

**Returns**: — String:
The filename component of the path.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

## function `extension`

Return the file extension from a path, excluding the dot. Operates on the basename portion only. Returns an empty string if there is no dot or the dot is the first character (hidden files).

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the extension from.`

**Returns**: — String:
The file extension without the leading dot, or empty string
if none.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

### Methods

#### `function extension( String path ) -> String`

Return the file extension from a path, excluding the dot. Operates on the basename portion only. Returns an empty string if there is no dot or the dot is the first character (hidden files).

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the extension from.`

**Returns**: — String:
The file extension without the leading dot, or empty string
if none.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

## function `stem`

Return the basename of a path with the extension removed. If there is no dot or the dot is the first character, the full basename is returned unchanged.

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the stem from.`

**Returns**: — String:
The filename without its extension.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

### Methods

#### `function stem( String path ) -> String`

Return the basename of a path with the extension removed. If there is no dot or the dot is the first character, the full basename is returned unchanged.

**Parameters**:

- `path` (`String`)
- `The filesystem path to extract the stem from.`

**Returns**: — String:
The filename without its extension.

**Complexity**:
- Time: `O(n) where n is the length of the path.`

## function `exists`

Check whether a file or directory exists at the given path using the access syscall with F_OK mode.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path exists, False otherwise.

### Methods

#### `function exists( String path ) -> Boolean`

Check whether a file or directory exists at the given path using the access syscall with F_OK mode.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path exists, False otherwise.

## function `isFile`

Check whether the given path exists and is a regular file.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path is a regular file, False otherwise.

### Methods

#### `function isFile( String path ) -> Boolean`

Check whether the given path exists and is a regular file.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path is a regular file, False otherwise.

## function `isDir`

Check whether the given path exists and is a directory.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path is a directory, False otherwise.

### Methods

#### `function isDir( String path ) -> Boolean`

Check whether the given path exists and is a directory.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path is a directory, False otherwise.

## function `isSymlink`

Check whether the given path exists and is a symbolic link. Uses lstat to avoid following the symlink itself.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path is a symbolic link, False otherwise.

### Methods

#### `function isSymlink( String path ) -> Boolean`

Check whether the given path exists and is a symbolic link. Uses lstat to avoid following the symlink itself.

**Parameters**:

- `path` (`String`)
- `The filesystem path to check.`

**Returns**: `Boolean` — True if the path is a symbolic link, False otherwise.

## function `readlink`

Read the target of a symbolic link and return it as a string.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the symbolic link.`

**Returns**: `String` — The target path that the symbolic link points to.

**Raises**:

- `IOError` → `Error` — If the readlink syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readlink( String path ) -> String`

Read the target of a symbolic link and return it as a string.

**Parameters**:

- `path` (`String`)
- `The filesystem path of the symbolic link.`

**Returns**: `String` — The target path that the symbolic link points to.

**Raises**:

- `IOError` → `Error` — If the readlink syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `absolute`

Convert a relative path to an absolute path by joining it with the current working directory. If the path is already absolute (starts with '/'), it is returned unchanged.

**Parameters**:

- `path` (`String`)
- `The filesystem path to resolve.`

**Returns**: `String` — The absolute path.

**Raises**:

- `IOError` → `Error` — If the getcwd syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function absolute( String path ) -> String`

Convert a relative path to an absolute path by joining it with the current working directory. If the path is already absolute (starts with '/'), it is returned unchanged.

**Parameters**:

- `path` (`String`)
- `The filesystem path to resolve.`

**Returns**: `String` — The absolute path.

**Raises**:

- `IOError` → `Error` — If the getcwd syscall fails.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `parentDir`

Return the parent directory of the given path.

Alias for dirname that reads more naturally in path-building code.

**Parameters**:

- `path` (`String`)
- `The filesystem path whose parent is requested.`

**Returns**: — String:
The parent directory path, or "." if the path has no separator.

**Complexity**:
- Time: `O(n) where n is the path length`
- Space: `O(1)`

### Methods

#### `function parentDir( String path ) -> String`

Return the parent directory of the given path.

Alias for dirname that reads more naturally in path-building code.

**Parameters**:

- `path` (`String`)
- `The filesystem path whose parent is requested.`

**Returns**: — String:
The parent directory path, or "." if the path has no separator.

**Complexity**:
- Time: `O(n) where n is the path length`
- Space: `O(1)`

## function `resolve`

Resolve a child path relative to a base path.

Alias for join that reads more naturally when building paths from a known base directory.

**Parameters**:

- `basePath` (`String`)
- `The base directory path.`
- `child` (`String`)
- `The child segment to append.`

**Returns**: — String:
The combined path.

**Complexity**:
- Time: `O(n + m) where n and m are the segment lengths`
- Space: `O(n + m)`

### Methods

#### `function resolve( String basePath, String child ) -> String`

Resolve a child path relative to a base path.

Alias for join that reads more naturally when building paths from a known base directory.

**Parameters**:

- `basePath` (`String`)
- `The base directory path.`
- `child` (`String`)
- `The child segment to append.`

**Returns**: — String:
The combined path.

**Complexity**:
- Time: `O(n + m) where n and m are the segment lengths`
- Space: `O(n + m)`

