---
title: "Leetcode: Shortest Path With Alternating Colors"
date: 2023-02-26T19:15:08Z
draft: false
tags:
- kotlin
- leetcode
---
## 1129. Shortest Path with Alternating Colors
Pretty much usual BFS problem with another dimension, colors. We need to store a Pair of NodeId and Color in the queue instead of only nodes. And keep track of distances accordingly.
```kotlin
import java.util.*

class Solution {
    fun shortestAlternatingPaths(n: Int, redEdges: Array<IntArray>, blueEdges: Array<IntArray>): IntArray {
        val dist = Array(n) { Array(2) { mutableMapOf<Pair<Int, Int>, Int>() } }
        val queue = LinkedList<Pair<Triple<Int, Int, Int>, Int>>().also {
            it.addAll(
                listOf(
                    Pair(Triple(0, 0, 0), RED),
                    Pair(Triple(0, 0, 0), BLUE)
                )
            )
        }
        dist[0][RED][Pair(0, 0)] = 0
        dist[0][BLUE][Pair(0, 0)] = 0
        val adjList = Array(n) { Array(2) { mutableListOf<Int>() } }
        redEdges.forEach { adjList[it.first()][RED].add(it.last()) }
        blueEdges.forEach { adjList[it.first()][BLUE].add(it.last()) }
        while (queue.isNotEmpty()) {
            val (node, color) = queue.pop()
            val (u, last, secondLast) = node
            val lastPair = Pair(last, secondLast)
            val newPair = Pair(u, last)
            val newColor = 1 - color
            adjList[u][newColor].forEach { v ->
                val newDist = dist[u][color][lastPair]!! + 1
                val vDist = dist[v][newColor].getOrDefault(newPair, Int.MAX_VALUE)
                if (newDist < vDist) {
                    queue.add(Pair(Triple(v, u, last), newColor))
                    dist[v][newColor][newPair] = newDist
                }
            }
        }
        return (0 until n).map { node ->
            (0 until 2).mapNotNull { color ->
                dist[node][color].values.min()
            }.min()
        }.map { it ?: -1 }.toIntArray()
    }

    companion object {
        const val RED = 0
        const val BLUE = 1
    }
}
```
