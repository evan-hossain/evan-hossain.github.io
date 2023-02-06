---
title: "Leetcode: N Th Tribonacci Number Kotlin Solution"
date: 2023-02-06T10:14:40Z
draft: false
tags:
- kotlin
- leetcode
---
## 1137. N-th Tribonacci Number
Since each number only depends on the last three numbers, we can pre compute the results in an array and return the results in \\({\Omicron(1)}\\) for all the queries. 
```kotlin
class Solution {

    init {
        dp[0] = 0
        dp[1] = 1
        dp[2] = 1
        for (i in 3 until 38) {
            dp[i] = (1 until 4).map { dp[i - it] }.sum()
        }
    }

    fun tribonacci(n: Int) = dp[n]

    companion object {
        val dp = IntArray(38)
    }
}
```
