### Brief Description of This Work
1. Provide a comprehensive classification of common errors in OpenMP applications that consists of three categories
    * Syntactic Defects - The code is not compliant to the grammar of OpenMP.
    * Semantic Defects - The code is consistent with the OpenMP grammars, but causes failures at runtime.
    * Performance Issues - The code is consistent with the OpenMP specification, but it is inefficient due to the improper use of constructs
2. Discuss possible mechanisms to detect and fix these errors (compiler, static analysis, runtime, debugger, correctness tool)
3. Take new constructs in OpenMP 3.1 (tasking) and OpenMP 4.0(offloading) into consideration. Introduce potential errors due to misuse of these new constructs.

### Discrimination of Ambiguous Notions
1. Defect: programming errors (e.g., incorrect source code)
2. Failure: error manifestation (e.g., execution abortions or deadlocks)

### Classification of Errors in OpenMP Applications
1. Syntactic Defects
    * mistyped constructs
    * mistyped clauses
2. Semantics Defects
    * violation of OpenMP Standard
        * uninitialized locks
        * barriers not reached by all threads in the team (barrier divergence)
        * violation of single entry single exit (SESE) principle
        * *worksharing* constructs not reached by all threads in the team (due to if conditions or goto statements)
        * invalid nesting of regions (e.g., nested parallel for loop)
        * unlocking locks not owned by current thread / task
        * <span style="color:red">*SIMD* constructs with unaligned data</span>
    * conceptual defects
        * mistyped *parallel* instead of *parallel for* constructs
        * <span style="color:red">single producer without worksharing</span>
        * using locks to mimic barriers
        * incorrect assumptions about the number of threads (the actual number of threads may not be equivalent to the requested number)
        * unallocated memory on the host / device
        * missing data mapping when offloading calculations to the device
    * conceptual defects related to concurrency errors
        * data races on the host side
        * data races on the device side
        * data races between the host and the device
        * deadlocks with multiple locks
        * deadlocks with locks and *taskwait* constructs
3. Performance Issues
    * heavily using synchronization constructs
    * improper memory access patterns

### Drawbacks
1. Don't consider the possible implementations of OpenMP constructs.
2. Don't introduce race conditions and performance issues in detail.
