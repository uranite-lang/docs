# uranite.os.arch.x86-64.cpu

## Table of Contents

- [struct `Registers`](#struct-registers)
  - [`Registers()`](#Registers)
- [function `halt`](#function-halt)
  - [`halt()`](#halt)
- [function `enableInterrupts`](#function-enableinterrupts)
  - [`enableInterrupts()`](#enableInterrupts)
- [function `disableInterrupts`](#function-disableinterrupts)
  - [`disableInterrupts()`](#disableInterrupts)
- [function `readCr0`](#function-readcr0)
  - [`readCr0()`](#readCr0)
- [function `writeCr0`](#function-writecr0)
  - [`writeCr0()`](#writeCr0)
- [function `readCr2`](#function-readcr2)
  - [`readCr2()`](#readCr2)
- [function `readCr3`](#function-readcr3)
  - [`readCr3()`](#readCr3)
- [function `writeCr3`](#function-writecr3)
  - [`writeCr3()`](#writeCr3)
- [function `readCr4`](#function-readcr4)
  - [`readCr4()`](#readCr4)
- [function `writeCr4`](#function-writecr4)
  - [`writeCr4()`](#writeCr4)
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

## struct `Registers`

Full x86_64 CPU register context including all general-purpose registers, the instruction pointer, flags register, and segment selectors. This structure is saved and restored during context switches between tasks and when entering or returning from interrupt handlers.

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

Construct a zeroed register context with all general-purpose registers, instruction pointer, flags, and segment selectors initialized to zero. This represents a clean CPU state suitable for a newly created task.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `halt`

Halt the CPU until the next interrupt arrives, reducing power consumption while idle. 

### Methods

#### `function halt(  ) -> Void`

Halt the CPU until the next interrupt arrives, reducing power consumption while idle. 

## function `enableInterrupts`

Enable maskable interrupts by setting the IF flag in RFLAGS via the sti instruction. 

### Methods

#### `function enableInterrupts(  ) -> Void`

Enable maskable interrupts by setting the IF flag in RFLAGS via the sti instruction. 

## function `disableInterrupts`

Disable maskable interrupts by clearing the IF flag in RFLAGS via the cli instruction. 

### Methods

#### `function disableInterrupts(  ) -> Void`

Disable maskable interrupts by clearing the IF flag in RFLAGS via the cli instruction. 

## function `readCr0`

Read control register CR0 which contains paging, protected mode, and FPU control flags. 

### Methods

#### `function readCr0(  ) -> I64`

Read control register CR0 which contains paging, protected mode, and FPU control flags. 

## function `writeCr0`

Write a new value to control register CR0 to enable or disable paging, protected mode, and FPU emulation settings.

**Parameters**:

- `value` (`I64`)
- `The new CR0 register value to write.`

### Methods

#### `function writeCr0( I64 value ) -> Void`

Write a new value to control register CR0 to enable or disable paging, protected mode, and FPU emulation settings.

**Parameters**:

- `value` (`I64`)
- `The new CR0 register value to write.`

## function `readCr2`

Read control register CR2 which contains the linear address that caused the last page fault. 

### Methods

#### `function readCr2(  ) -> I64`

Read control register CR2 which contains the linear address that caused the last page fault. 

## function `readCr3`

Read control register CR3 which holds the physical base address of the top-level page table (PML4 in x86_64 long mode). The lower 12 bits of CR3 contain process-context identifier and cache control flags, while the upper bits point to the 4KB-aligned PML4 table used by the MMU for virtual-to-physical address translation.

**Returns**: `I64` — The current CR3 register value containing the PML4 physical address.

### Methods

#### `function readCr3(  ) -> I64`

Read control register CR3 which holds the physical base address of the top-level page table (PML4 in x86_64 long mode). The lower 12 bits of CR3 contain process-context identifier and cache control flags, while the upper bits point to the 4KB-aligned PML4 table used by the MMU for virtual-to-physical address translation.

**Returns**: `I64` — The current CR3 register value containing the PML4 physical address.

## function `writeCr3`

Write a new physical address into control register CR3 to switch the active page table hierarchy. Writing CR3 implicitly flushes the entire Translation Lookaside Buffer (TLB), forcing the processor to re-walk the page tables for all subsequent memory accesses. This is the primary mechanism for switching between process address spaces during a context switch, and should be used sparingly due to the TLB flush cost.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new PML4 table to activate.`

### Methods

#### `function writeCr3( I64 value ) -> Void`

Write a new physical address into control register CR3 to switch the active page table hierarchy. Writing CR3 implicitly flushes the entire Translation Lookaside Buffer (TLB), forcing the processor to re-walk the page tables for all subsequent memory accesses. This is the primary mechanism for switching between process address spaces during a context switch, and should be used sparingly due to the TLB flush cost.

**Parameters**:

- `value` (`I64`)
- `The physical address of the new PML4 table to activate.`

## function `readCr4`

Read control register CR4 which contains flags controlling extended CPU features including Physical Address Extension (PAE), Page Size Extension (PSE), Supervisor Mode Execution Prevention (SMEP), and various other processor capabilities. These flags must be configured correctly during boot before enabling paging or entering long mode.

**Returns**: `I64` — The current CR4 register value.

### Methods

#### `function readCr4(  ) -> I64`

Read control register CR4 which contains flags controlling extended CPU features including Physical Address Extension (PAE), Page Size Extension (PSE), Supervisor Mode Execution Prevention (SMEP), and various other processor capabilities. These flags must be configured correctly during boot before enabling paging or entering long mode.

**Returns**: `I64` — The current CR4 register value.

## function `writeCr4`

Write a new value to control register CR4 to enable or disable extended CPU features. Changes to CR4 take effect immediately and may alter the processor's behavior for paging, memory protection, and instruction execution. Some CR4 bits require specific CPUID feature flags to be present before they can be set.

**Parameters**:

- `value` (`I64`)
- `The new CR4 register value to write.`

### Methods

#### `function writeCr4( I64 value ) -> Void`

Write a new value to control register CR4 to enable or disable extended CPU features. Changes to CR4 take effect immediately and may alter the processor's behavior for paging, memory protection, and instruction execution. Some CR4 bits require specific CPUID feature flags to be present before they can be set.

**Parameters**:

- `value` (`I64`)
- `The new CR4 register value to write.`

## function `invlpg`

Invalidate a single Translation Lookaside Buffer entry for the specified virtual address. Unlike writing CR3 which flushes the entire TLB, invlpg selectively removes only the cached page table entry for one virtual page. This is used after unmapping or remapping a single page to ensure the processor does not continue using stale translation data from the TLB.

**Parameters**:

- `virtualAddress` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

### Methods

#### `function invlpg( I64 virtualAddress ) -> Void`

Invalidate a single Translation Lookaside Buffer entry for the specified virtual address. Unlike writing CR3 which flushes the entire TLB, invlpg selectively removes only the cached page table entry for one virtual page. This is used after unmapping or remapping a single page to ensure the processor does not continue using stale translation data from the TLB.

**Parameters**:

- `virtualAddress` (`I64`)
- `The virtual address whose TLB entry should be invalidated.`

## function `readMsr`

Read a Model-Specific Register identified by its index number. The MSR index is loaded into ECX, and the rdmsr instruction returns the 64-bit value split across EDX (high 32 bits) and EAX (low 32 bits), which are then combined into a single I64 result. Common MSR indices include EFER (0xC0000080) for enabling long mode and NX bit, STAR (0xC0000081) for syscall segment selectors, and LSTAR (0xC0000082) for the syscall entry point address.

**Parameters**:

- `msr` (`I64`)
- `The MSR index number to read.`

**Returns**: — I64:
The 64-bit value stored in the specified MSR.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readMsr( I64 msr ) -> I64`

Read a Model-Specific Register identified by its index number. The MSR index is loaded into ECX, and the rdmsr instruction returns the 64-bit value split across EDX (high 32 bits) and EAX (low 32 bits), which are then combined into a single I64 result. Common MSR indices include EFER (0xC0000080) for enabling long mode and NX bit, STAR (0xC0000081) for syscall segment selectors, and LSTAR (0xC0000082) for the syscall entry point address.

**Parameters**:

- `msr` (`I64`)
- `The MSR index number to read.`

**Returns**: — I64:
The 64-bit value stored in the specified MSR.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

## function `writeMsr`

Write a 64-bit value to a Model-Specific Register identified by its index. The value is split into its low 32 bits (loaded into EAX) and high 32 bits (loaded into EDX), with the MSR index in ECX, before executing the wrmsr instruction. Writing to MSRs can fundamentally change processor behavior including enabling syscall/sysret, configuring APIC base addresses, and setting up performance monitoring counters.

**Parameters**:

- `msr` (`I64`)
- `The MSR index number to write.`
- `value` (`I64`)
- `The 64-bit value to store in the specified MSR.`

### Methods

#### `function writeMsr( I64 msr, I64 value ) -> Void`

Write a 64-bit value to a Model-Specific Register identified by its index. The value is split into its low 32 bits (loaded into EAX) and high 32 bits (loaded into EDX), with the MSR index in ECX, before executing the wrmsr instruction. Writing to MSRs can fundamentally change processor behavior including enabling syscall/sysret, configuring APIC base addresses, and setting up performance monitoring counters.

**Parameters**:

- `msr` (`I64`)
- `The MSR index number to write.`
- `value` (`I64`)
- `The 64-bit value to store in the specified MSR.`

## function `readRflags`

Read the current value of the RFLAGS register by pushing it onto the stack and popping it into a general-purpose register. RFLAGS contains arithmetic status flags (carry, zero, sign, overflow), the interrupt enable flag (IF), the direction flag (DF), and privilege level indicators. This is useful for saving and restoring interrupt state around critical sections.

**Returns**: `I64` — The current RFLAGS register value.

### Methods

#### `function readRflags(  ) -> I64`

Read the current value of the RFLAGS register by pushing it onto the stack and popping it into a general-purpose register. RFLAGS contains arithmetic status flags (carry, zero, sign, overflow), the interrupt enable flag (IF), the direction flag (DF), and privilege level indicators. This is useful for saving and restoring interrupt state around critical sections.

**Returns**: `I64` — The current RFLAGS register value.

## function `readTsc`

Read the processor's Time Stamp Counter using the rdtsc instruction, which returns the number of CPU cycles elapsed since the last processor reset. The 64-bit counter value is returned split across EDX (high) and EAX (low), then combined into a single I64. This provides the highest-resolution timing available on x86_64 and is commonly used for performance measurement, random seed generation, and busy-wait calibration in kernel code.

**Returns**: — I64:
The current 64-bit time stamp counter value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

### Methods

#### `function readTsc(  ) -> I64`

Read the processor's Time Stamp Counter using the rdtsc instruction, which returns the number of CPU cycles elapsed since the last processor reset. The 64-bit counter value is returned split across EDX (high) and EAX (low), then combined into a single I64. This provides the highest-resolution timing available on x86_64 and is commonly used for performance measurement, random seed generation, and busy-wait calibration in kernel code.

**Returns**: — I64:
The current 64-bit time stamp counter value.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

