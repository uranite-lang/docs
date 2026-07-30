# uranite.os.arch.aarch64.cpu

## Table of Contents

- [struct `Registers`](#struct-registers)
  - [`Registers()`](#Registers)
- [function `halt`](#function-halt)
  - [`halt()`](#halt)
- [function `enableInterrupts`](#function-enableinterrupts)
  - [`enableInterrupts()`](#enableInterrupts)
- [function `disableInterrupts`](#function-disableinterrupts)
  - [`disableInterrupts()`](#disableInterrupts)
- [function `readSctlr`](#function-readsctlr)
  - [`readSctlr()`](#readSctlr)
- [function `writeSctlr`](#function-writesctlr)
  - [`writeSctlr()`](#writeSctlr)
- [function `readFar`](#function-readfar)
  - [`readFar()`](#readFar)
- [function `readTtbr0`](#function-readttbr0)
  - [`readTtbr0()`](#readTtbr0)
- [function `writeTtbr0`](#function-writettbr0)
  - [`writeTtbr0()`](#writeTtbr0)
- [function `readTtbr1`](#function-readttbr1)
  - [`readTtbr1()`](#readTtbr1)
- [function `writeTtbr1`](#function-writettbr1)
  - [`writeTtbr1()`](#writeTtbr1)
- [function `readTcr`](#function-readtcr)
  - [`readTcr()`](#readTcr)
- [function `writeTcr`](#function-writetcr)
  - [`writeTcr()`](#writeTcr)
- [function `invlpg`](#function-invlpg)
  - [`invlpg()`](#invlpg)
- [function `readMsr`](#function-readmsr)
  - [`readMsr()`](#readMsr)
- [function `writeMsr`](#function-writemsr)
  - [`writeMsr()`](#writeMsr)
- [function `readRflags`](#function-readrflags)
  - [`readRflags()`](#readRflags)
- [function `readTsc`](#function-readtsc)
  - [`readTsc()`](#readTsc)
- [function `readCr0`](#function-readcr0)
  - [`readCr0()`](#readCr0)
- [function `readCr2`](#function-readcr2)
  - [`readCr2()`](#readCr2)
- [function `readCr3`](#function-readcr3)
  - [`readCr3()`](#readCr3)
- [function `writeCr3`](#function-writecr3)
  - [`writeCr3()`](#writeCr3)
- [function `readCr4`](#function-readcr4)
  - [`readCr4()`](#readCr4)
- [function `writeCr0`](#function-writecr0)
  - [`writeCr0()`](#writeCr0)
- [function `writeCr4`](#function-writecr4)
  - [`writeCr4()`](#writeCr4)

## struct `Registers`

Full AArch64 CPU register context including all general-purpose registers (x0-x15), the program counter, stack pointer, frame pointer, and PSTATE flags. This structure is saved and restored during context switches and interrupt entry/exit. Field names use x86-64 conventions for cross-platform compatibility with the kernel scheduler.

### Fields

| Name | Type | Access |
|------|------|--------|
| `rax` | `I64` | public |
| `rbx` | `I64` | public |
| `rcx` | `I64` | public |
| `rdx` | `I64` | public |
| `rsi` | `I64` | public |
| `rdi` | `I64` | public |
| `rbp` | `I64` | public |
| `rsp` | `I64` | public |
| `r8` | `I64` | public |
| `r9` | `I64` | public |
| `r10` | `I64` | public |
| `r11` | `I64` | public |
| `r12` | `I64` | public |
| `r13` | `I64` | public |
| `r14` | `I64` | public |
| `r15` | `I64` | public |
| `rip` | `I64` | public |
| `rflags` | `I64` | public |
| `cs` | `I64` | public |
| `ss` | `I64` | public |

### Methods

#### `function Registers( self ) -> Void`

Construct a zeroed register context suitable for a newly created task.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `halt`

Halt the CPU until the next interrupt arrives via the WFI instruction. 

### Methods

#### `function halt(  ) -> Void`

Halt the CPU until the next interrupt arrives via the WFI instruction. 

## function `enableInterrupts`

Enable IRQ interrupts by clearing the I bit in DAIF. 

### Methods

#### `function enableInterrupts(  ) -> Void`

Enable IRQ interrupts by clearing the I bit in DAIF. 

## function `disableInterrupts`

Disable IRQ interrupts by setting the I bit in DAIF. 

### Methods

#### `function disableInterrupts(  ) -> Void`

Disable IRQ interrupts by setting the I bit in DAIF. 

## function `readSctlr`

Read SCTLR_EL1 (System Control Register) which contains MMU enable, cache enable, alignment check, and other system control flags. Equivalent to x86-64 CR0 in function.

**Returns**: `I64` — The current SCTLR_EL1 value.

### Methods

#### `function readSctlr(  ) -> I64`

Read SCTLR_EL1 (System Control Register) which contains MMU enable, cache enable, alignment check, and other system control flags. Equivalent to x86-64 CR0 in function.

**Returns**: `I64` — The current SCTLR_EL1 value.

## function `writeSctlr`

Write a new value to SCTLR_EL1 to enable or disable the MMU, caches, and alignment checking. Changes take effect after an ISB barrier.

**Parameters**:

- `value` (`I64`)
- `The new SCTLR_EL1 value to write.`

### Methods

#### `function writeSctlr( I64 value ) -> Void`

Write a new value to SCTLR_EL1 to enable or disable the MMU, caches, and alignment checking. Changes take effect after an ISB barrier.

**Parameters**:

- `value` (`I64`)
- `The new SCTLR_EL1 value to write.`

## function `readFar`

Read FAR_EL1 (Fault Address Register) which contains the virtual address that caused the last data abort or instruction abort. Equivalent to x86-64 CR2 (page fault linear address).

**Returns**: `I64` — The faulting virtual address.

### Methods

#### `function readFar(  ) -> I64`

Read FAR_EL1 (Fault Address Register) which contains the virtual address that caused the last data abort or instruction abort. Equivalent to x86-64 CR2 (page fault linear address).

**Returns**: `I64` — The faulting virtual address.

## function `readTtbr0`

Read TTBR0_EL1 (Translation Table Base Register 0) which holds the physical base address of the page table for the lower virtual address range (user space). Equivalent to x86-64 CR3.

**Returns**: `I64` — The current TTBR0_EL1 value containing the page table base.

### Methods

#### `function readTtbr0(  ) -> I64`

Read TTBR0_EL1 (Translation Table Base Register 0) which holds the physical base address of the page table for the lower virtual address range (user space). Equivalent to x86-64 CR3.

**Returns**: `I64` — The current TTBR0_EL1 value containing the page table base.

## function `writeTtbr0`

Write a new physical address into TTBR0_EL1 to switch the active user-space page table. Followed by a TLB invalidation and ISB to ensure consistency. This is the primary mechanism for switching between process address spaces.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new translation table to activate.`

### Methods

#### `function writeTtbr0( I64 value ) -> Void`

Write a new physical address into TTBR0_EL1 to switch the active user-space page table. Followed by a TLB invalidation and ISB to ensure consistency. This is the primary mechanism for switching between process address spaces.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new translation table to activate.`

## function `readTtbr1`

Read TTBR1_EL1 which holds the page table base for the upper virtual address range (kernel space). On AArch64, TTBR1 maps the kernel half of the address space while TTBR0 maps the user half.

**Returns**: `I64` — The current TTBR1_EL1 value.

### Methods

#### `function readTtbr1(  ) -> I64`

Read TTBR1_EL1 which holds the page table base for the upper virtual address range (kernel space). On AArch64, TTBR1 maps the kernel half of the address space while TTBR0 maps the user half.

**Returns**: `I64` — The current TTBR1_EL1 value.

## function `writeTtbr1`

Write a new value to TTBR1_EL1 for kernel address space page table switching.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new kernel page table.`

### Methods

#### `function writeTtbr1( I64 value ) -> Void`

Write a new value to TTBR1_EL1 for kernel address space page table switching.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new kernel page table.`

## function `readTcr`

Read TCR_EL1 (Translation Control Register) which configures page table granule size, address space size, and cacheability attributes for both TTBR0 and TTBR1 regions.

**Returns**: `I64` — The current TCR_EL1 value.

### Methods

#### `function readTcr(  ) -> I64`

Read TCR_EL1 (Translation Control Register) which configures page table granule size, address space size, and cacheability attributes for both TTBR0 and TTBR1 regions.

**Returns**: `I64` — The current TCR_EL1 value.

## function `writeTcr`

Write a new value to TCR_EL1 to configure translation granule, region sizes, and shareability attributes.

**Parameters**:

- `value` (`I64`)
- `The new TCR_EL1 value.`

### Methods

#### `function writeTcr( I64 value ) -> Void`

Write a new value to TCR_EL1 to configure translation granule, region sizes, and shareability attributes.

**Parameters**:

- `value` (`I64`)
- `The new TCR_EL1 value.`

## function `invlpg`

Invalidate the TLB entry for the specified virtual address using TLBI VAE1IS. This is the AArch64 equivalent of x86-64 invlpg. The address is shifted right by 12 to form the page-aligned address input expected by TLBI.

**Parameters**:

- `virtualAddress` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

### Methods

#### `function invlpg( I64 virtualAddress ) -> Void`

Invalidate the TLB entry for the specified virtual address using TLBI VAE1IS. This is the AArch64 equivalent of x86-64 invlpg. The address is shifted right by 12 to form the page-aligned address input expected by TLBI.

**Parameters**:

- `virtualAddress` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

## function `readMsr`

Read a system register by index. On AArch64 this is provided for API compatibility; actual system register access uses specific mrs instructions. Returns 0 as a stub — use dedicated read functions for specific registers.

**Parameters**:

- `msr` (`I64`)
- `The system register index` (`unused on AArch64`)

**Returns**: `I64` — Always returns 0. Use specific register read functions instead.

### Methods

#### `function readMsr( I64 msr ) -> I64`

Read a system register by index. On AArch64 this is provided for API compatibility; actual system register access uses specific mrs instructions. Returns 0 as a stub — use dedicated read functions for specific registers.

**Parameters**:

- `msr` (`I64`)
- `The system register index` (`unused on AArch64`)

**Returns**: `I64` — Always returns 0. Use specific register read functions instead.

## function `writeMsr`

Write a system register by index. On AArch64 this is provided for API compatibility; actual system register access uses specific msr instructions. No-op — use dedicated write functions for specific registers.

### Methods

#### `function writeMsr( I64 msr, I64 value ) -> Void`

Write a system register by index. On AArch64 this is provided for API compatibility; actual system register access uses specific msr instructions. No-op — use dedicated write functions for specific registers.

## function `readRflags`

Read the current DAIF register and normalize to x86-64 RFLAGS format where bit 9 (IF) set means interrupts are enabled. On AArch64, DAIF bit 7 set means IRQ masked (disabled), so this function inverts the sense.

**Returns**: `I64` — Normalized flags value with bit 9 = interrupts enabled.

### Methods

#### `function readRflags(  ) -> I64`

Read the current DAIF register and normalize to x86-64 RFLAGS format where bit 9 (IF) set means interrupts are enabled. On AArch64, DAIF bit 7 set means IRQ masked (disabled), so this function inverts the sense.

**Returns**: `I64` — Normalized flags value with bit 9 = interrupts enabled.

## function `readTsc`

Read the generic timer counter (CNTVCT_EL0) which provides a monotonically increasing cycle counter similar to x86-64 RDTSC. The counter frequency can be read from CNTFRQ_EL0.

**Returns**: — I64:
The current virtual timer counter value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readTsc(  ) -> I64`

Read the generic timer counter (CNTVCT_EL0) which provides a monotonically increasing cycle counter similar to x86-64 RDTSC. The counter frequency can be read from CNTFRQ_EL0.

**Returns**: — I64:
The current virtual timer counter value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readCr0`

Read SCTLR_EL1 (AArch64 equivalent of CR0). 

### Methods

#### `function readCr0(  ) -> I64`

Read SCTLR_EL1 (AArch64 equivalent of CR0). 

## function `readCr2`

Read FAR_EL1 (AArch64 equivalent of CR2). 

### Methods

#### `function readCr2(  ) -> I64`

Read FAR_EL1 (AArch64 equivalent of CR2). 

## function `readCr3`

Read TTBR0_EL1 (AArch64 equivalent of CR3). 

### Methods

#### `function readCr3(  ) -> I64`

Read TTBR0_EL1 (AArch64 equivalent of CR3). 

## function `writeCr3`

Write TTBR0_EL1 (AArch64 equivalent of CR3). 

### Methods

#### `function writeCr3( I64 value ) -> Void`

Write TTBR0_EL1 (AArch64 equivalent of CR3). 

## function `readCr4`

Read TCR_EL1 (AArch64 equivalent of CR4). 

### Methods

#### `function readCr4(  ) -> I64`

Read TCR_EL1 (AArch64 equivalent of CR4). 

## function `writeCr0`

Write SCTLR_EL1 (AArch64 equivalent of CR0). 

### Methods

#### `function writeCr0( I64 value ) -> Void`

Write SCTLR_EL1 (AArch64 equivalent of CR0). 

## function `writeCr4`

Write TCR_EL1 (AArch64 equivalent of CR4). 

### Methods

#### `function writeCr4( I64 value ) -> Void`

Write TCR_EL1 (AArch64 equivalent of CR4). 

