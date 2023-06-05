# Concurrency Patterns

https://www.modernescpp.com/index.php/concurrency-patterns

## Synchronization Patterns

The focus of the synchronization patterns is to deal with sharing and mutation.

### Dealing with Sharing
If you don’t share, no data races can happen. Not sharing means that your thread works on local variables. This can be achieved by a Copied Value, using Thread-Specific storage, or transferring the result of a thread to its associated Future via a protected data channel.

#### Copied Value
If a thread gets its arguments by copy and not by reference, there is no need to synchronize access to any data. No data races and no lifetime issues are possible.

#### Thread-Specific Storage
Thread-specific or thread-local storage allows multiple threads to use local storage via a global access point. By using the storage specifier thread_local, a variable becomes a thread-local variable. This means you can use the thread-local variable without synchronization.

#### Futures
C++11 provides futures and promises in three flavors: std::async, std::packaged_task, and the pair std::promise and std::future. The future is a read-only placeholder for the value that a promise sets. From the synchronization perspective, a promise/future pair’s critical property is that a protected data channel connects both.

### Dealing with Mutation
No data race can happen if you don’t write and read data concurrently. First, protect the critical sections with a lock such as a Scoped or Strategized Locking. In object-oriented design, a critical section is typically an object, including its interface. The Thread-Safe Interface protects the entire object. The idea of the Guarded Suspension is to send a signal when it its done with its work.

#### Scoped Locking
Scoped locking is the idea of RAII applied to a mutex. The key idea of this idiom is to bind the resource acquisition and release to an object’s lifetime. As the name suggests, the lifetime of the object is scoped. Scoped means that the C++ run time is responsible for object destruction and, therefore, for releasing the resource.

#### Strategized Locking
Strategized locking is the idea of the strategy pattern applied to locking. This means putting your locking strategy into an object and making it into a pluggable component of your system.

#### Thread-Safe Interface
The Thread-Safe Interface fits very well when the critical sections are just objects. The naive idea to protect all member functions with a lock causes, in the best case, a performance issue and, in the worst case, a deadlock.

The thread-safe interface overcomes both issues. Here is the straightforward idea:

*All interface member functions (public) should use a lock.
*All implementation member functions (protected and private) must not use a lock.
The interface member functions call only protected or private member functions but no public member functions.
Guarded Suspension
The guarded suspension basic variant combines a lock and a precondition that must be satisfied. If the precondition is not fulfilled, that calling thread puts itself to sleep. The checking thread uses a lock to avoid a race condition that may result in a data race or a deadlock.

Various variants exist:

The waiting thread can passively be notified about the state change or actively ask for the state
change.
The waiting can be done with or without a time boundary.
The notification can be sent to one or all waiting threads.
