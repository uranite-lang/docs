# uranite.os.arch.registers

## Table of Contents

- [struct `Registers`](#struct-registers)
  - [`Registers()`](#Registers)

## struct `Registers`

CPU register context saved and restored during task context switches. Fields use x86-64 names as the canonical identifiers. On aarch64, the mapping is: rax-r15 hold x0-x15, rip holds pc, rsp holds sp, rbp holds x29, rflags holds pstate, cs and ss are unused (zero).

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

