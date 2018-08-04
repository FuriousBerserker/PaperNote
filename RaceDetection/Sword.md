### Algorithm
1. A offline data race detector for OpenMP programs
2. Each thread maintain a fixed-sized buffer (2 MB for fitting into L3 cache ). During the program execution, Sword only collects OpenMP events and memory accesses into the buffer. When the buffer is full, the data is compressed and write out to the disk. After the program execution, an offline analysis phase is executed to find out data races in the collected program trace.
3. The offline analysis utilizes Offset-Span labeling and barrier interval (code region between two adjacent barriers) to find out parallel region. After locating all parallel regions, it builds a interval tree (augmented red-black tree) for each thread (thread or parallel region) to summarize memory accesses. Data race detection is implemented by comparing the interval tree of a single thread with those of other threads to see whether these exists overlapping between tree nodes.
### Contribution
1. A bounded race detector for OpenMP programs.
2. Utilize Offset-Span labeling and lock set to find out potential data races in all possible thread interleavings of a certain input
3. Only collect data during program execution, reducing the interference to the program execution
### Drawbacks
1. The offline analysis is sequential
2. The offline analysis cannot tackle OpenMP tasking by Offset-Span labeling
3. The log file is huge, which can hurdle the offline memory analysis (Sword uses a stream algorithm to mitigate this drawback)
4. The slowdown is still high.
