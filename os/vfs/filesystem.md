# uranite.os.vfs.filesystem

## Table of Contents

- [Imports](#imports)
- [interface `FileSystem`](#interface-filesystem)
  - [`read()`](#read)
  - [`write()`](#write)
  - [`lookup()`](#lookup)
  - [`create()`](#create)
  - [`remove()`](#remove)
  - [`getSize()`](#getSize)
  - [`getType()`](#getType)
  - [`getPermissions()`](#getPermissions)
  - [`sync()`](#sync)
- [enum `FileSystemType`](#enum-filesystemtype)

## Imports

- `uranite.memory.memory`
  - `Memory`

## interface `FileSystem`

Pluggable filesystem interface that abstracts the underlying storage format. Concrete filesystem implementations (such as FAT32, ext4, or UraniteFS) implement this interface to provide file and directory operations. All operations use inode numbers and raw memory buffers, maintaining independence from the VNode layer above.

### Methods

#### `function read( self, I64 inodeNumber, Memory<I8> buffer, I64 offset, I64 length ) -> I64`

Reads up to the specified number of bytes from the file at the given inode number into the buffer, starting at the given byte offset within the file. Returns the number of bytes actually read. 

#### `function write( self, I64 inodeNumber, Memory<I8> buffer, I64 offset, I64 length ) -> I64`

Writes the specified number of bytes from the buffer to the file at the given inode number, starting at the given byte offset within the file. Returns the number of bytes actually written. 

#### `function lookup( self, I64 directoryInode, Memory<I8> name, I64 nameLength ) -> I64`

Looks up a child entry by name within the directory at the given inode number. Returns the child's inode number if found, or -1 if no entry with the given name exists. 

#### `function create( self, I64 directoryInode, Memory<I8> name, I64 nameLength, I64 nodeType ) -> I64`

Creates a new directory entry with the given name and node type within the directory at the given inode number. Returns the newly allocated inode number, or -1 on failure. 

#### `function remove( self, I64 directoryInode, Memory<I8> name, I64 nameLength ) -> Boolean`

Removes the directory entry with the given name from the directory at the given inode number. Returns True if the entry was found and removed, or False otherwise. 

#### `function getSize( self, I64 inodeNumber ) -> I64`

Returns the size in bytes of the file at the given inode number. 

#### `function getType( self, I64 inodeNumber ) -> I64`

Returns the type of the node at the given inode number, using values matching the VNodeType enum. 

#### `function getPermissions( self, I64 inodeNumber ) -> I64`

Returns the permission bitmask for the node at the given inode number. 

#### `function sync( self ) -> Void`

Flushes all pending filesystem metadata and data changes to the underlying storage device.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## enum `FileSystemType`

Identifies the type of filesystem mounted at a given mount point. Each variant corresponds to a specific filesystem implementation that the kernel can use for file and directory operations on the mounted volume.

