# Operating Systems

## What is the OS?

* The OS provides and interface to the hardware for the user.
* It is responsible for partitioning and protecting the computer's resources.
* `Byte` is the smallest addressable unit of memory.
* The OS protects memory in 2 ways, address translation and dual mode operation.
  * Address translation: The addresses that a process sees are not the physical addresses that the CPU knows about. A process thinks it has all of the resources of the computer, but it really doesn't. Addresses are put through an address map.
  * Dual Mode Operation: Critical operations are only performed in **kernel mode, **as opposed to the **user mode** that most operations are performed in.
* The stack is the memory set aside as scratch space for a thread of execution. When a function is called, a block \(also called _stack frame_\) is reserved on the top of the stack for local variables and some bookkeeping data. When that function returns, the block becomes unused and can be used the next time a function is called. The stack is always reserved in a LIFO order; the most recently reserved block is always the next block to be freed. This makes it really simple to keep track of the stack; freeing a block from the stack is nothing more than adjusting one pointer.
* The heap is memory set aside for dynamic allocation. Unlike the stack, there's no enforced pattern to the allocation and deallocation of blocks from the heap; you can allocate a block at any time and free it at any time.
* Each thread gets a stack, while there's typically only one heap for the application.
* The size of the stack is set when a thread is created. The size of the heap is set on application startup, but can grow as space is needed \(the allocator requests more memory from the operating system\).
* The stack is faster because the access pattern makes it trivial to allocate and deallocate memory from it \(a pointer/integer is simply incremented or decremented\), while the heap has much more complex bookkeeping involved in an allocation or memory freeing. Also, each byte in the stack tends to be reused very frequently which means it tends to be mapped to the processor's cache, making it very fast.
* _Virtual memory_ technique virtualizes the main storage available to a process or task, as a contiguous address space which is unique to each running process, or virtualizes the main storage available to all processes or tasks on the system as a contiguous global address space.
* [Virtual memory illustrated](http://en.wikipedia.org/wiki/File:Virtual_memory.svg)

## Processes

* A process has a self-contained execution environment. A process generally has a complete, private set of basic run-time resources; in particular, each process has its own memory space.
* Each process has it's own memory space, and addresses memory inside that space.
* Each process can contain many threads.
* Threads share the memory space of its parent process.
* Process resources:
  * Working directory
  * Environment variables
  * File handles
  * Sockets
  * Address space

## Threads

* The **TCB **\(thread control block\) holds the registers \(stack pointer and program counter\), and scheduling info \(CPU time and priority\). It is the representation of the thread inside of the kernel. 
* Thread creation is a costly operation, and resources need to be constructed and obtained for a new thread.
* Creating and tearing down threads isn't free: there'll be some CPU overhead each time we do so.
* There may be some moderate limit on the number of threads that can be created, determined by the resources that a thread needs to have allocated \(if a process has 2GB of address space, and each thread has 512KB of stack, that means a maximum of a few thousands threads per process\). 
* Solution: use a threadpool.

## Concurrency

* Hyper-threading / simultaneous multi-threading is when multiple threads are running on the CPU at the same time because all of the execution engines aren't necessary for one or the other.
* **Context switching** is the procedure that takes place when the system switches between threads running on the CPU. Caused by both internal and external events:
  * * Internal: Blocking on I/O, waiting for a signal from another thread \(ex: semaphore\), thread voluntarily yeilds.
    * External: Time slicing,  interrupts \(keyboard, printer, packets coming into the network, etc\).
* Every time the thread running on a CPU actually changes there'll be some negative impact due to the interruption of the instruction pipeline.
* Switching between threads of different processes will carry a higher cost, since the address-to-memory mappings must be changed, and the contents of the cache almost certainly will be irrelevant to the next process.
* Imagine you go to deposit $1,000 into an ATM at the same time your wife is depositing $10 into the same shared account at another ATM. The loop for `Deposit(dep_amount)` is  `curBal = GetBalance()` \(Blocks on I/0, control is passed to another thread\) -&gt; `UpdateAmount(dep_amount + curBal)`. The CPU returns a current amount of $0 to your ATM and $0 to your wife's. It updates with $1,000, the updates with $10. You now have $10 in your account instead of $1,010. That code block should have been **atomic**, meaning it always runs to completion or not at all.

## Locks, Mutexes, Semaphores

* _Lock_ is a generic term for an object that works like a key and allows the access to a target object only by one thread. Only the thread which owns the key can unlock the "door".
* _Monitor_ is a _cross-thread_ lock.
* Lock allows only one thread to enter the part that's locked and the lock is not shared with any other processes.

#### Mutex

* Mutex is a _cross-process_ lock, same as a lock but system wide.
* Mutex is a locking mechanism used to synchronize access to a **critical section**.
* Mutex provides \_mut\_ual \_ex\_clusion, either producer or consumer can have the key \(mutex\) and proceed with their work.

#### Semaphore

* Semaphore is signaling mechanism \("I am done, you can carry on" kind of signal\).
* A kind of lock that allows more than one thread to access the target object. It's like having more keys, so that many people can unlock the door.
* If upper bound is set to one \(1\) it's the same as a **monitor**.
* Does the same as a lock but allows `x` number of threads to enter.

![](/assets/Screen Shot 2017-01-12 at 5.32.06 PM.png)

Implementing a semaphore.

![](/assets/Screen Shot 2017-01-11 at 9.59.44 PM.png)

## Deadlocks, Livelocks, Starvation

* **Deadlock** describes a situation where two or more threads are blocked forever, waiting for each other.
* Deadlock is the phenomenon when, typically, two threads each hold an exclusive lock that the other thread needs in order to continue.
* In principle, there could actually be more threads and locks involved.
* Deadlock examples:
  * Locking on two different bank account with two threads, in an opposite order.
  * Two friends bowing to each other, and each one only gets up when the other one gets up.
* How to avoid – make locking order fixed \(e.g: always lock on the smaller bank account first, sort by identity hash code, etc.\).
* **Livelock** – A thread often acts in response to the action of another thread. If the other thread's action is also a response to the action of another thread, then livelock may result. As with deadlock, livelocked threads are unable to make further progress. However, the threads are not blocked – they are simply too busy responding to each other to resume work.
* This is comparable to two people attempting to pass each other in a corridor: Alphonse moves to his left to let Gaston pass, while Gaston moves to his right to let Alphonse pass. Seeing that they are still blocking each other, Alphonse moves to his right, while Gaston moves to his left. They're still blocking each other, so...
* Two thread can communicate with `wait();` and `notifyAll();` \(see Java ITC\)
* Livelock examples:
  * Two people passing in the corridor
  * Two threads trying to simultaneously avoid a deadlock
  * Husband and wife eating dinner with 1 spoon, and keep passing the spoon to the other spouse
* How to avoid live-locking – try to add some randomness.
* **Starvation** describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads. For example, suppose an object provides a synchronized method that often takes a long time to return. If one thread invokes this method frequently, other threads that also need frequent synchronized access to the same object will often be blocked.

## Race Conditions

* A race condition occurs when 2 or more threads are able to access shared data and they try to change it at the same time. Because the thread scheduling algorithm can swap between threads at any point, you don't know the order at which the threads will attempt to access the shared data. Therefore, the result of the change in data is dependent on the thread scheduling algorithm, i.e. both threads are 'racing' to access/change the data.
* Often problems occur when one thread does a "check-then-act" \(e.g. "check" if the value is X, and then "act" to do something that depends on the value being X\) and another thread does something to the value in between the "check" and the "act".
* In order to prevent race conditions occurring, typically you would put a lock around the shared data to ensure that only one thread can access the data at a time.

## Thread Scheduler

* Responsible for sharing out the available CPUs in some way among the competing threads.
* On multiprocessor systems, there is generally some kind of scheduler per processor, which then need to be coordinated in some way.
* Most systems use priority-based round-robin scheduling to some extent:
  * A thread of higher priority \(which is a function of base and local priorities\) will preempt a thread of lower priority.
  * Otherwise, threads of equal priority will essentially take turns at getting an allocated slice or quantum of CPU.
* A thread can be in states:
  * `New` – created and waiting to create needed resources
  * `Runnable` – waiting for CPU allotment
  * `Waiting` – cannot continue, waiting for a resource like lock/IO/memory to be paged/sleep to finish/etc.
  * `Terminated` – finished by waiting to clear resources
* Each thread has a quantum, which is effectively how long it is allowed to keep hold of the CPU.
* Thread quanta are generally defined in terms of some number of clock ticks.
* A clock tick is typically 1ms under Linux.
* A quantum is usually a small number of clock ticks, between 10-200 clock ticks \(i.e. 10-200 ms\) under Linux.
* At key moments, the thread scheduler considers whether to switch the thread that is currently running on a CPU. These key moments are usually: periodically, if a thread ceases to be runnable, when some other attribute of the thread changes.
* At these decision points, the scheduler's job is essentially to decide, of all the runnable threads, which are the most appropriate to actually be running on the available CPUs.
* Priorities differ between OS, and can be partially set by the user.
* Great article on [CPU Clocks and Clock Interrupts, and Their Effects on Schedulers](http://accu.org/index.php/journals/2185).

### IPC \(Inter-Process Communication\)

* Sharing data across multiple and commonly specialized processes.
* IPC methods:
  * File – record stored on disk, or a record synthesized on demand by a file server, which can be accessed by multiple processes.
  * Signal – system message sent from one process to another, not usually used to transfer data but instead used to remotely command the partnered process.
  * Socket – data stream sent over a network interface, either to a different process on the same computer or to another computer on the network.
  * Pipe – two-way data stream between two processes interfaced through standard input and output and read in one character at a time.
  * Named pipe – pipe implemented through a file on the file system instead of standard input and output.
  * Semaphore – A simple structure that synchronizes multiple processes acting on shared resources.
  * Shared memory – Multiple processes are given access to the same block of memory which creates a shared buffer for the processes to communicate with each other.

### Process Signaling

* Signals are a limited form of inter-process communication used in Unix, Unix-like, and other POSIX-compliant operating systems.
* A signal is an asynchronous notification sent to a process.
* If the process has previously registered a signal handler, that routine is executed. Otherwise, the default signal handler is executed.
* Some signals may be ignored by an application \(like `SIGTERM`\).
* The kernel can generate signals to notify processes of events.
* For example, `SIGPIPE` will be generated when a process writes to a pipe which has been closed by the reader.
* There are two signals which cannot be intercepted and handled: `SIGKILL` \(terminate immediately, no cleanup\) and `SIGSTOP`.
* Pressing `Ctrl+C` in the terminal sends `SIGINT` \(interrupt\) to the process; by default, this causes the process to terminate.
* `$ kill -9` will send a `SIGKILL`.
* Notable signals:
  * `SIGINT` \(2\) – Terminal interrupt
  * `SIGQUIT` \(3\) – Terminal quit signal
  * `SIGKILL` \(9\) – Kill \(cannot be caught or ignored\)
  * `SIGTERM` \(15\) – Termination
  * `SIGSTOP` \(na\) – Stop executing \(cannot be caught or ignored\)
* In the JVM we can register for `SIGTERM`/`SIGINT` by: `Runtime.getRuntime().addShutdownHook(new Thread(...));`



