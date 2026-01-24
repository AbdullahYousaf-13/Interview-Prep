# Week 1-2 (Python)

## Week 1

### 🧠 DAY 1 — Python Memory Model & Internals (Backend Perspective)

**🎯 Day-1 Goal (Very Important)**

By the end of today, you should be able to confidently explain:

- Where data lives in Python (stack vs heap)
- Why Python behaves “weirdly” with mutability
- Why Python consumes more memory under load
- How memory decisions affect backend scalability

#### Block 1 – Mental Model (Names vs Objects)

**Core Mindset Shift**

Wrong mental model: Variables store values

Correct mental model: Variables are names that reference objects

Python variables do not contain data. They point to objects that live independently in memory. Assignment creates a new reference, not a copy.

**Code:**

    a = 10
    b = a

**Explanation:**

`10` is an object

`a` and `b` both reference the same object

No data copying occurs

**Backend Relevance**

- Explains unexpected shared state
- Prevents cache mutation bugs
- Helps reason about API side effects

**Interview One-Liner**

Python uses name binding. Variables reference objects rather than storing values directly.

#### BLOCK 2 — Stack vs Heap (Python Reality)

##### 1️⃣ Big Picture (Very Important)

In Python, memory management works very differently from C/C++.

👉 Python does NOT store objects on the stack.
👉 Only references (names) live on the stack.
👉 All actual objects live on the heap.

This is non-negotiable and often tested.

##### 2️⃣ What Is the Stack in Python?

🔹 Stack = Function Call Stack (Frames)

The stack in Python contains:

- Function call frames
- Local variable references
- Parameters (as references)
- Return addresses

Each time a function is called, Python creates a stack frame.

**Code:**

    def foo():
        x = 10
        y = [1, 2, 3]
        bar(y)
    
    def bar(z):
        z.append(4)
    
    foo()

Stack View (Simplified)
|------------------|
| bar() frame      |
| z  → [1,2,3]     |
|------------------|
| foo() frame      |
| x  → 10          |
| y  → [1,2,3]     |
|------------------|


⚠️ Notice

`x`, `y`, `z` are references

The actual `10` and `[1,2,3]` are NOT on stack

##### 3️⃣ What Is the Heap in Python?

🔹 Heap = All Python Objects

The heap stores:

- Integers
- Floats
- Strings
- Lists
- Tuples
- Dictionaries
- Sets
- Custom class objects
- Functions themselves

**Heap View**

`Heap Memory:
  10
  [1, 2, 3, 4]`

The stack only points to these objects.

##### 4️⃣ Why Python Cannot Use Stack Objects Like C++

C++ (Stack Allocation)
   
    int x = 10;        // stored directly on stack
    
    Python (Reference Model)
    x = 10

Python must support:

- Dynamic typing
- Objects of unknown size
- Automatic memory management
- Garbage collection

➡️ Stack allocation is too rigid for Python.

##### 5️⃣ “Only References Live on Stack” — Explained Clearly

**Code:**

    a = 5
    b = a

What Really Happens

`Stack:
  a → 5
  b → 5`

`Heap:
  5`

Both `a` and `b` point to the same object.

##### 6️⃣ Mutable vs Immutable Objects (Critical Interview Point)

**Immutable Example**

**Code:**

    a = 10
    b = a
    b = 20

What happens?

`10` remains unchanged

`b` now points to a new object

`Stack:
  a → 10
  b → 20`

`Heap:
  10
  20`

**Mutable Example**

**Code:**

    a = [1, 2]
    b = a
    b.append(3)

**Result:**

`print(a)  # [1, 2, 3]`

**Why?**

- Both references point to same heap object
- Mutation happens in heap

##### 7️⃣ Function Calls — Stack + Heap Interaction

**Code:**

    def modify(lst):
        lst.append(99)
    
    nums = [1, 2, 3]
    modify(nums)

**Memory Explanation:**

- nums → heap list
- lst → same heap list
- Stack frame destroyed after function returns
- Heap object remains alive

##### 8️⃣ Garbage Collection & Reference Counting

Python primarily uses reference counting.

**Code:**

    x = [1, 2, 3]
    y = x
    del x

Heap object still exists because:

Reference count = 1

