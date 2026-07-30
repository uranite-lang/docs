# uranite.async

## Table of Contents

- [Imports](#imports)

## Imports

- `uranite.async.context`
  - `makeContext`
  - `swapContext`
- `uranite.async.epoll`
  - `EPOLLERR`
  - `EPOLLET`
  - `EPOLLHUP`
  - `EPOLLIN`
  - `EPOLLOUT`
  - `allocateEventBuffer`
  - `epollAdd`
  - `epollCreate`
  - `epollDel`
  - `epollEventGetEvents`
  - `epollEventGetFd`
  - `epollMod`
  - `epollWait`
- `uranite.async.runtime`
  - `runtimeAwait`
  - `runtimeAwaitFd`
  - `runtimeComplete`
  - `runtimeError`
  - `runtimeHasPending`
  - `runtimeInit`
  - `runtimeRun`
  - `runtimeSleep`
  - `runtimeSpawn`
  - `runtimeTaskGetError`
  - `runtimeTaskHasError`
  - `runtimeTaskResult`
  - `runtimeTaskState`
  - `runtimeYield`
- `uranite.async.scheduler`
  - `NativeScheduler`
- `uranite.async.task`
  - `createTask`
  - `taskAllocateStack`
  - `taskDestroy`
  - `taskInitContext`
  - `taskIsFinished`
  - `taskRspAddr`
- `uranite.async.timer`
  - `timerfdClose`
  - `timerfdCreate`
  - `timerfdSettime`

## Exported Symbols

- `EPOLLERR`
- `EPOLLET`
- `EPOLLHUP`
- `EPOLLIN`
- `EPOLLOUT`
- `NativeScheduler`
- `allocateEventBuffer`
- `createTask`
- `epollAdd`
- `epollCreate`
- `epollDel`
- `epollEventGetEvents`
- `epollEventGetFd`
- `epollMod`
- `epollWait`
- `makeContext`
- `runtimeAwait`
- `runtimeAwaitFd`
- `runtimeComplete`
- `runtimeError`
- `runtimeHasPending`
- `runtimeInit`
- `runtimeRun`
- `runtimeSleep`
- `runtimeSpawn`
- `runtimeTaskGetError`
- `runtimeTaskHasError`
- `runtimeTaskResult`
- `runtimeTaskState`
- `runtimeYield`
- `swapContext`
- `taskAllocateStack`
- `taskDestroy`
- `taskInitContext`
- `taskIsFinished`
- `taskRspAddr`
- `timerfdClose`
- `timerfdCreate`
- `timerfdSettime`

