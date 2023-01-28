---
title: "Leetcode: Data Stream as Disjoint Intervals Solution in Kotlin"
date: 2023-01-28T12:13:55Z
draft: false
tags:
- kotlin
- leetcode
---
## 352. Data Stream as Disjoint Intervals
In this problem, we need to maintain a list of intervals and return the list for each `getIntervals()` call. Since there will be a lot of such calls to get intervals, we should think of a way to keep the lists prepared beforehand. 

But there is another part to the problem: `addNum` calls every now and then is going to impact the prepared list. So we need to find a smart way to add the new numbers and keep the ranges prepared at the same time. 

While adding a new value to the stream, we can consider exactly four different scenario:

1. The new value is already part of one of the ranges that we already have prepared.
2. The new value can extend the end range for one of the ranges.
3. The new value can extend the start range for one of the ranges.
4. The new value should be added as a new range.

Once we have listed the possibilities, the rest is implementation. We can use a Sorted Set or TreeSet in Kotlin to maintain the ranges where the key is the start and value is the end for each range. Using a sorted set allows us to find the lower bound (floorEntry for TreeSet) and upper bound (higherEntry) and quickly update the ranges accordingly.

```kotlin
import java.util.*

class SummaryRanges() {

    fun addNum(value: Int) {
        val floorEntry = sortedRangesByStart.floorEntry(value)
        val higherEntry = sortedRangesByStart.higherEntry(value)

        // value is part of a range already, skip
        floorEntry?.let {
            if (value >= it.key && value <= it.value) return
        }

        // value should extend the floorEntry
        floorEntry?.let { floorIt ->
            if (value == floorIt.value + 1) {
                sortedRangesByStart[floorIt.key] = value
                // check if it combines next range as well
                higherEntry?.let {
                    if (value == it.key - 1) {
                        sortedRangesByStart.remove(it.key)
                        sortedRangesByStart[floorIt.key] = it.value
                    }
                }
                return
            }
        }

        // value should extend higherEntry
        higherEntry?.let {
            if (value == it.key - 1) {
                sortedRangesByStart.remove(it.key)
                sortedRangesByStart[value] = it.value
                return
            }
        }

        // value should be a new entry
        sortedRangesByStart[value] = value
    }

    fun getIntervals(): Array<IntArray> = sortedRangesByStart.map { it.toPair().toList().toIntArray() }.toTypedArray()

    companion object {
        val sortedRangesByStart = TreeMap<Int, Int>()
    }
}
```
