---
layout: post
title: "leetcode_Longest Consecutive Sequence"
date: 2016-08-22
excerpt: "算法解析日记之leetcode_Longest Consecutive Sequence"
tags: [算法,leetcode]
comments: true
---
# Longest Consecutive Sequence

## Question
leetcode: [Longest Consecutive Sequence | LeetCode OJ](https://leetcode.com/problems/longest-consecutive-sequence/)

## Problem Statement

```
Given an unsorted array of integers, find the length of the longest consecutive elements sequence.
```

## Example

```
Given [100, 4, 200, 1, 3, 2], The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length: 4.
```

## Clarification

```
Your algorithm should run in O(n) complexity.
```

## 题解

## 代码实现

```java
public int longestConsecutive(int[] num) {
        HashSet<Integer> set = new HashSet<Integer>();
        for (int cur : num) {
            set.add(cur);
        }
        int length = 0;
        for (int cur : num) {
            int down = cur - 1;
            while (set.contains(down)) {
                set.remove(down);
                down--;
            }
            int up = cur + 1;
            while (set.contains(up)) {
                set.remove(up);
                up++;
            }
            length = Math.max(length, up - down - 1);
        }

        return length;
    }

```

## 源码分析

首先使用 HashSet 建哈希表，然后遍历数组，依次往左往右搜相邻数，搜到了就从 `Set` 中删除。末尾更新最大值。

## 复杂度分析

时间复杂度和空间复杂度均为 `O(n)`.

## Notices

**Watch out!** You can also add notices by appending  to a paragraph.
{: .notice}
