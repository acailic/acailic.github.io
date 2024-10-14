---
title: Patterns for Coding Interview
layout: post
tags: [article, patterns]
date: 2024-10-14
---


| Problem Type / Keywords | Likely Pattern | Key Advice | Common Data Structures / Algorithms |
|-------------------------|----------------|------------|--------------------------------------|
| "Two pointers", "Sliding window" | Two Pointers | Consider using two pointers moving in the same or opposite directions | Array, String, LinkedList |
| "Subarray", "Contiguous" | Sliding Window | Think about expanding/contracting a window | Array, HashMap |
| "Tree", "Binary Tree" | Tree Traversal | Consider recursive and iterative approaches; Use DFS or BFS | Stack, Queue, Recursion |
| "Graph", "Connected" | Graph Traversal | Think about DFS or BFS | Queue (BFS), Stack/Recursion (DFS) |
| "Sorted array" | Binary Search, Two Pointers | Look for opportunities to eliminate half the search space; Use two pointers | Array |
| "Dynamic Programming", "Maximum/Minimum" | DP | Break problem into subproblems, look for overlapping subproblems | 2D Array, Memoization |
| "Parentheses", "Valid sequence" | Stack | Think about using a stack to match opening/closing elements | Stack |
| "Top K", "Kth largest/smallest" | Heap, QuickSelect | Consider using a heap to keep track of K elements; Use QuickSelect for O(n) average time | PriorityQueue/Heap |
| "Intervals", "Overlapping" | Interval Manipulation | Sort intervals, then process | Array, Sorting |
| "Linked List", "Cycle" | Fast & Slow Pointers | Think about using two pointers moving at different speeds | LinkedList |
| "Trie", "Prefix" | Trie | Consider building a trie for efficient prefix operations | Trie |
| "Bit manipulation" | Bitwise Operations | Think about using XOR, AND, OR, shift operations | Integers |
| "Permutation", "Combination" | Backtracking | Consider building a recursive solution with backtracking | Recursion, Array |
| "Monotonic stack/queue" | Monotonic Stack | Think about maintaining a monotonic (increasing/decreasing) stack | Stack, Queue |
| "Union Find", "Disjoint Set" | Union Find | Consider grouping elements and finding connections | Array, Tree |
| "In-place operation" | In-place Algorithms | Swap corresponding values; Store multiple values in the same pointer | Array |
| "Common strings" | Map, Trie | Use a map for counting; Consider a trie for prefix-based operations | HashMap, Trie |
| "Recursion banned" | Iterative Approaches | Use a stack to simulate recursion | Stack |
| "General problem" | Hash Table, Sorting | Use Map/Set for O(1) time & O(n) space; Sort input for O(nlogn) time and O(1) space | HashMap, Array |

