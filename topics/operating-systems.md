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
* A **race condition** is what just happened. Two or more threads access shared data and try to change it at the same time.

## Locks, Mutexes, Semaphores

* _Lock_ is a generic term for an object that works like a key and allows the access to a target object only by one thread. Only the thread which owns the key can unlock the "door". Shared data should always only be changed when the door is locked

![](/assets/Screen Shot 2017-01-12 at 5.28.45 PM.png)

* _**Monitor**_ is a lock plus one or more condition variables.

#### Mutex

* Mutex is a _cross-process_ lock, same as a lock but system wide.
* Mutex is a locking mechanism used to synchronize access to a **critical section**.
* Mutex provides \_mut\_ual \_ex\_clusion, either producer or consumer can have the key \(mutex\) and proceed with their work.

#### Semaphore

* Semaphore is signaling mechanism \("I am done, you can carry on" kind of signal\).
* A kind of lock that allows more than one thread to access the target object. It's like having more keys, so that many people can unlock the door.
* Does the same as a lock but allows `x` number of threads to enter

![](/assets/Screen Shot 2017-01-12 at 5.32.06 PM.png)

* In the above example, if you switch the P\(\)s in either function you risk deadlock. \(Mutex changed to 0, but there are no spaces for coke, so emptyBuffers is now 0 and the thread is now asleep. Now no one can do anything because mutex is 0.\) A solution to this is to separate the mutex functionality from the resource control functionality. Use locks for the mutex, and **conditional variables** for the scheduling.

#### Monitors

* Conditional variables allow a thread to sleep \(but be passed to a wait queue to it's not 
* They have three operations: `wait()`, `signal()` \(wake up one waiter\), and `broadcast()`. These methods are always done **inside of** the lock. `wait()` puts you to sleep **and** releases the lock, and when you exit `wait()` you must acquire the lock. `signal()` places the waiter on the ready queue. This separation of mutex and resource control makes it easier to avoid deadlocking the system.

![](/assets/Screen Shot 2017-01-12 at 6.02.39 PM.png)

![](/assets/Screen Shot 2017-01-12 at 6.54.30 PM.png)

![](/assets/Screen Shot 2017-01-12 at 6.54.44 PM.png)

### IPC \(Inter-Process Communication\)

* Sharing data across multiple and commonly specialized processes.
* IPC methods:
  * File – record stored on disk, or a record synthesized on demand by a file server, which can be accessed by multiple processes.
  * Signal – system message sent from one process to another, not usually used to transfer data but instead used to remotely command the partnered process.
  * Socket – data stream sent over a network interface, either to a different process on the same computer or to another computer on the network.
  * Pipe – two-way data stream between two processes interfaced through standard input and output and read in one character at a time.
  * Semaphore – A simple structure that synchronizes multiple processes acting on shared resources.
  * Shared memory – Multiple processes are given access to the same block of memory which creates a shared buffer for the processes to communicate with each other.

## Starvation & Deadlocks

* **Starvation** describes a situation where a thread is unable to gain regular access to shared resources and is unable to make progress. This happens when shared resources are made unavailable for long periods by "greedy" threads. For example, suppose an object provides a synchronized method that often takes a long time to return. If one thread invokes this method frequently, other threads that also need frequent synchronized access to the same object will often be blocked.
* **Deadlock** is a specific type of starvation where two or more threads are blocked forever, waiting for each other.
* Thread A owns Resource 1 \(say, the only cup\) and is waiting for Resource 2 \(say, the only straw\). Thread B owns Resource 2 and is waiting for Resource 3 \(say, the only faucet\). Thread C owns Resource 3 and is waiting for Resource 1.
* There are four conditions for a deadlock. If you get rid of one you'll fix your deadlock:
  * Mutex: When one thread has a resource,  another one can't.
  * Hold and wait: A thread grabs a resource and then waits for a different resource.
  * No preemption: Resources can only be released voluntarily by the thread.
  * Circular wait: Threads are waiting for resources in a circular manner.
* Ways to avoid:
  * Use the Banker's algorithm to keep things in a _safe state_. Making sure that before it gives away a resource, there are enough resources for at least one thread \(using the same resource, but perhaps not having enough in its control\) to finish and give up its resources. \(Can only pick up a chopstick if it's not the last one, or if someone else already has 2\). Ensuring all threads don't enter a waiting state.
  * Don't allow waiting \(like ethernet. Everyone goes for the resource, if collision then retry\)
  * Force resources to be requested in a particular order \(You have to get a cup first, or wait.\)
* Things you can do if it happens:
  * Terminate thread and force it to give up its resources.
  * Make sure that resources can be preempted if necessary.
  * Roll back actions of a thread that is deadlocked.

## Thread Scheduler

* The scheduler is responsible for allocating the available CPU time among the competing threads.
* On multiprocessor systems, there is generally some kind of scheduler per processor, which then need to be coordinated in some way.
* A thread can be in states:  
  ![](/assets/Screen Shot 2017-01-12 at 4.58.27 PM.png)

* Each thread has a quantum, which is effectively how long it is allowed to keep hold of the CPU. Kept in the TCB.

* Priorities differ between OS, and can be partially set by the user.

* Thread quanta are generally defined in terms of some number of clock ticks.

* When an interrupt happens, the scheduler's job is to decide which thread on the ready queue is most appropriate to run on the CPU.
* A clock tick is typically 1ms under Linux.
* A quantum is usually a small number of clock ticks, between 10-100 clock ticks \(i.e. 10-100 ms\) under Linux. If its too big, response time suffers. Too small: too much overhead.
* Round Robin: For `n` threads in the ready queue, and `q = quantum`, each thread gets `1/n` of the CPU time, but never  With this method, no thread ever waits `(1  - n)q` time. `q` must be large enough that there isn't more context switching than executing.
* Shortest Job First / Shortest Remaining Time First: Calculate the amount of time a thread on the ready queue has remaining. If less than current thread then preempt it and give control to the short thread to run to completion. Could lead to starvation of the longer thread if there are a lot of short threads. This is not 100% accurate because it's hard to tell exactly how long a job will run. Can get a good estimate from the average of past runs of this thread.
* Most systems use priority-based round-robin scheduling to some extent:
  * A thread of higher priority \(which is a function of base and local priorities\) will preempt a thread of lower priority.
  * Otherwise, threads of equal priority take turns getting CPU time



