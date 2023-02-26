---
title: "Leetcode: as Far From Land as Possible"
date: 2023-02-26T19:05:10Z
draft: false
tags:
- kotlin
- leetcode
---
## 1162. As Far from Land as Possible
This is a classic BFS problem. Put all the land cells in the queue and run Breadth First Search to find farthest water cell or vice versa. 

```kotlin
import java.util.*

class Solution {
    fun maxDistance(grid: Array<IntArray>): Int {
        val n = grid.size
        val ones = grid.mapIndexed { i, row ->
            row.mapIndexed { j, item ->
                if (item == 1) Pair(i, j) else null
            }.filterNotNull()
        }.flatten()
        if (ones.isEmpty() || ones.size == n * n)
            return -1
        val queue = LinkedList<Pair<Int, Int>>(ones)
        val dist = Array(n) { IntArray(n) { Int.MAX_VALUE } }
        ones.forEach { dist[it.first][it.second] = 0 }

        while (queue.isNotEmpty()) {
            val u = queue.pop()!!
            for (dir in listOf(Pair(0, 1), Pair(0, -1), Pair(1, 0), Pair(-1, 0))) {
                val v = Pair(u.first + dir.first, u.second + dir.second)
                if (!(v.first in 0 until n && v.second in 0 until n))
                    continue
                if (dist[v.first][v.second] > dist[u.first][u.second] + 1) {
                    dist[v.first][v.second] = dist[u.first][u.second] + 1
                    queue.add(v)
                }
            }
        }
        return dist.map { row ->
            row.filter { it < Int.MAX_VALUE }.max()!!
        }.max()!!
    }
}
```
