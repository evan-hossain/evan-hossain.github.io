---
title: "Leetcode: Restore Ip Addresses Solution in Kotlin"
date: 2023-01-22T11:16:39Z
draft: false
tags:
- kotlin
- leetcode
---
## 93. Restore IP Addresses
This problem could be solved in recursive or iterative way. I chose to code in the iterative way.

Run three For loops to divide the string into four partitions. Then for each partitions, see if they fall in the valid range of IP address. Two caveats:

1. The numbers might be too large to convert to Int or Long. So check the length first, anything with more than three digits is not worth converting to int as it will surely be larger than 255.
2. Avoid numbers with leading zeroes like "000..". This can simply be done by converting the string to number and converting it to string again.

```kotlin
class Solution {
    fun restoreIpAddresses(s: String): List<String> {
        fun validCandidate(num: String): Boolean {
            if (num.length > 3 || num.toInt() > 255) return false
            return num.toInt().toString().length == num.length
        }

        val n = s.length
        val results = mutableSetOf<String>()
        for (i in 0 until n) {
            val one = s.slice(IntRange(0, i))
            if (!validCandidate(one)) continue
            for (j in i + 1 until n) {
                val two = s.slice(IntRange(i + 1, j))
                if (!validCandidate(two)) continue
                for (k in j + 1 until n - 1) {
                    val three = s.slice(IntRange(j + 1, k))
                    val four = s.slice(IntRange(k + 1, n - 1))
                    if (!validCandidate(three)) continue
                    if (!validCandidate(four)) continue
                    results.add("$one.$two.$three.$four")
                }
            }
        }
        return results.toList()
    }
}
```
