---
title: "Leetcode: Greatest Common Divisor of Strings Solution in Kotlin"
date: 2023-02-06T10:35:20Z
draft: false
tags:
- kotlin
- leetcode
---
## 1071. Greatest Common Divisor of Strings
The brute force way would be considering all the prefixes of one of the strings and simulate to see if that is a candidate answer. But we can improve it a bit by taking GCD of the two string lengths. That GCD, G tells us a couple of things:
1. The result can be at most G.
2. The result must be a divisor of G, that is it must divide both string lengths.
3. We can start from G and go down till 1. If we find the result at any point, that's the best result.
```kotlin
class Solution {
    fun gcd(a: Int, b: Int): Int = if (b == 0) a else gcd(b, a % b)
    fun gcdOfStrings(str1: String, str2: String): String {
        val len1 = str1.length
        val len2 = str2.length
        val maxLength = gcd(len1, len2)
        for (len in maxLength.downTo(1)) {
            if (maxLength % len > 0) continue // len must be a divisor of gcd
            var matchFound = true
            for (start in 0 until len) {
                if (!matchFound) break
                
                for (index in start until len1 step len) {
                    if (str1[index] != str1[start]) {
                        matchFound = false
                        break
                    }
                    if (index < len2 && str1[index] != str2[index]) {  // cross-match with other sting
                        matchFound = false
                        break
                    }
                }

                if (!matchFound) break

                for (index in start until len2 step len) {
                    if (str2[index] != str2[start]) {
                        matchFound = false
                        break
                    }
                    if (index < len1 && str1[index] != str2[index]) {  // cross-match with other sting
                        matchFound = false
                        break
                    }
                }
            }
            if (matchFound)
                return str1.substring(IntRange(0, len - 1))
        }
        return ""
    }
}
```
