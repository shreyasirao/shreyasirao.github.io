---
title: Database Design
date: 2021-05-26 17:15:51
---

#### **Latency** is when you ask for something how long does it take to fetch the data.

- Caches take nanoseconds.
- Disks take milliseconds to move the head and it is called as `seek time`.

#### **Bandwidth** is data rate 
- It can be increased by adding fat pipes.
- When fetching one word of data get a whole block.

> On hard drives sequential data access is easier than random access which can be slow.

### Making reads faster
We use indexes - keep metadata. But use indexes only if you need them or writes will be slow.

1. #### Indexes with hashMap
Key is page number and value is memory address on disk.
HashMap should fit in your memory because HashMap is slow on disk
if you need range query the data will be scattered on disk.


2. #### SSTables and LSM Trees
Writes are really fast. Writes to in-memory buffer are fast.
Good for range queries. Since data is stored in SSTables in sorted order

Write first to an in memory buffer called **memtable** implemented as a balanced tree- AVL tree or red black tree.

When tree becomes too large, by virtue of tree traversal it is already sorted, you take all of the data and write to SSTable file. This SSTable file is held on disk.
We compact the SSTables since repeated entries. For duplicate values newer file given precedence over older value.

When you read you first search in LSM Tree then all the SSTables from newest to oldest and if there is no key you have wasted time.
We can use sparsed indexing and binary search on SSTable since it is sorted
also bloom filters are used


3. #### BTrees
Good for reading slow for writing compared to SSTables

It models data as tree on disk. Split the page and update the parent in case there is no space to add keys to page.

### What are transactions and transaction logs?
[youtube redirect](https://youtu.be/HQ2mcEssJ7Y)

Transaction- Multiple action that must all finish successfully or 
if any one fails all fail.

Transaction logs become a recorder of all the data that has been changed or added since our last backup
after backup we clean the transaction log to control the size of it.

Point in time recovery- update till 2.10pm to avoid the attack that happened at 2.15pm

Pair up begin (start) and end of a transaction (commit or rollback) to make sure transaction are completed before putting in a db.

Append only log to make updates faster on disk to take advantage of sequential logs.
HashMaps cannot scale as we'd have to store on disk and random access will be slow.

25% of DB size is transaction log by default.

##### ACID transaction
- Atomicity - Either all committed or all aborted.
- Consistency - If transaction works it will stay that way.
- Isolation - Concurrent transactions are isolated from one another.
- Durability - Database is persistent.