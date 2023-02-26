---
title: "Leetcode: Minimum Fuel Cost to Report to the Capital"
date: 2023-02-26T19:30:02Z
draft: false
tags:
- kotlin
- leetcode
---
## 2477. Minimum Fuel Cost to Report to the Capital
At each node, we just need to know how many people are passing through this node. Then, the cost is `ceil(people/seats)`. This can be done in a single DFS, returning a pair of (totalPeople, totalCost) from current subtree.
```kotlin
class Solution {
    private fun calculateCost(u: Int, par: Int, adjList: Array<MutableList<Int>>, seats: Int): Pair<Int, Long> {
        var totalPeople = 1
        var totalCost: Long = 0
        adjList[u]
            .filter {
                it != par
            }.forEach { v ->
                val (people, cost: Long) = calculateCost(v, u, adjList, seats)
                totalPeople += people
                totalCost += people / seats + if (people % seats > 0) 1 else 0
                totalCost += cost
            }
        return Pair(totalPeople, totalCost)
    }

    fun minimumFuelCost(roads: Array<IntArray>, seats: Int): Long {
        val n = roads.size + 1
        val adjList = Array(n) { mutableListOf<Int>() }
        roads.forEach {
            adjList[it.first()].add(it.last())
            adjList[it.last()].add(it.first())
        }
        return calculateCost(0, -1, adjList, seats).second
    }
}
```
