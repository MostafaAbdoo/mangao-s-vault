The scheduling function should: 
1. Share time fairly among processes
2. Prevent starvation of a process
3. Use the processor efficiently
4. Have low overhead (min TAX$)
5. Prioritise processes when necessary
**Types of Scheduling**:
Long-term scheduling
	whether to add a new process to the set of processes that are currently *active*.
Medium-term scheduling
	whether to add a process to those that are at partially (or fully) in *main memory*.
Short-term scheduling
	actual decision of which ready process to *execute next* on CPU
I/O scheduling
	Which I/O pending processes shall be handled by the available I/O device.
![[Pasted image 20261104191505.png]]
and they are *ALL* queues
##### **Long-Term:**
Determines which programs are admitted to the system for processing, according to:
- first-come-first-served
- priority
- I/O requirements
- expected execution time
Executes infrequently
*Controls* the degree of multiprogramming (Load Control)
- More processes -> smaller % of time each process is executed (Thrashing)
- Few processes -> CPU remains Idle
##### **Medium-Term:**
Part of the swapping function, *decision* based on the need to manage the *degree of multiprog*.
Executes *more* frequently.

##### **Short-Term:**
Known as *dispatcher*, Executes *most* frequently
Invoked when an event occurs:
- Clock interrupts
- I/O interrupts
- System calls
- Signals
## Performance Criteria
**User-oriented**
behaviour of system as perceived by user/process
	Ex: Response Time: elapsed time between the submission of a request until the response begins to appear as output (Turnaround time is from start to finish)
**System-oriented**
focus on effective and efficient utilization of CPU
	Ex: Throughput: number of processes completed per unit of time
**Performance-related**
Quantitative, Easily measured
	Ex: response time and throughput
**Non-performance related**
Qualitative, Hard to measure
	Ex: predictability
![[Pasted image 20261104194936.png]]
**Types of Processes**
- Processor-Bound: It mainly performs computational work & *occasionally* uses I/O devices.
- I/O-Bound: depends primarily on the time spent waiting for I/O operations.
## Short-term Policies
**Non-preemptive**
process in run state will continue until it terminates OR blocks itself for I/O

**Preemptive**
running process may be interrupted and moved to ready state by the OS
Preemption may occur when:
- new process arrives,
- on an interrupt
- periodically (clock interrupt).
### **FCFS**
Each process joins the Ready queue, Select process with max wait-time, *Non-preemptive*.
![[Pasted image 20261104200604.png]]
*Pros*:
- Simplest – min overhead (low TAX$)
- No starvation
*Cons*:
- Bad response time for short process -> (may have to wait a very long time before it can execute)
- Favors CPU-bound processes -> inefficient use of I/O (I/O-bound has to wait until all CPU-bound processes complete)
### **Shortest Remaining Time (SRT)**
Preemptive version of SPN, Preemption on process arrival
Select process with min (service – execution)
![[Pasted image 20261104211649.png]]
*Pros*:
- High throughput
- Good response time for short processes
- Efficient use of I/O devices (prefer I/O-bounds over CPU-bounds)

*Cons*:
- Starvation for long processes
- Overhead (to estimate service time) (high TAX$)
- Bad predictability of longer processes
### **Round Robin (RR)**
- Preemptive version of FCFS
- Clock interrupt is generated at periodic intervals
- each process is given a slice of time (quantum) before being preempted.
- No need-to-know remaining time of the process
![[Pasted image 20261104212644.png]]
if quantum is smaller than the smallest process -> worst mean nTaT
*Pros*:
- Independent on service time
- Good response time for short processes
*Cons*:
- Favors CPU-bound processes è inefficient use of I/O devices (I/O bounds need to wait all CPU-bounds in the queue)
- Context-switch overhead   (TAX$)
- Affect the cache usage  (TAX$)

Time Quantum?
- very long -> response time suffers!
	Small later job waits for long time
- very short -> throughput suffers!
	Overhead- many context switch, cache misses
- slightly > a typical interaction time
In practice, need to balance short-job performance & long-job throughput
**Solution to I/O Bound Problem**
*Virtual Round Robin*
- Produce fair treatment for I/O-bound
- Use auxiliary queue with high priority over ready Q
- Process from auxiliary Q allowed for a single quantum only
### **Multilevel Feedback Queue (MLFQ)**
- Preemptive on a clock
- Quantum per Queue
- Different priority/queue
- last queue with less priority.
Last queue can be:
- RR
- FCFS
most Approx. to SRT
(*-*) Penalize long jobs
(*+*) No need to know remain. time
note: u can leave the process at the same queue IF there's no any other process at the queue
note: when picking a process to run always pick from higher queues, that's what priority mean u fucking idiot
*Quantums*?!
- OPT1: same quantum for all queues
	*Cons*: turnaround of long processes is very high
- OPT2: increasing quantum per queue
	*Cons*: long processes still suffer starvation
	*Solution*: promote process to higher-priority queue after certain wait time
note: like RR, always good to pick quantum >= to the smallest service time
![[Pasted image 20261104221611.png]]
*Pros*:
- Independent on service time
- Efficient use of I/O devices (prefer I/O bounds over CPU-bounds)
*Cons*:
- Possible starvation for long processes… HOW to avoid?
- Overhead (different quantums, many clock interrupts, change priority after certain wait…) (high TAX$)