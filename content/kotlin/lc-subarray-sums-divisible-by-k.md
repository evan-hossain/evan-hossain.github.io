---
title: "Leetcode: Subarray Sums Divisible by K Solution in Kotlin"
date: 2023-01-22T11:34:44Z
draft: false
tags:
- kotlin
- leetcode
---
## 974. Subarray Sums Divisible by K
Well known sub-array divisibility problem. Only catch was that the numbers could be negative. So don't forget to handle modulus for negative values.

```kotlin
class Solution {
    fun subarraysDivByK(nums: IntArray, k: Int): Int {
        val counter = mutableMapOf(Pair(0, 1))
        var sum = 0
        var result = 0
        for (num in nums) {
            sum += num
            sum %= k
            if (sum < 0) sum = (sum + k) % k
            result += counter.getOrDefault(sum, 0)
            counter[sum] = counter.getOrDefault(sum, 0) + 1
        }
        return result
    }
}
```
