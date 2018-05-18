---
title: ArrayMap源码阅读
date: 2018-05-18 10:11:04
tags: [android]
categories: 源码阅读
---



ArrayMap 可以用来代替HashMap，它是一个<key,value>映射的数据结构，设计上更多的是考虑了内存的优化。

### ArrayMap基础知识

> ArrayMap 位于 [android.util.ArrayMap](https://github.com/aosp-mirror/platform_frameworks_base/blob/master/core/java/android/util/ArrayMap.java)和[android.support.v4.util.ArrayMap](https://github.com/aosp-mirror/platform_frameworks_support/blob/master/compat/src/main/java/android/support/v4/util/ArrayMap.java)包中。 



ArrayMap在设计上比传统的HashMap更多的考虑了内存的优化，可以理解为以时间换空间的一种优化。它使用了两个数组来存储数据——一个整型数组存储键的hashCode，另一个对象数组存储键/值对。这样既能避免为每个存入map中的键创建额外的对象，又能更积极的控制这些数据的长度的增加。因为增加长度只需要拷贝数组中的键，而不是重新构建一个哈希表。

针对Android这种移动平台，也推出了更符合自己的api，比如SparseArray、ArrayMap用来代替HashMap在有些情况下能带来更好的性能提升。 

ArrayMap是一个<key,value>映射的数据结构，它设计上更多的是考虑内存的优化，内部是使用两个数组进行数据存储，一个数组记录key的hash值，另外一个数组记录Value值，它和SparseArray一样，也会对key使用二分法进行从小到大排序，在添加、删除、查找数据的时候都是先使用二分查找法得到相应的index，然后通过index来进行添加、查找、删除等操作，所以，应用场景和SparseArray的一样，如果在数据量比较大的情况下，那么它的性能将退化至少50%。 



推荐的数据结构:
 • ArrayMap<K,V> 替代 HashMap<K,V>
 • ArraySet<K,V> 替代 HashSet<K,V>
 • SparseArray<V> 替代 HashMap<Integer,V>
 • SparseBooleanArray 替代 HashMap<Integer,Boolean>
 • SparseIntArray 替代 HashMap<Integer,Integer>
 • SparseLongArray 替代 HashMap<Integer,Long>
 • LongSparseArray<V> 替代 HashMap<Long,V>