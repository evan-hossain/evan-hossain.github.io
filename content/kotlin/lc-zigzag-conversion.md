---
title: "Leetcode: Zigzag Conversion Solution in Kotlin"
date: 2023-02-06T10:46:45Z
draft: false
tags:
- kotlin
- leetcode
---
## 6. Zigzag Conversion
Create a dummy 2D array, `canvas` and draw the pattern recursively. Note that the directions only change on the first and last rows. So we can change the direction variables in the recursion during those two events, otherwise just calculate the new position accordingly. 
```kotlin
class Solution {
    fun convert(s: String, numRows: Int): String {
        if (numRows == 1 || numRows > s.length) return s
        val canvas = Array(numRows) { MutableList(s.length) { '#' } }

        fun draw(row: Int = 0, col: Int = 0, rowDirection: Int = 0, colDirection: Int = 0, index: Int = 0) {
            if (index == s.length) return
            canvas[row][col] = s[index]
            when (row) {
                0 -> draw(row + 1, col, 1, 0, index + 1)
                numRows - 1 -> draw(row - 1, col + 1, -1, 1, index + 1)
                else -> draw(row + rowDirection, col + colDirection, rowDirection, colDirection, index + 1)
            }
        }
        draw()
        return canvas.map { row -> row.filter { it != '#' }.joinToString("") }.joinToString("")
    }
}
```
