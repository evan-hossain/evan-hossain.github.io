---
title: "Leetcode: Shuffle the Array Solution in Kotlin"
date: 2023-02-06T11:06:01Z
draft: false
tags:
- kotlin
- leetcode
---
## 1470. Shuffle the Array
Zip the two half lists, flatten.

```kotlin
class Solution {
    fun shuffle(nums: IntArray, n: Int) = nums.zip(
        nums.drop(n)
    ).flatMap { it.toList() }
}
```
