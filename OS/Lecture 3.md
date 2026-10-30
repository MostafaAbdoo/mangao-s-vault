*Don't forget problem sessions*
*Physical memory* -> Main memory, the actual RAM
*Virtual memory* -> Memory on disk. Allows for effective multiprogramming and relieves the user of tight constraints of main memory using the ability to store on memory or on disk
*Advantages*:
1. More processes may be maintained in main memory
2. .A process may be larger than all of RAM size
*Illusion* of Infinite Memory, TAX? Dealing with Disk!

*Memory Manager* Requirements: Memory needs to be allocated to ensure a reasonable *supply* of *ready processes* to consume available processor time
1. Relocation
2. Protection
3. Sharing
4. Logical organisation
5. Physical organisation
![[Pasted image 20261014112527.png|399]]
## Partitioning
### **Paging**
- subdivide RAM into equal-sized frames and subdivide the process into equal-sized pages; size of page = size of frame (by OS)
- put every page in a frame in any order in memory or in disk
**Page Table**:
- per process
- maps page number to corresponding frame number (2d array, index and value)
*OS maintains*:
1. page table for each process
	Contains the frame location for each page in process
2. Free Frame List (Shared Resource)
	A list of currently free frames in RAM
*PROBLEM*:
Internal Fragmentation -> Memory in last page of a process is fragmented (Not Used – Can NOT be Used)
EX: frame size = 4, process A = 18
### **SEGMENTATION**
RAM is not subdivided, program can be subdivided into segments
- Segments may vary in length by programmer or compiler
- Put each segment anywhere that fits, in any order, memory or disk
- There is a maximum segment length; to not consume all memory for one process
**Segment Table**:
- per process
- Saves index of segment, start address and size
**Memory addressing** consist of:
1. segment number
2. offset
*PROBLEM*:
External Fragmentation -> Memory external to all processes is fragmented (Not Used – Can be Used)
*SOLUTION*:
Compaction ->
- OS moves processes so that they are contiguous
- Time consuming and wastes CPU time
**Placement Algorithms**:
*Best-fit algorithm*
- Chooses the block that is closest in size to the request
- Worst performer overall
- Since smallest block is found for process, the smallest amount of fragmentation is left
- Memory compaction must be done more often
*First-fit algorithm*
- Scans memory form the beginning and chooses the first available block that is large enough
- Fastest
- May have many process loaded in the front end of memory that must be searched over when trying to find a free block
*Next-fit*
- Scans memory from the location of the last placement
- More often allocate a block of memory at the end of memory where the largest block is found
- The largest block of memory is broken up into smaller blocks
- Compaction is required to obtain a large block at the end of memory
![[Pasted image 20261014120501.png]]
## Fault Handler
![[Pasted image 20261018193625.png]]
- An exception is generated when an address is needed that is not in main memory, we can know using the *present* bit in page table is it's = 0
- issue an interrupt and block the process to fitch the address from the disk
- put it in any free frame in the page table and restart the same instruction
- if there is no free frame swap out a victim to replace it, the victim is put on the disk if it's only modified, if it's not it's already have a copy on the disk
	- all of that using a *modified* bit by MMU
## Locality
Program and data references within a process tend to cluster. Only a few pieces of a process will be needed over a short period of time
As a program executes it transitions through a sequence of “*working sets*” consisting of varying sized subsets of the address space
![[Pasted image 20261018200040.png]]
- Temporal Locality (Locality in Time): Keep recently accessed data items closer to processor
- Spatial Locality (Locality in Space): Move contiguous blocks to the upper levels (like recursion)
Working Set defines *minimum* number of pages needed for process to behave well
Not enough memory for Working Set ->*Thrashing*
## Thrashing
A state in which the system spends most of its time swapping pieces rather than executing instructions
*Reasons*:
1. OS failed to satisfy the current working set
2. OS replace wrong victim(s) [i.e. required in WS]
*Solution*:
- To avoid this, the OS tries to guess which pieces are least likely to be used in the near future.
- The guess is based on recent history
## Paging Address Translation
![[Pasted image 20261018203202.png|535]]
![[Pasted image 20261018203339.png|533]]
or u can just concatenate, if page size = 2^k then offset is limited to k
- *MMU* is responsible for adding and concatenation
- We need 1 bits in virtual paging and segmentation: P -> present in RAM or not, M -> modified or not 