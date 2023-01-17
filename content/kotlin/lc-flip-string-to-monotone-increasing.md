---
title: "Leetcode: Flip String to Monotone Increasing Kotlin Solution"
date: 2023-01-17T21:51:22Z
draft: false
tags:
- kotlin
- leetcode
---
## 926. Flip String to Monotone Increasing
At each position, consider two scenarios:

1. How many digits do we need to change if we want to make everything 0 until this position and everything 1 starting from this index?

2. How many digits do we need to change if we want to make everything 0 including this position and everything 1 starting from the next index?

Minimum of all of these is the answer.

```kotlin
class Solution {
    fun minFlipsMonoIncr(s: String): Int {
        val n = s.length
        val totalOnes = s.count { it == '1' }
        val totalZeros = n - totalOnes
        var zeros = 0
        var ones = 0
        return s.map {
            val makeOneFromNextPosition = ones + totalZeros - zeros // 00001..1111
            zeros += (it == '0').compareTo(false)
            ones += (it == '1').compareTo(false)
            val makeOneFromThisPosition = ones + totalZeros - zeros // 00011...1111
            makeOneFromNextPosition.coerceAtMost(makeOneFromThisPosition).toInt()
        }.min()!!
    }
}
```
