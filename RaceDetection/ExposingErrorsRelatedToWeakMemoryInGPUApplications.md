### Brief Description of This Work
1. Propose a memory stressing based testing strategy to trigger weak behaviors frequently
2. Carry out a large number of pilot experiments to find out proper parameters for the memory stressing (patch size, stressing thread access sequences, the number of locations to stress, thread randomisation)
3. Propose a fence insertion algorithm, utilizing the testing strategy to verify the correctness of the applications after fence insertion

### Related Background
1. GPU Architecture
    * CTA
    * weak memory model
        * MP litmus test
        * LB litmus test
        * SB litmus test
2. Memory Model and Consistency
    * sequential consistency
        * Operations from the same process executes in the program order
        * Operations of all processes interleaves during the program execution
        * Writes to variables by different processors have to be observed in the same order by all processors
    * relaxed memory consistency
        * TSO and PC only allows a read operation to be reordered before preceding write operations
        * PSO allows a read operation to be reordered before preceding write operations and a write operation to be reordered before preceding write operations
        * WO (weak ordering) allows all four types of reordering
3. Memory Fence in CUDA(PTX)
    * In order to avoid operation reordering, programmers can use memory fences in CUDA. Memory fences can have different scope of effects

### Memory Stressing based Testing Environment
1. Allocate a scratchpad memory region and stressing threads that are independent of the memory and threads used by the original application
2. Carry out multiple pilot experiments using MP, LB, SB to figure out proper configuration of the memory stressing
3. It uses thread randomisation to generate stressing thread ID (not sure why this is useful)
4. In the evaluation, this memory stressing based testing strategy outperformed other strategies. (Repeatedly execute each application for an hour. The memory stressing based testing strategy can observe errors in more than 5% of all executions for most of the applications) 

### Empirical Fence Insertion
1. In the beginning, adding fence after all operations to guarantee correctness. Repeatedly remove fences and execute the modified application with memory stressing testing to find out the smallest fence set that doesn't trigger weak behaviours
2. First try to use binary reduction to remove fences efficiently, then apply linear reduction to calculate the final fence set
3. The generated fence set incurs low time and energy overhead (<3%)

### Drawbacks
1. The pilot experiment need to be carried out for each device architecture to find out the proper configuration of the memory stressing testing
2. The fence insertion doesn't guarantee correctness and determinism, since the memory stressing based testing strategy is a systematic method to find out weak behaviours
3. The fence insertion is not efficient for large applications
