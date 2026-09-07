# immutable-collection

Persistent (immutable) `Stack` and `Queue` in Java. Every operation returns a new
collection; the original is never mutated, so a value can be shared freely across threads
and held as a snapshot.

## Stack

A singly-linked persistent stack. `push` allocates one node pointing at the current stack;
`pop` returns the tail. Structural sharing means both operations are O(1) and no copying
happens.

```java
Stack<Integer> s = ImmutableStack.initEmptyStack();
Stack<Integer> s1 = s.push(1).push(2);   // s is still empty
s1.head();       // 2
s1.pop().head(); // 1
```

`EmptyStack` is a singleton sentinel that throws on `head`/`pop` and reports `isEmpty()`.

## Queue

The classic **two-stack queue** (Okasaki). An `in` stack takes `enQueue`; `head`/`deQueue`
read from an `out` stack. When `out` empties, `in` is reversed into it — each element is
moved at most twice, giving amortized O(1) `deQueue`.

```java
Queue<Integer> q = ImmutableQueue.getEmptyQueue()
        .enQueue(1).enQueue(2).enQueue(3);
q.head();            // 1
q.deQueue().head();  // 2
```

## Build

```bash
mvn test
```

Java 8 · JUnit 4. Covered by `ImmutableStackTest` / `ImmutableQueueTest`.
