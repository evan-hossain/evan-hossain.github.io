---
title: "Leetcode: Binary Tree Preorder Traversal in Kotlin"
date: 2023-01-09T20:05:03Z
draft: false
tags:
- kotlin
- leetcode
---
## 144. Binary Tree Preorder Traversal
Given the root of a binary tree, return the preorder traversal of its nodes' values.
## Solution
Traverse the tree in pre-order using DFS.
```kotlin
class Solution {
    fun preorderTraversal(root: TreeNode?): List<Int> = if (root == null) listOf()
    else listOf(root.`val`).plus(preorderTraversal(root.left)).plus(preorderTraversal(root.right))
```
