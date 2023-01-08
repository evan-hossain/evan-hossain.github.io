---
title: "Leetcode: Max Points on a Line in Kotlin"
date: 2023-01-08T22:15:27Z
draft: false
tags:
- kotlin
- leetcode
---
The basic idea is translating all the points with respect to each point. Then find and count the slope using map of [x/gcd(x,y), y/gcd(x.y), sign_of_the_slope].
```kotlin
class Solution {
    fun maxPoints(points: Array<IntArray>): Int {
        fun gcd(i: Int, j: Int): Int = if (j == 0) 1.coerceAtLeast(i) else gcd(j, i % j)
        fun translate(point: IntArray): List<IntArray> = points.map { intArrayOf(it[0] - point[0], it[1] - point[1]) }
        fun getSign(point: IntArray) = ((point.first() > 0) == (point.last() > 0)).compareTo(false)

        return points.size.coerceAtMost(points
            .map { origin ->
                translate(origin).map { newArray ->
                    val g = gcd(newArray[0], newArray[1])
                    listOf(newArray[0] / g, newArray[1] / g, getSign(intArrayOf(newArray[0] / g, newArray[1] / g)))
                }
                    .groupingBy { it }
                    .eachCount()
                    .maxBy { it.value }!!.value
            }.maxBy { it }!! + 1
        )
    }
}
```
