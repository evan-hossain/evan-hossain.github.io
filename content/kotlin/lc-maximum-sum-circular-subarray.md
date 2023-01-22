---
title: "Leetcode: Maximum Sum Circular Subarray Solution in Kotlin"
date: 2023-01-22T11:39:24Z
draft: false
tags:
- kotlin
- leetcode
---
## 918. Maximum Sum Circular Subarray
The maximum sum problem can be solved using the well known [Kadane's algo](https://en.wikipedia.org/wiki/Maximum_subarray_problem). But this problem becomes a bit special because of the circular element. Now the answer for the problem can be one of the following two results.

1. Usual maximum sum of the array.
2. Maximum sum of a prefix + maximum sum of a suffix of the array.

The first part is easy following the Kadane's algo. As for the second part, we can first pre-calculate maximum suffix some on the right for any position. Then run prefix sum and add the maximum suffix sum on the right to get maximum possible sum using this prefix sum. Maximum of all of these is the answer.

```kotlin
class Solution {
    fun maxSubarraySumCircular(nums: IntArray): Int {
        var sum = 0
        var result = nums.max()!!
        if (result < 0) return result
        for (i in nums.indices) {
            sum += nums[i]
            if (sum < 0) sum = 0
            result = result.coerceAtLeast(sum)
        }
        val prefixSum = MutableList(nums.size + 1) { 0 }
        val rightMax = MutableList(nums.size + 1) { 0 }
        val suffixSum = MutableList(nums.size + 1) { 0 }
        for (i in 1 until nums.size + 1) prefixSum[i] = prefixSum[i - 1] + nums[i - 1]
        for (i in nums.size - 1 downTo 1) {
            suffixSum[i] = suffixSum[i + 1] + nums[i]
            rightMax[i] = rightMax[i + 1].coerceAtLeast(suffixSum[i])
        }
        for (i in nums.indices) result = result.coerceAtLeast(prefixSum[i] + rightMax[i])

        return result
    }
}
```
