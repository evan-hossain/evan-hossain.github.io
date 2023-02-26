---
title: "Leetcode: Single Element in a Sorted Array"
date: 2023-02-26T19:45:24Z
draft: false
tags:
- kotlin
- leetcode
---
## 540. Single Element in a Sorted Array
There are simpler methods like finding xor of all elements. However, the time complexity would be `O(N)` in that case.
To find the element in `O(log N)`, as stated in the problem, we can use divide and conquer technique. 

Few things to observe:
1. The number of elements in the array is odd.
2. If we divide the list in half, there can be these two cases. 

	2.1. Previous or next number is different, we got the number.

	2.2. Previous or next number matches. Means we can discard either left or right half completely. Now the first observation comes into play again. After discarding this matching pair, we should only look at the half where remaining number of elements is odd.
```kotlin
class Solution {
    fun singleNonDuplicate(nums: IntArray, from: Int = 0, to: Int = nums.size - 1): Int {
        if (from == to) return nums[from]
        // must have odd number of elements remaining
        val mid = (from + to) / 2
        val newRanges = mutableListOf<Pair<Int, Int>>() // hold possible new divisions
        if (nums[mid] == nums[mid - 1]) {
            newRanges.add(Pair(from, mid - 2))
            newRanges.add(Pair(mid + 1, to))
        }
        if (nums[mid] == nums[mid + 1]) {
            newRanges.add(Pair(from, mid - 1))
            newRanges.add(Pair(mid + 2, to))
        }
        if (newRanges.isEmpty()) return nums[mid] // landed on single element
        // take the range that has odd number of element
        val newRange = newRanges.single { (it.second - it.first) % 2 == 0 }
        return singleNonDuplicate(nums, newRange.first, newRange.second)
    }
}
```
