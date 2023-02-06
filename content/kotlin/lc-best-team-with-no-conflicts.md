---
title: "Leetcode: Best Team With No Conflicts Kotlin solution"
date: 2023-02-06T10:22:39Z
draft: false
tags:
- kotlin
- leetcode
---
## 1626. Best Team With No Conflicts

Since there are two variable values, it'll be easier to work with if we could put them into some kind of order. Order by any of them shall be okay but since players' age has a lower limit, we should order by score and potentially use age a dimension in our solution using dynamic programming technique.

Now since the items are sorted in ascending order of <score, age>, at any point, we can certainly say that current player has score >= than any previous player. But to really consider the current player in the team, we need to make sure they are definitely not younger than any player previously. So we shall maintain the maximum age of the taken players till now, thus it becomes our second index for our DP solution.

```kotlin
class Solution {
    fun bestTeamScore(scores: IntArray, ages: IntArray): Int {
        val agesAndScores = ages.zip(scores).sortedWith(
            compareBy({ it.second }, { it.first })
        )
        val n = scores.size
        val dp = Array(n) { IntArray(1002) { -1 } }

        fun compute(index: Int, maxAge: Int): Int {
            if (index == n) return 0
            var ret = dp[index][maxAge]
            if (ret > -1) return ret
            ret = compute(index + 1, maxAge)
            if (agesAndScores[index].first >= maxAge)
                ret = ret.coerceAtLeast(
                    compute(
                        index + 1,
                        maxAge.coerceAtLeast(agesAndScores[index].first)
                    ) + agesAndScores[index].second
                )
            return ret.also { dp[index][maxAge] = it }
        }

        return compute(0, 0)
    }
}
```