Only when:

`del y`

➡️ Object becomes eligible for garbage collection.

##### 9️⃣ Why Stack Access Is “Fast” in Python

**Stack access is fast because:**

- Stack frames are small
- They only store references
- No object traversal required

**Heap access is slower because:**

- Indirection through references
- Memory fragmentation
- GC tracking

**🔑 Interview-Ready Summary**

In Python, the stack stores function call frames and local variable references, while all actual objects live on the heap. Python never places objects directly on the stack like C++. Variables are merely names pointing to heap-allocated objects. This design supports dynamic typing, mutability, and garbage collection.

#### Block 3 – Mutability & References (Critical)

**Immutable Types:**

- int
- float
- str
- tuple
- frozenset

Immutable means the object cannot be changed. Any “modification” creates a new object.

**Code:**

    a = 10
    b = a
    b += 1

**Explanation:**

`b += 1` creates a new int object

`a` remains unchanged

**Mutable Types:**

- list
- dict
- set
- custom objects

Mutable objects can be modified in place.

**Code:**

    a = [1, 2, 3]
    b = a
    b.append(4)

**Explanation:**

- Both names reference the same list
- Mutation affects all references

**Code:**

    Function Argument Trap (Very Common)
      def add_item(lst):
          lst.append(100)
      
      my_list = []
      add_item(my_list)

**Explanation:**

- Python passes object references by value
- Function mutates the original list

**Backend Impact**

- Shared in-memory state bugs
- Session corruption
- Cache poisoning

**🔑 Interview Level Summary**

For immutable types, any modification creates a new object and rebinds the variable instead of mutating the original.

#### BLOCK 4 — Reference Counting & Garbage Collection (Python Reality)

##### 1️⃣ Python’s Primary GC: Reference Counting

**Core Rule:**

- Every Python object keeps a counter of how many references point to it.
- If the count becomes zero → Python immediately frees the object.

##### 2️⃣ Understanding sys.getrefcount()

**Code:**

    import sys
    
    a = []
    print(sys.getrefcount(a))
    
    b = a
    print(sys.getrefcount(a))
    
    del b
    print(sys.getrefcount(a))

##### 3️⃣ Line-by-Line Explanation (Memory Level)

**Line 1:**

    a = []

**Memory:**

Stack:
  `a → L1`

Heap:
  `L1: []`


References to L1:

`a`

Temporary reference inside `getrefcount()`

**Line 2:**

    print(sys.getrefcount(a))

**⚠️ Important Trap**

`getrefcount(a)` itself adds one temporary reference.

So if you see:

`2`

It means:

`1` reference from `a`

`1` temporary reference from function call

**Line 3:**

    b = a

**Now:**

Stack:
  `a → L1
  b → L1`


Reference count increases by `1`.

**Line 4:**

    print(sys.getrefcount(a))

**Expected output:**

`3`

**Why?**

`a`

`b`

Temporary reference from `getrefcount`

**Line 5:**

    del b

`b` reference removed

Reference count decreases by `1`

**Line 6:**

    print(sys.getrefcount(a))

**Output:**

`2`

Still includes the temporary reference.

##### 4️⃣ Why Reference Count Increases

**Because:**

- Variable assignment creates a new reference
- Function arguments create temporary references
- Containers (lists, dicts) store references
- Global variables keep references alive
- Python never copies objects implicitly.

##### 5️⃣ Why Python Frees Memory Automatically

**Key Mechanism:**

Reference Count → 0 → Immediate Deallocation

**This gives Python:**

- Predictable memory release
- Deterministic cleanup (unlike JVM)

**Code:**

    x = []
    del x  # object freed immediately

⚠️ In CPython only (interview detail).

##### 6️⃣ The Big Problem: Circular References

**Code:**

    a = []
    b = []
    a.append(b)
    b.append(a)

**Memory:**

`a → b
b → a`

**Reference counts:**

- `a references b`
- `b references a`

**Even if:**

- `del a`
- `del b`

👉 Reference counts never reach zero.

##### 7️⃣ Secondary GC: Cyclic Garbage Collector

**Python solves this with a secondary GC:**

- Periodically scans heap
- Detects unreachable cycles
- Frees them

