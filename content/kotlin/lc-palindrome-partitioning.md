---
title: "Leetcode: Palindrome Partitioning Solution in Kotlin"
date: 2023-01-22T10:26:53Z
draft: false
tags:
- kotlin
- leetcode
---
## 131. Palindrome Partitioning
The idea is to recursively partition the given string and check if the newly created partition forms a palindrome. One optimisation could be pre-calculating all the \\({\Omicron(N^2)}\\) partitions for quick palindrome look up. But considering there are only 16 characters in total, it's not super important.
```kotlin
class Solution {
    fun partition(s: String): List<List<String>> {
        fun isPalindrome(p: String) =
            p == p.reversed() // this can be optimised by precalculating. But seems overkill for 16 chars

        val results = mutableListOf<List<String>>()
        fun rec(index: Int, partition: List<String>) {
            if (index >= s.length) {
                results.add(partition)
                return
            }
            for (i in index until s.length) {
                val newPartition = s.slice(IntRange(index, i))
                if (isPalindrome(newPartition))
                    rec(i + 1, partition.plus(newPartition))
            }
        }
        rec(0, listOf())
        return results
    }
}
```
