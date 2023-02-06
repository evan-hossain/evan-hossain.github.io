---
title: "Leetcode: Verifying an Alien Dictionary Solution in Kotlin"
date: 2023-02-06T10:42:15Z
draft: false
tags:
- kotlin
- leetcode
---
## 953. Verifying an Alien Dictionary
Create a map of the unusual indexing of characters, use the map to compare adjacent strings' order.

```kotlin
class Solution {
    fun isAlienSorted(words: Array<String>, order: String): Boolean {
        val indexMap = order.toList().zip(0 until 26).toMap()
        fun orderedAlright(first: String, second: String): Boolean {
            first.zip(second).map {
                if (indexMap[it.first]!! < indexMap[it.second]!!)
                    return true
                if (indexMap[it.first]!! > indexMap[it.second]!!)
                    return false
            }
            return first.length <= second.length
        }

        return words.zip(words.drop(1)).map { orderedAlright(it.first, it.second) }
            .all { it }
    }
}
```