**This GC is:**

- Generational
- Non-deterministic
- Slower than ref counting

##### 8️⃣ Why Circular References Are Special (Interview Gold)

Reference counting cannot detect cycles.

**Why?**

Because objects in a cycle keep each other alive artificially.

**GC must:**

- Pause execution
- Traverse object graph
- Detect unreachable subgraphs

This is expensive.

##### 9️⃣ Backend Relevance (VERY Important)

**Memory Leaks in Long-Running APIs:**

**Example:**

- Django / FastAPI service
- Global caches
- ORM objects holding circular refs
- Closures capturing objects

**Result:**

- Heap grows → RAM spikes → OOM → crash

**🧟 Zombie Objects in Cache-Heavy Systems**

- LRU caches
- In-memory dicts
- Weak reference misuse

**Objects appear “unused” but:**

- Still referenced somewhere
- Never collected

Example:🔄 Why Services Are Restarted Periodically

**Even with GC:**

- Fragmentation happens
- Native extensions leak memory
- Cyclic GC misses edge cases
- Memory is not always returned to OS

**Hence:**

- Rolling restarts = safety valve

**🔑 Interview-Ready Summary**

Python primarily uses reference counting for garbage collection, where objects are freed immediately when their reference count reaches zero. However, reference counting cannot handle circular references, so Python uses a secondary cyclic garbage collector to detect and clean unreachable cycles. In long-running backend systems, improper reference handling can cause memory leaks, which is why services often require periodic restarts.

#### Block 5 — Interview-Grade Memory Traps

##### 1️⃣ Shallow vs Deep Copy

**Code:**

    import copy
    
    a = [[1, 2], [3, 4]]
    b = copy.copy(a)
    c = copy.deepcopy(a)
    
**Shallow Copy (copy.copy):**

What it does:

- Creates a new outer list
- Does NOT copy inner objects

Memory idea:

`a → [ L1 , L2 ]
b → [ L1 , L2 ]`

So:

    b[0].append(99)
    print(a)   # [[1, 2, 99], [3, 4]]

👉 Inner lists are shared

**Deep Copy (copy.deepcopy):**

What it does:

- Copies everything
- New outer list
- New inner lists

Memory idea:

`a → [ L1 , L2 ]
c → [ L3 , L4 ]`

So:

    c[0].append(99)
    print(a)   # unchanged

**Key Interview Lines:**

- Shallow copy: copies container, shares contents
- Deep copy: copies entire object graph
- Deep copy is expensive (time + memory)

**Use Case:**

- Shallow copy → performance, read-only nested data
- Deep copy → isolation, safety

##### 2️⃣ Default Mutable Argument Trap (EXTREMELY COMMON)

**Problem Code:**

    def add_item(item, lst=[]):
        lst.append(item)
        return lst

Looks innocent ❌, But it’s dangerous.

**Why This Is a Bug:**

- Default arguments are evaluated once
- That list is created at function definition
- Same list reused every call

**Example:**

    add_item(1)  # [1]
    add_item(2)  # [1, 2]  ❌
    add_item(3)  # [1, 2, 3]

👉 Same list shared forever

##### 3️⃣ Correct Pattern (Always Use This)
def add_item(item, lst=None):
    if lst is None:
        lst = []
    lst.append(item)
    return lst

**Why this works:**

- None is immutable
- New list created per call
- No shared state

##### 4️⃣ Backend Impact (Why Interviewers Care)

**🚨 Data Leakage**

- One user’s data appears in another request

**🚨 Stateful Bugs**

- APIs behave differently over time

**🚨 Under Load**

- Bugs appear only after many requests
- Hard to debug, easy to fail production

**🔑 Interview-Ready Summary**

Shallow copy duplicates only the outer container while inner objects remain shared, whereas deep copy duplicates the entire object graph at a higher cost. Default mutable arguments are evaluated once at function definition time, causing shared state across calls. This can lead to serious bugs in backend systems, so the safe pattern is to use None and create a new object inside the function.

#### Block 6 – Interview Self-Check

You should be able to answer clearly:

##### 1️⃣ Why is Python memory-heavy?

Python is memory-heavy because everything is an object.

**Each object stores:**

