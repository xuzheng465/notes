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

  <img src="/Users/xuzheng/Projects/notes/CSAPP/Caches/Cache 1.assets/image-20201116160030421.png" alt="image-20201116160030421" style="zoom:33%;" />

  

* ***Tag field***: Leftover upper bits of memory address determine which portion of memory the block came from (identifier)

  <img src="/Users/xuzheng/Projects/notes/CSAPP/Caches/Cache 1.assets/image-20201116160114214.png" alt="image-20201116160114214" style="zoom:33%;" />

<img src="/Users/xuzheng/Projects/notes/CSAPP/Caches/Cache 1.assets/image-20201116165921257.png" alt="image-20201116165921257" style="zoom:33%;" />

* Meaning of the field sizes:
  * **Offset** bits <-> **2^offset^** bytes per block = 2^offset-2^ bytes per block
  * **Tab** bits = A -**Offset**, where A = *# of address bits* (A=32 here)
  * 



## Fully Associative Cache