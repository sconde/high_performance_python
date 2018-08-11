# Understanding Performant Python

- Programming: moving bits of data and transforming them in special ways in order to achieve a particular result
- High performance programming:
    - act of minimizing these operations by:
        1. reducing the overheads: writing more efficient code
            - helpful to have insight in the actual hardware on which we are movign these bits
            - *not necessary* since python does a great job to abstract away direct interactions with the hardware
            - understanding the best way that bits can be moved in the real hardware and ways that python's abstraction for moving bits -> writing high performance programs in python
        2. finding more suitable algorithm: chaning the way we do these operations in order to make each one more meaningful

## Fundamental Computer System

- Underlying computer components: computing units, memory units, and the connections between them
    - CPU: how many computations it can do per second (FLOPS)
        - L1/L2/L3 cache are connected to the CPU through the *backside bus*
    - memory: how much data it can hold and how fast we can read/write to it (RAM/HD)
    - connections: how fast they can move data from one place to another (bus)

### Computing Units (CPU)

- Centerpiece of its usefulness, provides the ability to transform bits or change the state of the current process
- most commonly used computing unit; GPU are becoming more applicable for numerical applications
- basic arithmetic operations: integers, real number and bitwise operations
- specilized operations: fused multiply add (FMA: `A*B + C`)
- main properties: IPC and clock speed
    - IPC: instruction per cype, number of operation it can do in one cycle -> change level of `vectorization` that is possible
    - Clock speed: how many cycles it can do in one cycle -> able to do more calculations per second
        - GPU: high IPC and clock speed -> high throughput
        - `vectorization`: SIMD -> when CPU is provided with multiple pieces of data a time and is able to operate on all of them at one
    - IPC and clock speed have been stagnant due to transistors size
- To overcome limitations of IPC and clock speed:
    - hyperthreading: when successful, gains of up to 30% over a single thread
        - present virtual second CPU to the host OS
        - clever hardware logic tries to interleave two threads of instructions into the execution units on a single CPU
        - works well when the unit of work across both threas uses different type of execution unit
            - *example:* one performs floating-point operations and the other performs integer operation
    - clever out-of-order execution: allowing greater overall utilization of the available resources
        - enables a compiler to spot that some parts of a linear program sequence do not depend on the results of a previous piece of work:
            both piece of work could potentially occur in any order or at the same time
        - pieces of work are computed out of their programmed order
        - enables some instructions to execute when others might be blocked (i.e. waiting for a memory access)
    - multicore architectures: multiple CPU within the same unit
        - increases the total capability without running into barries in making each individual unit faster
        - two cores: has two physical computing units that are connected to each other
        - increases the total number of operations that can be done per second
        - introduces intricacies in fully utilizing both computing units are the same time
        - Simply adding more cores to CPU does not alays speed up program -> **Amdahl's law**
			-  if a program designed to run on multiple cores has some routines that must run on one core, this will be the bottleneck for the final speedup that can be achieved by allocating more cores.
- CPUs, we can add more cores that can perform various chunks of the computation as necessary until we reach a point where the bottleneck is a specific core finishing its task
- The bottleneck in any parallel calculation is always the smaller serial task that are being spread out
- major hurdle with utilizing multiple cores in Python is Python's use of a global interpreter lock (GIL)
    - makes sure that a python process can only run one instruction at a time, regardless of the number of cores its using
    - even though code have access to more cores, only one core is running a python instruction at any given time
    - this problem can be avoided by using other standard library tools (`multiprocessing`, etc), technologies (`numexpr`, `Cython`, etc)

### Memory Units


### Communications Layers
