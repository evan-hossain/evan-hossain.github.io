---
title: "Leetcode: Lfu Cache Solution in Kotlin"
date: 2023-01-29T11:41:00Z
draft: false
tags:
- kotlin
- leetcode
- hard
---
## 460. LFU Cache
This problem is pretty much about implementing the requirement. I chose to do it in a more object oriented way. The `Item` and `ItemComparator` classes should be self explanatory with the documentation.

```kotlin
/**
 * key: as stated in the problem statement
 * value: as stated in the problem statement
 * timestamp: an indicator for tracking when this item was last used
 */
class Item(val key: Int, var value: Int, var timestamp: Int) {
    var usageCount = 1
    fun incrementUsage(ts: Int) {
        usageCount++
        timestamp = ts
    }

    override fun toString(): String {
        return "(key: $key, value: $value, ts: $timestamp, count: $usageCount)"
    }
}


class ItemComparator : Comparator<Item> {
    override fun compare(o1: Item?, o2: Item?): Int {
        if (o1 == null || o2 == null) return 0
        if (o1.usageCount == o2.usageCount)
            return o1.timestamp.compareTo(o2.timestamp)
        return o1.usageCount.compareTo(o2.usageCount)
    }
}
```

In the main `LFUCache` class there are a couple of data structure used to implement the requirements described in the problem statement. 

1. `cache` is of type `TreeSet<Item>`. I have used treeset here to keep the items sorted according to their `usageCount` first. And used the recent timestamps to break ties. This treeset helps removing the top item whenever the capacity is reached. Also it provides fast look up ([guaranteed log(n) time cost for the basic operations (add, remove and contains)](https://docs.oracle.com/javase/7/docs/api/java/util/TreeSet.html).
2. `itemTracker` is used to find the `Item` objects quickly in the treeset at different occasions. 

```kotlin

class LFUCache(capacity: Int) {

    private val cache = TreeSet<Item>(ItemComparator())
    private val itemTracker = mutableMapOf<Int, Item>()
    private var timestamp = 0 // used to break tie
    private val cap = capacity

    fun get(key: Int): Int {
        itemTracker[key] ?: let { return -1 }
        return cache.floor(itemTracker[key])!!.also {
            cache.remove(it)
            it.incrementUsage(++timestamp)
            cache.add(it)
        }.value
    }

    fun put(key: Int, value: Int) {
        if (cap == 0) return
        itemTracker[key]?.let {
            cache.floor(it)!!.let { item ->
                cache.remove(it)
                item.value = value
                item.incrementUsage(++timestamp)
                cache.add(it)
            }
            return
        }
        // new key
        if (cache.size == cap) {
            cache.first().let {
                cache.remove(it)
                itemTracker.remove(it.key)
            }
        }
        Item(key, value, ++timestamp).also {
            itemTracker[key] = it
            cache.add(it)
        }
    }
}
```
