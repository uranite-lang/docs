# uranite.os.sync.spinlock

## Table of Contents

- [Imports](#imports)
- [class `Spinlock`](#class-spinlock)
  - [`Spinlock()`](#Spinlock)
  - [`lock()`](#lock)
  - [`unlock()`](#unlock)
  - [`tryLock()`](#tryLock)
  - [`isLocked()`](#isLocked)
- [class `SpinlockIrq`](#class-spinlockirq)
  - [`SpinlockIrq()`](#SpinlockIrq)
  - [`lock()`](#lock)
  - [`unlock()`](#unlock)
  - [`tryLock()`](#tryLock)
  - [`isLocked()`](#isLocked)

## Imports

- `uranite.os.arch.cpu`
  - `disableInterrupts`
  - `enableInterrupts`
  - `readRflags`

## class `Spinlock`

Atomic spinlock using xchg instruction for mutual exclusion. Suitable for user-space code or kernel code where interrupt management is handled externally by the caller. Does not modify interrupt state. For kernel contexts requiring automatic interrupt safety, use SpinlockIrq instead.

### Fields

| Name | Type | Access |
|------|------|--------|
| `locked` | `I64` | protect |

### Methods

#### `function Spinlock( self ) -> Void`

Construct an unlocked spinlock with the locked flag initialized to zero.

#### `function lock( self ) -> Void`

Acquire the spinlock by spinning in a loop using atomic xchg until the lock is acquired. The pause instruction hints the CPU to reduce power consumption during the spin wait.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unlock( self ) -> Void`

Release the spinlock by clearing the lock flag with a memory barrier to ensure all writes performed while the lock was held are visible before release.

#### `function tryLock( self ) -> Boolean`

Attempt to acquire the spinlock without spinning. Performs a single atomic exchange and returns True if the lock was successfully acquired, False if already held.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isLocked( self ) -> Boolean`

Return whether the spinlock is currently held. 

## class `SpinlockIrq`

Interrupt-safe spinlock for kernel-context use. Saves the processor interrupt state (RFLAGS IF bit) and disables maskable interrupts before acquiring the lock, then restores the previous interrupt state on release. This prevents deadlock when an interrupt handler running on the same CPU attempts to acquire a lock already held by the interrupted code path. Each SpinlockIrq instance stores its own saved flags, so nested locking across different instances is safe.

### Fields

| Name | Type | Access |
|------|------|--------|
| `locked` | `I64` | protect |
| `savedFlags` | `I64` | protect |

### Methods

#### `function SpinlockIrq( self ) -> Void`

Construct an unlocked interrupt-safe spinlock with the locked flag and saved RFLAGS both initialized to zero.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function lock( self ) -> Void`

Acquire the spinlock with interrupt protection. Reads the current RFLAGS register to capture the interrupt enable state, disables maskable interrupts via cli, then spins using atomic xchg until the lock is acquired. The saved RFLAGS value is stored for restoration when the lock is released.

**Complexity**:
- Time: `O(n)`
- Space: `O(1)`

#### `function unlock( self ) -> Void`

Release the spinlock and restore the interrupt state that was active before lock acquisition. A memory barrier ensures all writes performed while the lock was held are visible before release. Interrupts are re-enabled only if the IF flag (bit 9) was set in the saved RFLAGS register, preserving the caller's original interrupt disposition.

#### `function tryLock( self ) -> Boolean`

Attempt to acquire the interrupt-safe spinlock without spinning. Saves RFLAGS and disables interrupts before the single atomic exchange attempt. If the lock is already held, restores the previous interrupt state before returning False. Returns True on successful acquisition.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function isLocked( self ) -> Boolean`

Return whether the interrupt-safe spinlock is currently held. 

