- **Illusionist** ->Provide clean, easy-to-use abstractions of physical resources
**Process**: Execution environment with restricted rights provided by OS, it's the thing that u can deal with using system libraries, every program has a process
it contains Threads(Processor), Address space(Memory), Files(Storage) and Sockets(Network)
- **Governor** -> Manage protection, isolation, and sharing of resources(CPU, RAM, I/O Devices)
**OS** isolates processes from each other, **OS** isolates itself from other processes
**Context Switch**: Save CPU’s Regs of P1, Load CPU’s Regs of P2
TAX1: Save/Load Overhead, TAX2: Lose Cache

---
**Uniprogramming**: Single App. System, Processor must wait for I/O instruction to complete before proceeding
**Multiprogramming**: When one job needs to wait for I/O, the processor can switch to the other job
only one process can execute at a time; meanwhile all other processes are waiting for the processor.
**Multiprocessing**: more than one process can be running simultaneously, each on a different processor.

---
Stack -> local data and function calls, Head -> Dynamic variables
after u compile the code the EXE file has data (global and static variables) and instructions (code)

**Thread**: Single unique execution context (CPU Snapshot), virtual core, each virtual core (thread) consists of: *CPU Registers* (IP, SP, Flags, Int & Float Regs) and *STACK*
anything else is shared across threads
*Multiple threads*: Multiplex hardware in time
Two states for a thread:
- If Run: on the real (physical) core 
- Else: Saved in chunk of memory – called Thread Control Block (TCB)
What happened to switch?
1. Save Program counter (PC), stack pointer (SP), … in vCPU1’s TCB (memory)
2. Load Program counter (PC), stack pointer (SP), … from vCPU2's TCB to phys. CPU, jumped to PC

What triggered this switch?
**Timer, voluntary yield, I/O**, other things we will discuss
•*Illusion:* Infinite number of processors
•*Reality*: Threads execute on limited number of physical CPUs with variable “speed”

**Address Space**: •the set of accessible addresses + state associated with them (illusion):
For 32-bit processor: 2^32
For 64-bit processor: 2^64
ALL achieved by **Virtual Memory** concept: allows programs to address memory from a logical point of view Without regard to the limits of RAM
- Dynamic Allocation & Management    
- Isolation
- Modular Programming
- Sharing
- Protection (RAM is accessed by OS...)
CPU will provide u a virtual address, Memory Management Unit (MMU) and OS are responsible to translate that VA to Physical Address
PA = f(VA, PARAM) -> OS set params and MMU apply the function
VA = f^-1 (PA, PARAM) -> only OS
- *Illusion*: Max address space for each process (e.g. 4 GB / process)
- *Reality*: All processed are sharing the physical memory (RAM)
  - Only active part of a process is loaded at a given period

**Process**: •execution environment with Restricted Rights
- One or more threads executing in a (protected) Address Space
- Owns memory (address space), file descriptors, network connections, …
main reason is protection, EXE to Process is 1 to M same as Process to Threads
**between threads**:
Switch overhead
- Same process:  low
- Different proc.: high
Protection
- Same proc: low
- Different proc: high
Sharing overhead
- Same proc: low
- Different proc: high
Parallelism: No
but if it's a multi-core processor:
Sharing overhead
- Same proc: low
- Different proc simultaneous core: medium
- Different proc offloaded core: high
Parallelism: Yes

**Dual Mode Operations**:
Processes (i.e., programs you run) execute in USER mode
- Less privilege
- To perform privileged actions, processes request services from the OS kernel via *System Calls*

Kernel executes in KERNEL mode
- Performs privileged actions to support running processes
- … and configures hardware to properly protect them (e.g., address translation)

Carefully controlled transitions between user mode and kernel mode via:
1. System calls
2. interrupts
3. exceptions
![[Pasted image 20261005151121.png]]
