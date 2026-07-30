# uranite.os.debug.perf

## Table of Contents

- [Imports](#imports)
- [class `PerfCounters`](#class-perfcounters)
  - [`PerfCounters()`](#PerfCounters)
  - [`allocate()`](#allocate)
  - [`increment()`](#increment)
  - [`record()`](#record)
  - [`reset()`](#reset)
  - [`getValue()`](#getValue)
  - [`getMin()`](#getMin)
  - [`getMax()`](#getMax)
  - [`getSampleCount()`](#getSampleCount)
  - [`getCount()`](#getCount)
  - [`destroy()`](#destroy)

## Imports

- `uranite.memory.memory`
  - `Memory`
- `uranite.os.sync.spinlock`
  - `Spinlock`

## class `PerfCounters`

Collection of named performance counters for tracking kernel metrics. Each counter stores a current value along with running minimum, maximum, and sample count statistics. Counters are allocated by index and can be used for tracking latencies, throughput, cache hit rates, and other kernel performance indicators. A spinlock protects counter allocation.

### Fields

| Name | Type | Access |
|------|------|--------|
| `values` | `Memory<I64>` | public |
| `mins` | `Memory<I64>` | public |
| `maxs` | `Memory<I64>` | public |
| `sampleCounts` | `Memory<I64>` | public |
| `capacity` | `I64` | public |
| `count` | `I64` | public |
| `lock` | `Spinlock` | public |

### Methods

#### `function PerfCounters( self, I64 maxCounters ) -> Void`

Construct a performance counter collection with capacity for the specified maximum number of counters. Each counter is initialized with a value of 0, a minimum of I64 max value (9223372036854775807), a maximum of 0, and a sample count of 0.

**Complexity**:
- Time: `O(n)`
- Space: `O(n)`

#### `function allocate( self ) -> I64`

Allocate a new performance counter and return its ID (index). Returns -1 if all counter slots are exhausted. The allocation is protected by a spinlock for thread-safe concurrent access.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function increment( self, I64 id, I64 delta ) -> Void`

Increment the counter identified by id by the given delta value. Also increments the sample count and updates the minimum and maximum tracked values if the new accumulated value exceeds those bounds. Silently returns if the counter ID is invalid.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function record( self, I64 id, I64 value ) -> Void`

Record an absolute sample value for the counter identified by id, replacing its current value. This is intended for latency tracking where each sample is an independent measurement. Updates the sample count and adjusts the minimum and maximum if the value exceeds them.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function reset( self, I64 id ) -> Void`

Reset the counter identified by id to its initial state: value 0, minimum set to I64 max value, maximum set to 0, and sample count cleared. Silently returns if the counter ID is invalid.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

#### `function getValue( self, I64 id ) -> I64`

Return the current value of the counter at the given ID, or 0 if the ID is invalid. 

#### `function getMin( self, I64 id ) -> I64`

Return the minimum recorded value of the counter at the given ID, or 0 if the ID is invalid. 

#### `function getMax( self, I64 id ) -> I64`

Return the maximum recorded value of the counter at the given ID, or 0 if the ID is invalid. 

#### `function getSampleCount( self, I64 id ) -> I64`

Return the number of samples recorded for the counter at the given ID, or 0 if the ID is invalid. 

#### `function getCount( self ) -> I64`

Return the total number of allocated performance counters. 

#### `function destroy( self ) -> Void`

Free all Memory arrays used for storing counter data.

**Complexity**:
- Time: `O(1)`
- Space: `O(1)`