- Type information
- Reference count
- Metadata
- Pointers to other objects

**On top of that:**

- Variables store references, not raw values
- Containers store references to objects, not objects themselves

➡️ This flexibility (dynamic typing, mutability, GC) trades performance and memory efficiency for developer productivity.

##### 2️⃣ Why are mutable defaults dangerous?

Mutable default arguments are evaluated once at function definition, not per call.
That means the same object is shared across all calls, leading to hidden shared state.

**In backend systems, this causes:**

- Data leaking between requests
- Stateful bugs that worsen over time
- Unpredictable behavior under load

➡️ This is why None is used as the safe default.

##### 3️⃣ Where do Python objects live?

All Python objects live on the heap.

**The stack only stores references, such as:**

- Local variable names
- Function parameters
- Call frames

➡️ Python never places objects directly on the stack like C++.
This allows dynamic typing, resizing, and garbage collection.

##### 4️⃣ How does Python clean memory automatically?

**Python primarily uses reference counting:**

- Every object tracks how many references point to it
- When the count reaches zero, the object is freed immediately

➡️ Because reference counting can’t detect cycles, Python also runs a secondary cyclic garbage collector to clean circular references.

##### 5️⃣ Why does this matter for backend APIs?

**Backend services are:**

- Long-running
- Concurrent
- Memory-sensitive

**Poor understanding of Python memory leads to:**

- Memory leaks
- Shared state bugs
- Cache corruption
- Gradual RAM growth and crashes

➡️ **That’s why backend engineers must:**

- Avoid shared mutable state
- Use safe defaults
- Understand GC behavior
- Restart services strategically

**One-Line Final Takeaway (Gold):**

Python’s memory model favors flexibility and safety over raw efficiency, but misunderstanding references, mutability, and garbage collection leads directly to production-level backend bugs.

**`If you cannot explain these 5 verbally, revision is required.`**

---

### Day 2 – GIL & Concurrency (Backend Engineering Perspective)

**Objective**

Understand how Python executes code concurrently, why the Global Interpreter Lock (GIL) exists, and how backend systems scale despite it.

**Why This Day Matters**

Most candidates say “Python is slow” or “GIL blocks threads” without understanding implications. Interviewers are testing whether you can design scalable backend systems despite Python’s constraints.

**Global Interpreter Lock (GIL)**

The Global Interpreter Lock is a mutex that ensures only one thread executes Python bytecode at a time in a single process.

**Why the GIL Exists**

- Simplifies memory management
- Makes reference counting thread-safe
- Avoids complex locking around objects

Python chose simplicity and safety over raw CPU parallelism.

**What the GIL Does NOT Mean**

- Python cannot do concurrency ❌
- Python cannot scale ❌
- Python is unusable for backend ❌

The GIL mainly affects CPU-bound tasks, not IO-bound tasks.

**CPU-Bound vs IO-Bound Tasks**

**CPU-Bound:**

- Heavy computation
- Encryption
- Image processing
- ML inference

Threads do NOT help due to the GIL.

**IO-Bound:**

- API calls
- Database queries
- File reads/writes
- Network requests

Threads and async work very well here.

**Interview Question (Very Common)**

Why doesn’t threading speed up CPU-bound tasks in Python?

**Correct Answer:**

Because the GIL allows only one thread to execute Python bytecode at a time, CPU-bound threads compete for the GIL instead of running in parallel.

**Threading in Python**

What Threading Is Good For:

- Best for IO-bound work: network calls, disk reads, waiting on APIs.
- Helps reduce latency (total wait time), not CPU throughput.

Why it helps with IO:

- Threads can overlap waiting.
- While one thread waits for IO, another can run.
- In CPython, threads release the GIL during IO, so waiting doesn’t block others.

**Interview-Relevant Code:**

    # Import the thread pool helper
    from concurrent.futures import ThreadPoolExecutor
    # Import time utilities (for sleep)
    import time

    # Define a function that simulates IO work
    def io_task():
        # Wait for 2 seconds to mimic IO delay
        time.sleep(2)
        # Return a result after the wait
        return "done"

    # Create a pool of up to 3 worker threads
    with ThreadPoolExecutor(max_workers=3) as executor:
        # Run io_task 3 times in parallel and collect results
        results = list(executor.map(lambda _: io_task(), range(3)))

