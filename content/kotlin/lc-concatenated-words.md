---
title: "Leetcode: Concatenated Words Solution in Kotlin"
date: 2023-01-27T20:57:58Z
draft: false
tags:
- kotlin
- leetcode
---
## 472. Concatenated Words
The problem basically asks to check which of the given words can be composed by concatenating two or more other words. In other words, we can pick each word and check if it can be composed of two or more other words from the list. Note that although there are 10,000 words, the length of each word is at most 30. For each string, we can 

* Recursively check all possible partitions of the string. 
* If each of the partition exists in the given word list, that means the original word in question can be constructed by concatenating these partitions. 

But recursively checking all partitions and looking up strings every time can be taxing. We can put all the initial strings in a Set for quick look up and use memoization (dynamic programming) to optimise the recursive partitioning part and avoid computing same substring multiple times.
```kotlin
class Solution {
    fun findAllConcatenatedWordsInADict(words: Array<String>): List<String> {
        val wordSet = words.toMutableSet()
        var dp: Array<IntArray> = arrayOf()
        fun isConcatenated(from: Int, to: Int, word: String): Int {
            if (from > to) return 0
            var ret = dp[from][to]
            if (ret > -1) return ret
            if (wordSet.contains(word.substring(IntRange(from, to))))
                return 1
            ret = 0
            for (i in from until to) {
                if (isConcatenated(from, i, word) == 1 && isConcatenated(i + 1, to, word) == 1) {
                    ret = 1
                    break
                }
            }
            dp[from][to] = ret
            return ret
        }
        return words
            .sortedWith(compareBy { it.length })
            .reversed() // sort in descending order of length of string
            .map { word ->
                wordSet.remove(word) // safe to remove since this is the largest string by length
                dp = Array(word.length) { IntArray(word.length) { -1 } }
                Pair(word, isConcatenated(0, word.length - 1, word))
            }
            .filter { it.second == 1 } // can be constructed by others
            .map { it.first }
    }
}
```
