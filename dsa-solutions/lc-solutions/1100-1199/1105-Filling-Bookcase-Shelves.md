---
id: filling-bookcase-shelves
title: Filling Bookcase Shelves
sidebar_label: 1105 - Filling Bookcase Shelves
tags: [Dynamic Programming, Array, C++]
description: Solve the problem of minimizing the total height of a bookcase by arranging books on shelves, adhering to a specified shelf width.
---

## Problem Statement

### Problem Description

You are given an array `books` where `books[i] = [thickness_i, height_i]` indicates the thickness and height of the `i`-th book. You are also given an integer `shelfWidth` representing the width of the shelf.

We want to place these books in order onto bookcase shelves that have a total width of `shelfWidth`.

We choose some of the books to place on this shelf such that the sum of their thickness is less than or equal to `shelfWidth`, then build another level of the shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down. We repeat this process until there are no more books to place.

Note that at each step of the above process, the order of the books we place is the same order as the given sequence of books.

For example, if we have an ordered list of 5 books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth books on the last shelf.

Return the minimum possible height that the total bookshelf can be after placing shelves in this manner.

### Example

**Example 1:**
```
Input: `books = [[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelfWidth = 4`

Output: `6`
```
**Explanation:**

- The sum of the heights of the 3 shelves is 1 + 3 + 2 = 6.
- Notice that book number 2 does not have to be on the first shelf.
```
**Example 2:**

Input: `books = [[1,3],[2,4],[3,2]], shelfWidth = 6`

Output: `4`
```
### Constraints

- $1 \leq \text{books.length} \leq 1000$
- $1 \leq \text{thickness}_i \leq \text{shelfWidth} \leq 1000$
- $1 \leq \text{height}_i \leq 1000$

## Solution

### Intuition

To solve this problem, we can use dynamic programming (DP) to track the minimum height required to place the first `i` books on the shelf. 

Let `dp[i]` represent the minimum height of the bookshelf after placing the first `i` books. We can iterate through each book and decide whether to place it on the current shelf or start a new shelf:

1. **Starting a New Shelf**: This means `dp[i] = dp[j] + height_i`, where `j < i` and `books[j + 1]` to `books[i]` are placed on the new shelf.

2. **Placing on Current Shelf**: We calculate the sum of thicknesses and update the maximum height for the books on the current shelf. The goal is to minimize `dp[i]` for each book.

### Time Complexity

- **Time Complexity**: The time complexity of this approach is $O(N^2)$, where `N` is the number of books. For each book, we might need to check all previous books to decide whether to place them on the same shelf or not.

### Space Complexity

- **Space Complexity**: The space complexity is $O(N)$, which is used to store the `dp` array.

### Code
#### C++
```cpp
class Solution {
public:
    int minHeightShelves(vector<vector<int>>& books, int shelfWidth) {
        int n = books.size();
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;

        for (int i = 1; i <= n; ++i) {
            int width = 0, height = 0;
            for (int j = i; j > 0; --j) {
                width += books[j - 1][0];
                if (width > shelfWidth) break;
                height = max(height, books[j - 1][1]);
                dp[i] = min(dp[i], dp[j - 1] + height);
            }
        }

        return dp[n];
    }
};
