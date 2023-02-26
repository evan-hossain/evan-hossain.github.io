---
title: "Leetcode: Minimize Deviation in Array"
date: 2023-02-26T20:34:43Z
draft: false
tags:
- kotlin
- leetcode
- hard
---
## 1675. Minimize Deviation in Array
A couple of things to notice.
1. Odd elements can only be changed once, which makes them even.
2. Even elements can be changed up to `Log N` times.

So the overall number of possible numbers is not a lot. We can find all possible transformation for each number, pair them with their initial indexes to ensure uniqueness later. Now if we sort the candidate set by their numbers, we can run a sliding window over the candidates. Whenever the window has all of the indexes appear at least once, we have a potential answer: difference between the max and min values.
```kotlin
class Solution {
    fun minimumDeviation(nums: IntArray): Int {
        val allCandidates = mutableListOf<Pair<Int, Int>>()
        val n = nums.size
        nums.forEachIndexed { index, value ->
            if (value % 2 == 1) {
                allCandidates.add(Pair(value, index))
                allCandidates.add(Pair(value * 2, index))
            } else {
                var v = value
                while (v % 2 == 0) {
                    allCandidates.add(Pair(v, index))
                    v /= 2
                }
                allCandidates.add(Pair(v, index))
            }
        }
        allCandidates.sortBy { it.first }
        val indexMap = mutableMapOf<Int, Int>()
        var (fast, slow, result) = mutableListOf(0, 0, Int.MAX_VALUE)
        while (fast < allCandidates.size) {
            while (fast < allCandidates.size && indexMap.size < n) {
                indexMap[allCandidates[fast].second] = indexMap.getOrDefault(allCandidates[fast].second, 0) + 1
                fast++
            }
            if (indexMap.size == n) result =
                result.coerceAtMost(allCandidates[fast - 1].first - allCandidates[slow].first)
            while (slow < allCandidates.size && indexMap.getOrDefault(allCandidates[slow].second, 0) > 1) {
                indexMap[allCandidates[slow].second] = indexMap[allCandidates[slow].second]!! - 1
                slow++
                if (indexMap.size == n) result =
                    result.coerceAtMost(allCandidates[fast - 1].first - allCandidates[slow].first)
            }
            if (slow < allCandidates.size) {
                indexMap[allCandidates[slow].second] = indexMap[allCandidates[slow].second]!! - 1
                if (indexMap[allCandidates[slow].second] == 0) indexMap.remove(allCandidates[slow].second)
                slow++
            }
        }
        return result
    }
}
```