**Pseudocode:**

    define io_task:
        wait for IO (sleep or network)
        return "done"

    create thread pool with N workers
    submit the same io_task N times
    wait for all to finish
    collect results


**Explanation:**

- Each task waits for IO.
- Threads overlap those waits.
- Total time is closer to the longest single wait, not the sum.

**Multiprocessing in Python**

Why it works for CPU:

- Each process has its own Python interpreter and its own GIL.
- So CPU work runs in parallel on multiple cores.

Tradeoffs:

- More overhead than threads.
- Memory isn’t shared by default.

**Interview-Relevant Code**

    # Import the process pool helper
    from concurrent.futures import ProcessPoolExecutor

    # Define a function that does CPU work
    def cpu_task(x):
        # Compute the square of x
        return x * x

    # Create a pool of up to 4 worker processes
    with ProcessPoolExecutor(max_workers=4) as executor:
        # Run cpu_task on numbers 0..9 in parallel and collect results
        results = list(executor.map(cpu_task, range(10)))
        
**Pseudocode:**

    define cpu_task(x):
        return x * x

    create process pool with N workers
    submit cpu_task for a list of inputs
    wait for all to finish
    collect results

**Explanation:**

- Each process runs on a separate CPU core
- Memory is not shared
- Higher overhead than threads

**One-line summary:**

Threads = good for IO waits, lower latency.
Processes = good for CPU work, true parallelism.

| **Aspect**  | **Threading**  | **Multiprocessing** |
| ----------- | -------------- | ------------------- |
| Parallelism | No (CPU-bound) | Yes                 |
| Memory      | Shared         | Isolated            |
| Overhead    | Low            | High                |
| Use Case    | IO-bound tasks | CPU-bound tasks     |

**Interview takeaway:**

Use threads for IO, processes for CPU.

**Async I/O (Why FastAPI Is Fast)**

What Async Really Is:

Async is single-threaded concurrency using an event loop. Tasks voluntarily yield control when waiting. Async does NOT mean parallel execution.

**Event Loop Concept:**

- Runs tasks
- Pauses tasks on await
- Switches efficiently without threads

**Interview-Relevant Async Code**

    # Async library that provides the event loop and coroutine tools
    import asyncio
    
    # Define an async function that simulates an IO wait and returns data
    async def fetch_data():
        # Yield control to the loop for ~2s, letting other tasks run
        await asyncio.sleep(2)
        # Return a result after the wait
        return "data"
    
    # Top-level coroutine that coordinates other coroutines
    async def main():
        # Schedule three independent fetches at once, wait for all to finish
        results = await asyncio.gather(
            fetch_data(),
            fetch_data(),
            fetch_data()
        )
        # Display the list of returned values
        print(results)
    
    # Start the event loop and run main() until it completes
    asyncio.run(main())

**Pseudocode:**

    import async tools
    
    define async fetch_data:
        wait non-blocking for 2 seconds
        return "data"
    
    define async main:
        start three fetch_data tasks at the same time
        wait until all three finish
        print the list of results
    
    start event loop and run main until done

**Explanation:**

- No threads created
- No blocking
- Excellent for high-concurrency APIs

**When NOT to Use Async:**

- CPU-heavy logic
- Blocking libraries
- Legacy synchronous SDKs

**Interview one-liner:**

Async improves concurrency for IO-bound workloads, not CPU-bound workloads.

**Backend Scaling Reality (Production)**

- FastAPI uses async for IO efficiency
- Gunicorn runs multiple worker processes
- Each worker has its own GIL
- Horizontal scaling handles high load

This is how Python scales in production.

**Common Interview Traps**

Trap 1:    
“Async is faster than threads” X
Correct:    
Async is more efficient for IO, not inherently faster.

Trap 2:    
“GIL makes Python useless” X
Correct:    
The GIL affects CPU-bound threading, not IO-bound backend workloads.

**Final Interview Summary**

Python’s GIL prevents parallel execution of CPU-bound threads but enables safe memory management. Backend systems overcome this using async IO, multiprocessing, and horizontal scaling.

`**If you can explain this calmly, you pass most backend interviews.**`
