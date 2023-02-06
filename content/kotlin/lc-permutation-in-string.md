---
title: "Leetcode: Permutation in String Solution in Kotlin"
date: 2023-02-06T10:50:50Z
draft: false
tags: 
- kotlin
- leetcode
---
## 567. Permutation in String
To verify if a substring in `S2` is a permutation of `S1`, counting the frequency of characters in that substring and comparing them with characters frequency of `S1` should do it. To do that optimally, we can first calculate and preserve the character frequency of string `S1`. Then run a sliding window of `S1.length` on `S2`. For each movement of the window (substring), update the character frequencies and check if the current window contains `S1`'s permutation.
```kotlin
class Solution {
    fun checkInclusion(s1: String, s2: String): Boolean {
        if (s2.length < s1.length) return false
        val s1CharFrequency = s1.groupingBy { it }.eachCount()
        val s2RunningFrequency = s2.slice(IntRange(0, s1.length - 1)).groupingBy { it }.eachCount().toMutableMap()

        fun isPerm() = s1CharFrequency.map { (k, v) -> s2RunningFrequency.getOrDefault(k, 0) >= v }.all { it }

        if (isPerm()) return true

        for (i in s1.length until s2.length) {
            val add = s2[i]
            val remove = s2[i - s1.length]
            s2RunningFrequency[add] = s2RunningFrequency.getOrDefault(add, 0) + 1
            s2RunningFrequency[remove] = s2RunningFrequency.getOrDefault(remove, 0) - 1
            if (isPerm()) return true
        }
        return false
    }
}
```
