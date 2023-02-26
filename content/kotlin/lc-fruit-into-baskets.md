---
title: "Leetcode: Fruit Into Baskets"
date: 2023-02-26T18:34:21Z
draft: false
tags:
- kotlin
- leetcode
---
## 904. Fruit Into Baskets
Maintain a sliding window to keep track of indexes of at most two different types of fruits. Maximum length of this window is our answer.

Now, whenever we need to replace one fruit to try extending the window with a new one, how to choose which fruit to remove? Since the existing fruits could appear at various places. Just removing the one that appeared earlier might not be optimal. This is where the indexes come into play. We should keep the fruit which appeared latest and remove the other one, and calculate the window size accordingly.

```kotlin
class Solution {
    fun totalFruit(fruits: IntArray): Int {
        val runningMap = mutableMapOf<Int, Int>()
        var result = 0
        var currentBest = 0
        for(fruit in fruits.withIndex()){
            if(runningMap.containsKey(fruit.value) || runningMap.size <= 1){
                currentBest++
                result = result.coerceAtLeast(currentBest)
                runningMap[fruit.value] = fruit.index  // latest index of that fruit
                continue
            }
            // need to remove one, optimal is keeping the one which appeared latest
            val keyWithMinValue = runningMap.minBy { it.value }!!
            runningMap.remove(keyWithMinValue.key)
            currentBest = fruit.index - keyWithMinValue.value // this is still remaining
            runningMap[fruit.value] = fruit.index
        }
        return result
    }
}
```

