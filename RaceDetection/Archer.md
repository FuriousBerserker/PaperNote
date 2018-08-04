# Algorithm
1. A two-phases hybrid data race detector consisting of a static parallel region analyzer and a dynamic happens-before race checker
2. The static phase uses LLVM IR code and the call graph to find out race-free code region (sequential region and data independent parallel region) Polly is used to carry out data dependence analysis on LLVM IR code
3. The dynamic phase is built upon TSAN. It use TSAN's Annotation API to point out synchronization points, namely happens-before edges, in the OpenMP runtime. TSAN is responsible for instrumenting the parallel code region and detect data races by vector clock.
# Contribution
1. A data race detector than can tackle OpenMP constructs
2. Utilize static analysis to avoid unnecessary instrumentations and happens-before related operations

# Drawbacks
1. TSAN allocates vector clock for each OS threads, which can only detect data races in current thread interleaving
2. TSAN allocates fixed numbers of shadow memory cells for every 8-byte adjacent memory location, and randomly swipes out a cell for the latest memory access when all cells are occupied. Although this strategy is lock-free, it may miss potential data races
