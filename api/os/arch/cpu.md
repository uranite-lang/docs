# uranite.os.arch.cpu

## Table of Contents

- [function `disableInterrupts`](#function-disableinterrupts)
  - [`disableInterrupts()`](#disableInterrupts)
- [function `enableInterrupts`](#function-enableinterrupts)
  - [`enableInterrupts()`](#enableInterrupts)
- [function `readRflags`](#function-readrflags)
  - [`readRflags()`](#readRflags)
- [function `halt`](#function-halt)
  - [`halt()`](#halt)
- [function `readTsc`](#function-readtsc)
  - [`readTsc()`](#readTsc)
- [function `readCr3`](#function-readcr3)
  - [`readCr3()`](#readCr3)
- [function `writeCr3`](#function-writecr3)
  - [`writeCr3()`](#writeCr3)
- [function `invlpg`](#function-invlpg)
  - [`invlpg()`](#invlpg)
- [function `readCr0`](#function-readcr0)
  - [`readCr0()`](#readCr0)
- [function `writeCr0`](#function-writecr0)
  - [`writeCr0()`](#writeCr0)
- [function `readCr2`](#function-readcr2)
  - [`readCr2()`](#readCr2)
- [function `readCr4`](#function-readcr4)
  - [`readCr4()`](#readCr4)
- [function `writeCr4`](#function-writecr4)
  - [`writeCr4()`](#writeCr4)
- [function `readMsr`](#function-readmsr)
  - [`readMsr()`](#readMsr)
- [function `writeMsr`](#function-writemsr)
  - [`writeMsr()`](#writeMsr)

## function `disableInterrupts`

Disable maskable interrupts on the current CPU core. On x86-64 this clears the IF flag via cli. On aarch64 this sets the IRQ mask bit in DAIF via msr daifset.

### Methods

#### `function disableInterrupts(  ) -> Void`

Disable maskable interrupts on the current CPU core. On x86-64 this clears the IF flag via cli. On aarch64 this sets the IRQ mask bit in DAIF via msr daifset.

## function `enableInterrupts`

Enable maskable interrupts on the current CPU core. On x86-64 this sets the IF flag via sti. On aarch64 this clears the IRQ mask bit in DAIF via msr daifclr.

### Methods

#### `function enableInterrupts(  ) -> Void`

Enable maskable interrupts on the current CPU core. On x86-64 this sets the IF flag via sti. On aarch64 this clears the IRQ mask bit in DAIF via msr daifclr.

## function `readRflags`

Read the current interrupt enable state as a normalized flags value. Bit 9 (value 512) is set if interrupts are currently enabled, matching x86 RFLAGS IF flag semantics on all architectures.

On x86-64, directly reads RFLAGS via pushfq/pop. On aarch64, reads DAIF register and normalizes: if IRQ mask (bit 7) is clear (interrupts enabled), sets bit 9 in the result.

**Returns**: `I64` — Flags value where (result & 512) != 0 means interrupts enabled.

### Methods

#### `function readRflags(  ) -> I64`

Read the current interrupt enable state as a normalized flags value. Bit 9 (value 512) is set if interrupts are currently enabled, matching x86 RFLAGS IF flag semantics on all architectures.

On x86-64, directly reads RFLAGS via pushfq/pop. On aarch64, reads DAIF register and normalizes: if IRQ mask (bit 7) is clear (interrupts enabled), sets bit 9 in the result.

**Returns**: `I64` — Flags value where (result & 512) != 0 means interrupts enabled.

## function `halt`

Halt the CPU until the next interrupt arrives, reducing power consumption while idle.

### Methods

#### `function halt(  ) -> Void`

Halt the CPU until the next interrupt arrives, reducing power consumption while idle.

## function `readTsc`

Read the high-resolution monotonic counter. On x86-64 this reads the TSC via rdtsc. On AArch64 this reads the generic timer counter CNTVCT_EL0.

**Returns**: — I64:
The current counter value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readTsc(  ) -> I64`

Read the high-resolution monotonic counter. On x86-64 this reads the TSC via rdtsc. On AArch64 this reads the generic timer counter CNTVCT_EL0.

**Returns**: — I64:
The current counter value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `readCr3`

Read the page table base register. On x86-64 this reads CR3 (PML4 base). On AArch64 this reads TTBR0_EL1.

**Returns**: `I64` — The current page table base physical address.

### Methods

#### `function readCr3(  ) -> I64`

Read the page table base register. On x86-64 this reads CR3 (PML4 base). On AArch64 this reads TTBR0_EL1.

**Returns**: `I64` — The current page table base physical address.

## function `writeCr3`

Write the page table base register to switch address spaces. On x86-64 this writes CR3. On AArch64 this writes TTBR0_EL1.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new page table root.`

### Methods

#### `function writeCr3( I64 value ) -> Void`

Write the page table base register to switch address spaces. On x86-64 this writes CR3. On AArch64 this writes TTBR0_EL1.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new page table root.`

## function `invlpg`

Invalidate a single TLB entry for the specified virtual address.

**Parameters**:

- `virtualAddress` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

### Methods

#### `function invlpg( I64 virtualAddress ) -> Void`

Invalidate a single TLB entry for the specified virtual address.

**Parameters**:

- `virtualAddress` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

## function `readCr0`

Read the system control register. On x86-64 this reads CR0. On AArch64 this reads SCTLR_EL1.

**Returns**: `I64` — The system control register value.

### Methods

#### `function readCr0(  ) -> I64`

Read the system control register. On x86-64 this reads CR0. On AArch64 this reads SCTLR_EL1.

**Returns**: `I64` — The system control register value.

## function `writeCr0`

Write the system control register.

**Parameters**:

- `value` (`I64`)
- `The new control register value.`

### Methods

#### `function writeCr0( I64 value ) -> Void`

Write the system control register.

**Parameters**:

- `value` (`I64`)
- `The new control register value.`

## function `readCr2`

Read the fault address register. On x86-64 this reads CR2 (page fault linear address). On AArch64 this reads FAR_EL1.

**Returns**: `I64` — The faulting virtual address.

### Methods

#### `function readCr2(  ) -> I64`

Read the fault address register. On x86-64 this reads CR2 (page fault linear address). On AArch64 this reads FAR_EL1.

**Returns**: `I64` — The faulting virtual address.

## function `readCr4`

Read the extended control register. On x86-64 this reads CR4. On AArch64 this reads TCR_EL1.

**Returns**: `I64` — The extended control register value.

### Methods

#### `function readCr4(  ) -> I64`

Read the extended control register. On x86-64 this reads CR4. On AArch64 this reads TCR_EL1.

**Returns**: `I64` — The extended control register value.

## function `writeCr4`

Write the extended control register.

**Parameters**:

- `value` (`I64`)
- `The new extended control register value.`

### Methods

#### `function writeCr4( I64 value ) -> Void`

Write the extended control register.

**Parameters**:

- `value` (`I64`)
- `The new extended control register value.`

## function `readMsr`

Read a model-specific/system register. On x86-64 this uses rdmsr. On AArch64 this returns 0 (use dedicated register access functions instead).

**Parameters**:

- `msr` (`I64`)
- `The register index.`

**Returns**: `I64` — The register value (0 on AArch64).

### Methods

#### `function readMsr( I64 msr ) -> I64`

Read a model-specific/system register. On x86-64 this uses rdmsr. On AArch64 this returns 0 (use dedicated register access functions instead).

**Parameters**:

- `msr` (`I64`)
- `The register index.`

**Returns**: `I64` — The register value (0 on AArch64).

## function `writeMsr`

Write a model-specific/system register. On x86-64 this uses wrmsr. On AArch64 this is a no-op (use dedicated register access functions instead).

**Parameters**:

- `msr` (`I64`)
- `The register index.`
- `value` (`I64`)
- `The value to write.`

### Methods

#### `function writeMsr( I64 msr, I64 value ) -> Void`

Write a model-specific/system register. On x86-64 this uses wrmsr. On AArch64 this is a no-op (use dedicated register access functions instead).

**Parameters**:

- `msr` (`I64`)
- `The register index.`
- `value` (`I64`)
- `The value to write.`

