---
title: "Leetcode: Ipo"
date: 2023-02-26T20:01:44Z
draft: false
tags:
- kotlin
- leetcode
- hard
---
## 502. IPO
Note that, at any point, if we know what is the most profitable project within our current budget, we can solve this problem. That is the single most important observation required. Then we can just sort the `(capital, project)` pair by capital and keep taking the most profitable project within our current capital. 
```kotlin
import java.util.*

class Solution {
    fun findMaximizedCapital(k: Int, w: Int, profits: IntArray, capital: IntArray): Int {
        val profitsAndCapitalSortedByCapital = profits.zip(capital).sortedBy { it.second }
        val availableProfitCounts = TreeMap<Int, Int>()
        var (projectsDone, capitalIterator, totalCapital) = listOf(0, 0, w)
        while (projectsDone < k) {
            // make profits available which has required capital <= totalCapital
            while (capitalIterator < profitsAndCapitalSortedByCapital.size &&
                profitsAndCapitalSortedByCapital[capitalIterator].second <= totalCapital
            ) {
                val profit = profitsAndCapitalSortedByCapital[capitalIterator].first
                availableProfitCounts[profit] = availableProfitCounts.getOrDefault(profit, 0) + 1
                capitalIterator++
            }
            if (availableProfitCounts.isEmpty()) break // hopeless
            // take the max available profit
            availableProfitCounts.pollLastEntry().let { (maxProfit, freq) ->
                totalCapital += maxProfit
                projectsDone++
                if (freq > 1) availableProfitCounts[maxProfit] = freq - 1
            }
        }
        return totalCapital
    }
}
```
