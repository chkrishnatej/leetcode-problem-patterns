# Kth Largest Element in an Array

## Problem

{% embed url="https://leetcode.com/problems/kth-largest-element-in-an-array/description/" %}

## Intuition

Coming up with Heap approach is very easy and it takes `O(n log n)` time complexity. But in the question the added an extra constraint that it needs to be solved in `O(n)` time complexity.

The approach that needs to be taken to solve it in `O(n)` time complexity is by using **Quick Select** algorithm.

> I have favorited few posts in the **Solutions** tab to get an idea of it.
