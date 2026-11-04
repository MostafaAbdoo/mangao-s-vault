![[Pasted image 20261104113453.png]]
**OS Main Goal**:
- Minimize number of disk access -> No definitive best policy
## Fetch Policy
**1. Demand Paging**:
- Only brings pages into main memory when a reference is made to a location on the page
- Many page faults when process first started
**2. Prepaging**:
- Brings in more pages than needed, either at:
	1. Process Start
	2. Page Fault
(*+*) More efficient to bring in pages that reside contiguously on disk
(*-*) Ineffective if most of the brought pages are not referenced
## Placement Policy
Determines where in real memory a process piece is to reside, Important in a *segmentation* only
## Replacement Policy
determines which page currently in memory is to be replaced
Answer 3 questions:
1. When to replace?
	- RAM is Full
	- Process reaches its Max
2. Which page to be replaced?
	- Page least likely to be referenced
	- Predict future from the past (Based on: Principle of Locality)
	- 5 different algorithms
 3. From whom?
	- Local from process
	- Global from any other process

**Frame Locking**
Associate a lock bit with each frame, Locked frame not replaced
*Examples*:
- Kernel of the operating system
- Key control structures
- I/O buffers
- Stack page
what is the best case scenario? *resident set* >= *working set*

### **Optimal Algorithm:**
Selects page for which time to the next reference is the longest
- *Adv*: BEST method (benchmark for others)
- *DisAdv*: Not feasible – need perfect knowledge of future
![[Pasted image 20261104133725.png]]
Least Number of Faults of all algorithms-> 6 (2 * Resident Set?)
### **Least Recently Used (LRU):**
Replaces the page that has not been referenced for longest time, By the principle of locality, should be least likely to be referenced in the near future.
- *Adv*: BEST feasible
- *DisAdv*: Difficult to implement
	- Tag each page with the time of last reference.
	- requires a great deal of overhead.
In Practice: Approximate LRU
![[Pasted image 20261104134736.png]]
### **FIFO:**
Treats allocated frames as a circular buffer, Pages are removed in round-robin style
- *Adv*: Simplest to implement
- *DisAdv*: first-loaded pages may be needed again very soon
![[Pasted image 20261104135124.png]]
### **Clock:**
Compromise between: *Simplicity* of FIFO and *Not used pages* from LRU.
Chose an old page rather that the oldest page 
Uses an additional bit called a “*use bit*”, Set to 1 when the page is used (accessed). (By MMU)
OS scans the set *flipping* all 1’s to 0, The *first* frame with use bit = 0 is replaced.
![[Pasted image 20261104141459.png]]
What if hand moving slowly? Good sign or bad sign?
- Not many page faults and/or find page quickly
What if hand is moving quickly?
- Lots of page faults and/or lots of reference bits set
One way to view clock algorithm:
- Crude partitioning of pages into two groups: young and old
- Why not partition into more than 2 groups?

**Nth chance algorithm**: Give page N chances
- OS keeps counter per page: # sweeps
- On page fault, OS checks use bit:
	- 0-> *increment* counter; if count=N, replace page
	* 1-> *clear* use and also clear counter (used in last sweep)
Means that clock hand has to sweep by N times without page being used before page is replaced
*How do we pick N?*
Why pick large N? Better approx to LRU
- If N ~ 1K, really good approximation
Why pick small N? More efficient

Otherwise might have to look a long way to find free page
![[Pasted image 20261104142405.png]]
*Nth chance algorithm* would be between *clock* and *LRU* depends on size of N

In *all prev. algorithms*: They DON’T take care of modified pages
	•Replacing modified page -> 2 I/O (write, read)
	•Replacing clean page -> 1 I/O (read)
*Solution*
- Opt1: prefer clean over modified (*modified clock*) (Read-only, works only for clock)
- Opt2: keep replaced page in RAM as long as possible (*page buffer*)
```
The “Present” bit (called “Valid” elsewhere):
	P==0: Page is invalid and a reference will cause page fault
	P==1: Page frame number is valid and MMU is allowed to proceed with translation

The “Writable” bit (could have opposite sense and be called “Read-only”):
	W==0: Page is read-only and cannot be written. 
	W==1: Page can be written

The “Accessed” bit (called “Use” elsewhere):
	A==0: Page has not been accessed (or used) since last time software set A®0
	A==1: Page has been accessed (or used) since last time software set A®0

The “Dirty” bit (called “Modified” elsewhere):
	»D==0: Page has not been modified (written) since PTE was loaded
	»D==1: Page has changed since PTE was loaded
	
The 3 “FREE” bits: Available for OS developers (not used by MMU)
```
### **Page Buffering**
Keep replaced “victim” page in RAM
Add it to the *TAIL* of one of 2 lists
1. *Free page list*: if page has not been modified
2. *Modified page list*: if page has been modified
Modified list written out on disk in batches when
-  Its size reaches MAX
- Need to free some RAM
then after writing append the list to the TAIL of free page list (so that u keep it in memory as long as possible and not to go to disk)
*When bringing a PAGE:*
- HOW to know whether it’s Buffered(at the end of any of the lists) or not? add a bit to page table (u have 3 free bits)
- If Buffered, HOW to know its frame #? By keeping its value in the Page Table even after removing it from Resident Set (i.e. even after setting Present bit to 0)
*When allocating a FRAME:* 
- HOW to know whether it’s Buffered or not? Env & P#? Add “Buffered” bit, corresponding “Page #” & “environment ID” in *frame information*
- If Buffered, WHAT shall OS do? “Buffered” bit of the corresponding “page” in the page table of the associated “environment”

*Advantages*:
1. Reduce # reads & # writes from/to H.D.D
2. Modified pages are written out in clusters (faster than writing one at a time)
## Cleaning Policy
Determine when a modified page should be written out to secondary memory.
1. *Demand cleaning*
	A page is written out only when it has been selected for replacement
2. *Precleaning*
	Pages are written out in batches
3. BEST approach uses *page buffering*
	Pages in Modified list are periodically written out in batches
## Load Control
Determines # processes in main memory
	-> The multiprogramming level

1. Too few processes:
	Bad CPU utilization (idle if all processes are blocked)
2. Too many processes:
	lead to thrashing
![[Pasted image 20261104183956.png]]
**Process Suspension**
If the degree of multiprogramming is to be reduced, process(es) should be suspended
*Six possibilities exist…*:

- Lowest priority process
- Faulting process
	does not have its working set in RAM è will be blocked anyway
- Last process activated
	least likely to have its working set resident
- Process with smallest resident set
	requires the least future effort to reload
- Largest process
	obtains the most free frames
- Process with the largest remaining execution window