---
title: "Leetcode: Gas Station solution in kotlin"
date: 2023-01-07T23:41:20Z
draft: false
tags:
- kotlin
- leetcode
---
```kotlin
class Solution {
    fun canCompleteCircuit(gas: IntArray, cost: IntArray): Int {
        val gain = gas.zip(cost).map { it.first - it.second }
        val gainAppended = gain.plus(gain)
        var (result, sum, maxSum, start, end) = arrayOf(-1, 0, -1, 0, 0)
        while (end < gainAppended.size) {
            sum += gainAppended[end++]
            if (sum < 0) {
                sum = 0
                start = end
                continue
            }
            if (maxSum < sum) {
                maxSum = sum
                result = start
            }
        }
        return if (result == -1 || gainAppended.subList(result, result + gain.size)
                .fold(mutableListOf(0)) { acc, i -> acc.apply { acc.add(i + acc.last()) } }.min()!! < 0
        ) -1 else result
    }
}
```
