NBHashTree
==========

NBHashTree is non-balanced B-Tree stored on disk.

In order to make the tree balanced, instead of keys, we add MD5().

Real live examples show that in this way we can have near-balanced tree.

Of cource in this way we will loose the ability to have ordered information and the result will look more like hash-table.

However, this tree have on very strong advantage against hash-table. This is avoidance of collisions.

Second anvantage is there is no need to resize the hashtable.

In order to make the NBTree even more faster, we will make one IMPORTANT ASSUMPTION - e.g. there is no two keys with same MD5().

For real live data, this assumption is OK.

Assuming that, helps us NOT TO STORE THE KEY into the tree. All we store is 16 bytes for the MD5(key) plus 8 bytes for the tree "pointer".

In order to make the NBTree even more faster, we will do "lazy" deletes - e.g. will mark deleted nodes as deleted. This can be done with bitmask, but we will use one more byte for it.

This makes one node to be:

N * (16 + 8 + 1) bytes. This makes around 150 "small" nodes in single BTree node of 4096 bytes - e.g. typical disk cluster/sector.

This also means we can seek 22,500 values with 2 disk seeks, and with 3 seeks - 3,375,000 values.

If we future cache the root node in memory (will took just 4KB), we can eliminate one more disk seek.

Of cource if we want the value itself, it will take one more seek if value is short or several seeks if value is bigger.
