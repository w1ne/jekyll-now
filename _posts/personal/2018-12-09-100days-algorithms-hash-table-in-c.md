---
layout: post
title: Algorithms series: Hash table implementation in c(draft)
blogid: personal
sticky: false
published: false
tags: [100 days of code, algorithms, training, hash table, c]
---
Implementation of the hash table data structure in c. Object oriented way.

# Naїve approach example
Let's consider simple hash table with 8bit hashing function.
Range of possible hash function values effectively limits size of the hash table to 256 objects.

Interfaces:

```c
8bitHashTable_getData(8bitHash_TableObject_t* HashTable, 8bitHash_KeyObject_t* Key)
8bitHashTable_setData(8bitHash_TableObject_t* HashTable, 8bitHash_DataObject_t* Data)
8bitHashTbale_delData(8bitHash_TableObject_t* HashTable, 8bitHash_KeyObject_t* Key)
8bitHashTbale_initTable(8bitHash_TableObject_t* HashTable)
```
