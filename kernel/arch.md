# uranite.kernel.arch

## Table of Contents

- [Imports](#imports)
- [class `CpuContext`](#class-cpucontext)
  - [`CpuContext()`](#CpuContext)
  - [`disableInterruptsAndHalt()`](#disableInterruptsAndHalt)
  - [`flushTlbPage()`](#flushTlbPage)
  - [`flushTlbAll()`](#flushTlbAll)
  - [`getTimestamp()`](#getTimestamp)
- [class `DescriptorTables`](#class-descriptortables)
  - [`DescriptorTables()`](#DescriptorTables)
  - [`install()`](#install)
- [class `PageMapper`](#class-pagemapper)
  - [`PageMapper()`](#PageMapper)
  - [`mapPage()`](#mapPage)
  - [`unmapPage()`](#unmapPage)

## Imports

- `uranite.os.arch.cpu`
  - `disableInterrupts`
  - `enableInterrupts`
  - `halt`
  - `invlpg`
  - `readCr0`
  - `readCr2`
  - `readCr3`
  - `readCr4`
  - `readMsr`
  - `readRflags`
  - `readTsc`
  - `writeCr0`
  - `writeCr3`
  - `writeCr4`
  - `writeMsr`
- `uranite.os.arch.registers`
  - `Registers`
- `uranite.os.arch.gdt`
  - `Gdt`
  - `GdtEntry`
  - `GdtPointer`
  - `KERNEL_CODE_SELECTOR`
  - `KERNEL_DATA_SELECTOR`
  - `loadGdt`
  - `reloadSegments`
  - `userCodeSelector`
  - `userDataSelector`
- `uranite.os.arch.idt`
  - `GateType`
  - `IRQ_BASE`
  - `Idt`
  - `IdtEntry`
  - `IdtPointer`
  - `InterruptVector`
  - `loadIdt`
- `uranite.os.arch.paging`
  - `HUGE_PAGE_1GB`
  - `HUGE_PAGE_2MB`
  - `PAGE_ACCESSED`
  - `PAGE_CACHE_DISABLE`
  - `PAGE_DIRTY`
  - `PAGE_GLOBAL`
  - `PAGE_HUGE`
  - `PAGE_PRESENT`
  - `PAGE_SIZE`
  - `PAGE_USER`
  - `PAGE_WRITABLE`
  - `PAGE_WRITE_THROUGH`
  - `PageTable`
  - `PageTableEntry`
  - `flushPage`
  - `flushTlb`
  - `pageNoExecute`
  - `pdIndex`
  - `pdptIndex`
  - `pml4Index`
  - `ptIndex`
- `uranite.os.arch.port`
  - `inb`
  - `inl`
  - `inw`
  - `ioWait`
  - `outb`
  - `outl`
  - `outw`

## class `CpuContext`

Unified CPU state manager wrapping x86-64 register and control operations. Provides convenience methods for common CPU management patterns used during kernel initialization and runtime.

### Fields

| Name | Type | Access |
|------|------|--------|
| `registers` | `Registers` | public |

### Methods

#### `function CpuContext( self ) -> Void`

#### `function disableInterruptsAndHalt( self ) -> Void`

Atomically disable interrupts and halt the CPU. Used for fatal error conditions where no further execution is possible.

#### `function flushTlbPage( self, I64 virtualAddress ) -> Void`

Invalidate the TLB entry for a single virtual address. Use after modifying a single page table entry to ensure the CPU sees the updated mapping.

#### `function flushTlbAll( self ) -> Void`

Flush the entire TLB by reloading CR3 with its current value. Use after bulk page table modifications.

#### `function getTimestamp( self ) -> I64`

Read the CPU timestamp counter (TSC) for high-resolution timing.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

## class `DescriptorTables`

Manages GDT and IDT setup for kernel initialization. Provides a single install() call that loads both tables into the CPU.

### Fields

| Name | Type | Access |
|------|------|--------|
| `gdt` | `Gdt` | public |
| `idt` | `Idt` | public |

### Methods

#### `function DescriptorTables( self ) -> Void`

#### `function install( self ) -> Void`

Load both GDT and IDT into the CPU via LGDT/LIDT instructions. Must be called during kernel boot before enabling interrupts.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## class `PageMapper`

High-level page table management wrapping raw paging operations. Provides map/unmap/protect operations over the four-level x86-64 page table hierarchy.

### Fields

| Name | Type | Access |
|------|------|--------|
| `rootTable` | `PageTable` | public |

### Methods

#### `function PageMapper( self ) -> Void`

#### `function mapPage( self, I64 virtualAddress, I64 physicalAddress, I64 flags ) -> Void`

Map a single 4KB page from virtualAddress to physicalAddress with the given flags (PAGE_PRESENT | PAGE_WRITABLE | PAGE_USER etc.). Uses the L3/PT index to write into the root table directly — caller must ensure intermediate tables are walked.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function unmapPage( self, I64 virtualAddress ) -> Void`

Unmap a single 4KB page and invalidate its TLB entry.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

