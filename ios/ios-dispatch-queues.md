# Dispatch Queues

### GCD(Grand Central Dispatch)란?

> iOS 에서 Multi-threading 처리를 위한 C 기반의 저수준 API

- 참고 : Obj-C 기반의 NSOperation 이라는 고수준 API 도 있다.

## Dispatch Queues

> 앱에서 task를 비동기적으로 동시에 수행할 수 있는 클래스이다.

- 두가지 종류로 나눌 수 있다.
1. **Serial** Dispatch Queue
2. **Concurrent** Dispatch Queue
- 두가지 모두 실행 중인 task 는 dispatch queues 에서 관리하는 고유한 쓰레드에서 실행된다.

### 1. Serial Queue

- 큐에 등록된 순서대로 한번에 하나의 task 를 FIFO 방식으로 처리한다. 처리 중인 작업이 완료되면 다음 작업을 처리한다. 즉, thread 하나로 순차 처리를 하는 것이다.
- 즉, Serial 을 4개 작성하면 각 큐는 한번에 하나의 작업만 실행하지만, 최대 4개의 작업이 각 큐에서 동시에 실행될 수 있다.
- 주로 특정 자원에 대한 엑세스를 동기화 하는데 사용된다.

### 2. Concurrent Queue

- 등록된 작업을 한번에 하나씩 처리하지 않고, 여러 작업들을 동시에 처리한다.
- 작업들을 동시에 별도의 쓰레드에 수행시킨다.
- 동시에 실행되는 작업 수는 시스템에 따라 달라지며, 이것을 판단 및 관리는 XNU 커널이 알아서 해준다.

### ***!! Application 실행 시 시스템에서 기본적으로 2개의 Queue를 미리 만들어 준다.***

### 1. Main Queue

- 메인 쓰레드(UI 쓰레드)에서 사용되는, 전역적으로 사용 가능한 `Serial Queue`이다.
- *tip*: 모든 UI 처리는 `main thread` 에서 처리한다.
- `main thread` 에서 실행 되므로, 주로 앱의 주요 동기화 지점으로 사용된다!*
    - `DispatchQueue.main.async { code }`
- DispatchQueue.main.sync 하면 절대 안된다.

### 2. Global Queue

- 편의상 사용할 수 있게 만들어 놓은 `Concurrent Queue`이다.
- 병렬적으로 동시에 처리를 하기 때문에 작업 완료의 순서는 정할 수 없지만, 우선적으로 일을 처리하게 할 수 있다.
- Global Queue는 처리 우선 순위를 위한 `QOS(Quality of Service)` 파라미터를 제공한다.

### QOS 우선순위

```
높은 순서대로
1. userInteractive
2. userInitiated
3. default
4. utility
5. background
6. unspecified
```

## sync / async

> Serial / Concurrent 와 sync / async 는 별개이다.

- 즉, 직렬인데 비동기 일수도 있고, 병렬인데 동기 일수도 있다.
- Serial / Concurrent : 한번에 하나만 처리 / 동시에 여러 개를 처리의 개념
- sync / async : 처리가 끝날 때까지 wait / 등록 만하고 다른 처리의 개념

### sync

- 큐에 작업을 등록 했으면 그게 끝날 때까지 **“기다리는”** 것이다.
- `blocking` 이라고 생각하면 된다.

### async

- 일단 큐에 작업을 등록하고, 다른 작업을 할 수 있는 것이다.
- `non-blocking` 이라고 생각하면 된다.