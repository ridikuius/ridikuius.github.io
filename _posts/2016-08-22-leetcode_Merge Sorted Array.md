---
layout: post
title: "leetcode_Merge Sorted Array"
date: 2016-08-22
excerpt: "算法解析日记之leetcode_Merge Sorted Array"
tags: [算法,leetcode]
comments: true
---
# Merge Sorted Array

## Question
leetcode: [Merge Sorted Array | LeetCode OJ](https://leetcode.com/problems/merge-sorted-array/)

## Problem Statement

Given two sorted integer arrays `nums1` and `nums2`, merge `nums2` into `nums1` as one sorted array.
{: .notice}


## Clarification

You may assume that `nums1` has enough space (size that is greater or equal to `m + n`) to hold additional elements from `nums2`. The number of elements initialized in `nums1` and `nums2` are m and n respectively.
{: .notice}

## 题解

## 代码实现

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
          if (nums1 == null || nums2 == null) {
            return;
        }
        int index = m + n - 1;
        while (m > 0 && n > 0) {
            if (nums1[m - 1] > nums2[n - 1]) {
                nums1[index] = nums1[m - 1];
                m--;
            } else {
                nums1[index] = nums2[n - 1];
                n--;
            }
            index--;
        }
        while (n > 0) {
            nums1[index] = nums2[n - 1];
            n--;
            index--;
        }
    }
```

## 源码分析

因为有 `in-place` 限制，故必须从数组末尾的两个元素开始比较；否则就会产生挪动，一旦挪动就会是 `O(n^2)` 的。 由于`nums1` 的容量较 `nums2` 大，故最后` m == 0` 或者` n == 0`时仅需处理` nums1` 中的元素，因为 `nums1 `中的元素已经在` nums1` 中，无需处理。
{: .notice}

## 复杂度分析

最坏情况下需要遍历两个数组中所有元素，时间复杂度为 `O(n)`. 空间复杂度` O(1)`.
{: .notice}
