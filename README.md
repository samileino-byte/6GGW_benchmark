Based on DRAMM, CPU, GNU performance upgrade for 6G network in 5G.


Benchmark stability improvements done:

1. Timeout protection - each benchmark type has a timeout (CPU/Memory 20s, GPU/DRAM 30s, Continuous 60s). If exceeded, shows [TIMEOUT] instead of hanging.

2. Memory safety - automatically detects available RAM and adjusts buffer sizes:
- Low RAM (under 256MB free): uses 2MB buffers, 8MB DRAM probe
- Medium RAM (under 512MB free): uses 4MB buffers, 16MB DRAM probe
- Normal: uses default 10MB/32MB

3. OOM protection - catches OutOfMemoryError, shows [OOM] with available RAM, triggers garbage collection

4. Error recovery - all benchmarks wrapped in try/catch. If a benchmark crashes (native code, GPU driver issue), shows error message and recovers to ready state instead of freezing the app.

5. Frame timing error handling - catches exceptions from Choreographer/GPU setup

6. Version bumped to 1.1.0, shows available RAM at startup
