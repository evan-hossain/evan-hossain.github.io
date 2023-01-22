---
title: "Leetcode: Non Decreasing Subsequences Solution in Kotlin"
date: 2023-01-22T11:24:05Z
draft: false
tags:
- kotlin
- leetcode
---
## 491. Non-decreasing Subsequences
Brute force recursive idea works. The only tricky part is handling the situation when there are a group of same digit. In this case, we can take no digit from this group or one or more digits from this group. But in all those scenarios, we should skip the remaining digits from this particular group and jump to the next different digit. Otherwise, we will be double counting some sub-sequences. I've pre-calculated this next candidate digit to jump to for each character but I believe finding such candidates each time should work fine too.

```kotlin
class Solution {
    fun findSubsequences(nums: IntArray): List<List<Int>> {
        val result = mutableSetOf<List<Int>>()
        val sameCount = Array(nums.size) {
            var count = 0
            for (i in it until nums.size) {
                if (nums[i] == nums[it])
                    count++
                else
                    break
            }
            count
        }

        fun rec(index: Int, current: List<Int>, last: Int?) {
            if (index >= nums.size) {
                if (current.size > 1) result.add(current.toList())
                return
            }
            val jumpTo = index + sameCount[index]
            rec(jumpTo, current, last)
            last?.let { if (last > nums[index]) return }
            val temList = mutableListOf<Int>()
            for (i in index until nums.size) {
                if (nums[index] != nums[i]) break
                temList.add(nums[i])
                rec(jumpTo, current.plus(temList), nums[i])
            }
        }
        rec(0, listOf(), null)
        return result.toList()
    }
}
```
