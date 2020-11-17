## Goal 

Present the programmer with ~= as much memory as the largest memory at ~= the speed of the fastest memory.

## Approach

Memory Hierarchy

- Successively higher levers conatin "most used" data from lower levels

- Exploits ==temporal and spatial locality==

  

## Cache Management



* What is the overall organization of blocks we impose on our cache?
  * Where do we put a block of data from memory ?
  * How do we know if a block is already in cache?
  * How do we quickly find a block when we need it?
  * When do we replace something in the cache?



* Initially,
  * Garbage, "Cold"
  * Keep track with Valid bit



* number of blocks the cache can hold

  * ==**\#** of block = **Sizeof(cache)** / **Sizeof(block)**==

* ***Offset field***: lowest bits of memory address can be used to index to specific bytes within a block

  * block size needs to be a power of two (in bytes)
  * ==\# of offset bits = log_2(block size)==

  <img src="./Cache 1.assets/image-20201116160030421.png" alt="image-20201116160030421" width="70%" height="70%" />

  

* ***Tag field***: Leftover upper bits of memory address determine which portion of memory the block came from (identifier)

  <img src="./Cache 1.assets/image-20201116160114214.png" alt="image-20201116160114214" width="70%" height="70%" />

<img src="./Cache 1.assets/image-20201116165921257.png" alt="image-20201116165921257" width="70%" height="70%" />

* Meaning of the field sizes:

  * **Offset** bits <-> **2^offset^** bytes per block = 2^offset-2^ bytes per block

  * **Tab** bits = A -**Offset**, where A = *# of address bits* (A=32 here)

    

## Fully Associative Cache

* 在cache里究竟有什么
  * actual data block **8 x K = 8 x 2^Offset^** bits
  * **Tag** field of address as identifier
  * **Valid** bit (1 bit): Whether cache slot was filled in 
  * 必要的替换管理位(LRU bits)

## Read and Write

### Handling Cache Hits

* Read Hits 
* Write Hits
  1. Write-Through Policy: Always write data to cache and to memory (through cache)
     * Forces cache and memory to always be consistent
     * Slow! (every memory access is long)
     * Include a Write Buffer that updates memory in parallel with processor
  2. **Write-back Policy**: Write data only to cache, then update memory when block is removed
     * Allows cache and memory to be inconsistent
     * Multiple writes collected in cache, single write to memory per block
     * **Dirty bit**: Extra bit per cache row that is set if block was written to (is "dirty") and needs to be written back.

### Handling Cache Misses



