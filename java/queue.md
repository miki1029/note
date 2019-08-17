# Queue Implementations

## General-Purpose Queue Implementations

|             | *Throws exception*                                           | *Returns special value*                                      |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Insert**  | [`add(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#add-E-) | [`offer(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#offer-E-)<br />실패시 false |
| **Remove**  | [`remove()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#remove--) | [`poll()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#poll--)<br />실패시 null |
| **Examine** | [`element()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#element--) | [`peek()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#peek--)<br />실패시 null |

* 대표적인 구현체
  * **LinkedList**
  * PriorityQueue
    * ordering
      * default : natural ordering
      * or use explicit Comparator
    * iterator 주의점
      * 특정 순서로 순회한다고 보장하지 않는다.
        * 순서를 보장 받고 싶다면 내부 배열 `pq.toArray()`을 정렬한 후 iterator를 호출하면 정렬된 결과를 가져올 수 있다.
        *  `Arrays.sort(pq.toArray())`
      * iterator method를 사용해 반복자를 가져온 후 큐에 요소를 집어넣고 다시 반복자로 요소에 접근하면 `ConcurrentModificationException`가 발생한다.

## Concurrent Queue Implementations

* `java.util.concurrent` package
* thread-safe

### BlockingQueue

|             | *Throws exception*                                           | *Special value*                                              | *Blocks*                                                     | *Times out*                                                  |
| ----------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Insert**  | [`add(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#add-E-) | [`offer(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#offer-E-) | [`put(e)`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#put-E-) | [`offer(e, time, unit)`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#offer-E-long-java.util.concurrent.TimeUnit-) |
| **Remove**  | [`remove()`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#remove-java.lang.Object-) | [`poll()`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#poll-long-java.util.concurrent.TimeUnit-) | [`take()`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#take--) | [`poll(time, unit)`](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html#poll-long-java.util.concurrent.TimeUnit-) |
| **Examine** | [`element()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#element--) | [`peek()`](https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html#peek--) | *not applicable*                                             | *not applicable*                                             |

* 큐가 꽉찼을 때 삽입 시도 / 큐가 비었을 때 추출 시도를 막는다.(wait)
* 대표적인 구현체
  * **LinkedBlockingQueue** : node
    * bounded / unbounded
    * capacity 초기값 Integer.MAX_VALUE
    * node는 삽입시 동적으로 생성
  * ArrayBlockingQueue : array
    * bounded
    * 생성 후 크기 변경 불가
    * 대기열 처리에 대한 순서 보장 없음
  * PriorityBlockingQueue : heap
    * unbounded
  * DelayQueue : heap
    * time-based scheduling queue
  * SynchronousQueue
    * [rendezvous](https://en.wikipedia.org/wiki/Rendezvous_(Plan_9)) mechanism
  * LinkedTrasferQueue : node
    * `LinkedTrasferQueue` implements `TrasferQueue` extends `BlockingQueue`
    * unbounded
    * 값을 추출할 때 wait 할지 여부를 직접 결정할 수 있음

# Reference

https://docs.oracle.com/javase/tutorial/collections/implementations/queue.html

https://docs.oracle.com/javase/8/docs/api/java/util/Queue.html

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/BlockingQueue.html

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/TransferQueue.html

<http://oniondev.egloos.com/558949>

https://202psj.tistory.com/990