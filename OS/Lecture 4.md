## Issue#1: Sharing
*How to share?* -> the shared page appears in both page tables
*Where to use it?*
- The kernel region of every process has the same page table entries
- Different processes using the same binary (Program)
- User-level system libraries
- Shared-memory segments between processes
	- can share objects directly between processes
	- must map page into the same place in page table (address space)
## Issue#1: Page Size
*Smaller page size*:
- *(+)*: less amount of internal fragmentation
- *(+)*: large amount of pages will be found in RAM (i.e. large WS achieving locality)-> Page faults is low
- *(-)*: more pages ->larger page tables (memory overhead)
*Larger page size*:
- *(+)*: less pages required per process > smaller page tables
- *(+)*: large portion of page tables in virtual memory
	Secondary memory is designed to efficiently transfer large blocks of data so a large page size is better
- *(-)*: large amount of internal fragmentation
- *(-)*: small WS > causes pages to contain locations further from any recent reference(locality in space) > Page faults rise.
the larger WS is the less page fault until the WS contains all pages of the process
![[Pasted image 20261024131237.png]]
*p is size of entire process, n is total number of pages in process*

## PR1: Sparse VM + Large Page Table
if the process is too small the pages will be so many and the page table will be large
ex) VM = 2 GB, Page size = 1 KB -> 2 million page
this may cause that the page table may be larger than the process itself and most of the page table is NULLs 
*Solution*: Multi-Level Paging
![[Pasted image 20261024135931.png]]
cut the table into pieces and put the pieces that u want in the memory and leave what u don't
that will require a new page table to know which piece is which, that new page table may be to large too and u may need an extra one and so on
- Single table should fit in 1 frame > Min # Levels subject to Max # entries per table (to fit in 1 frame)
- Fragmentation may happen in the last table only (doesn't fit in a frame)

*Ex1*: Given:
RAM size = 4 GB, Virtual size / process = 4 GB, Page size = 4 KB .
Required:
Min # Levels?
![[Pasted image 20261024142242.png]]
*Answers*:
- How many entries in page table? = Virtual Size / Page Size = 4 GB / 4 KB = 1 M
- What’s the size of each entry (in bytes)?
	Since # frames = 4 GB / 4 KB = 1 M = 220
	Need 20 bit for Frame# + 2 bits (Present + Modified)
	22 bits can be stored in integer data type (32 bit)
- What’s the total size of the page table? =1M * 4B= 4MB
- How we store it? In separate frames of RAM
![[Pasted image 20261026182047.png]]
![[Pasted image 20261026182102.png]]
## Pr2: # ref’s to RAM per program ref!!
In simple paging: each virtual memory reference can cause two physical memory accesses
- One to fetch the page table
- One to fetch the data
In 2-level paging, HOW many ref’s to RAM per prog. ref? -> 3
in 6-level paging? -> 7
-> Too slow, Too many almost-empty tables. Use inverted page table instead
*Solution*: CACHE Page Tables
- Use a high-speed cache for page table entries
- Called a Translation Lookaside Buffer (TLB) (managed by MMU)
- Contains page table entries that have been most recently used
*it helps with locality*
![[Pasted image 20261026184227.png]]
- first check TLB, then check page table (with it's levels) then check the disk
- u update the TLB (MMU) and Page Table (OS) based on where u found it
*How to access it?*
- As the TLB only contains some of the page table entries we cannot simply index into the TLB based on the page number
- we will use Associative Mapping to scan every entry at the same time in O(1) unlike page table that uses Direct Mapping which is also O(1)
- anther difference between Direct Mapping and Associative Mapping is that the second one implemented using Hardware components (circuits) as we said it's managed by MMU but Direct Mapping is done using code 
![[Pasted image 20261026185258.png]]
- EAT = Hit Rate x Hit Time + Miss Rate x Miss Time
- EAT = Hit Time + Miss Rate x Miss Penalty
we will use cache memory to reduce the time for fetching the content

---
*What happens to TLB on a Context Switch?*
Address Space just changed, so TLB entries no longer valid!
- *Invalidate TLB*(Delete it all): simple but might be expensive
- Include *ProcessID* in TLB: This is an architectural solution: needs hardware

*What if TLB change?*
it's the same process but a page got deleted or got saved on disk
- Must invalidate TLB entry! Otherwise, might think that page is still in memory!
- Called “TLB Consistency”

*Example*: 
Given:
Page size = 64 bytes, “Process A” reference virtual addresses:
5, 64, 128, 512, 20, 30, 520,256, 30, 520, 100
TLB size = 3 entries
*p# = VA/PAGE_SIZE*
-  What’s the number of TLB hits & misses?
- What’s the final content of the TLB?
![[Pasted image 20261026190640.png]]
