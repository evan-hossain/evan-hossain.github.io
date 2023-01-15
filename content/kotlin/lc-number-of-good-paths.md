---
title: "Leetcode: Number of Good Paths Solution in Kotlin"
date: 2023-01-15T16:48:41Z
draft: false
tags:
- leetcode
- kotlin
---
## 2421. Number of Good Paths
## Solution
I found the problem somewhat harder than typical leetcode problems. My solution idea is as follows:

We need to build the tree by connecting nodes, starting from the nodes with minimum weights to maximum. This will ensure that whenever we are connecting two different nodes, the max weight of those two nodes will be greater or equal any other weights already considered in the relevant sub trees. We will use disjoint set to maintain and connect different trees. Weights of the edges will be maximum value of the related nodes. 

Once we start picking up the edges in order, look at the two sub trees of the two nodes in question using disjoint set. Additionally, we need to maintain two information for each of these sub trees:
1. What is the maximum value in the sub tree?
2. How many times the maximum value occurs in those sub trees?

With these two information maintained in two maps, we can find if the maximum values match in each of the new sub tree we are about to connect. If the values match, increment the result. Also update the two maps accordingly.

```kotlin
class Solution {
    class Edge(val from: Int, val to: Int, val weight: Int)

    private lateinit var parent: IntArray

    fun numberOfGoodPaths(vals: IntArray, edges: Array<IntArray>): Int {
        val sortedEdges = edges.map { Edge(it.first(), it.last(), vals[it.first()].coerceAtLeast(vals[it.last()])) }
            .sortedWith(compareBy { it.weight })
        val n = vals.size
        parent = IntArray(n) { -1 }
        val valCount = mutableMapOf<Pair<Int, Int>, Int>() // count of values in sub trees
        val maxVal = mutableMapOf<Int, Int>() // maintain max value in sub trees
        var result = n

        for (edge in sortedEdges) {
            val uRoot = root(edge.from)
            val vRoot = root(edge.to)
            val uRootMaxVal = maxVal.getOrDefault(uRoot, vals[edge.from])
            val vRootMaxVal = maxVal.getOrDefault(vRoot, vals[edge.to])
            val uVal = valCount.getOrDefault(Pair(uRoot, uRootMaxVal), 1)
            val vVal = valCount.getOrDefault(Pair(vRoot, vRootMaxVal), 1)
            if (uRootMaxVal == vRootMaxVal) result += uVal * vVal
            join(edge.from, edge.to)
            val newRoot = root(edge.from)
            valCount[Pair(newRoot, edge.weight)] = valCount.getOrDefault(
                Pair(uRoot, edge.weight), (vals[edge.from] == edge.weight).compareTo(false)
            ) + valCount.getOrDefault(
                Pair(vRoot, edge.weight), (vals[edge.to] == edge.weight).compareTo(false)
            )
            maxVal[newRoot] = edge.weight
        }
        return result
    }

    private fun root(u: Int): Int = if (parent[u] < 0) u else root(parent[u])
    private fun join(u: Int, v: Int) {
        var uRoot = root(u)
        var vRoot = root(v)
        if (uRoot == vRoot) return
        if (parent[vRoot] < parent[uRoot]) uRoot = vRoot.also { vRoot = uRoot } // rank optimisation
        parent[uRoot] += parent[vRoot]
        parent[vRoot] = uRoot
    }
}
```
