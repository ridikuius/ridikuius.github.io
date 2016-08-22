---
layout: post
title: "leetcode_Valid Palindrome"
date: 2016-08-22
excerpt: "算法解析日记之leetcode_Valid Palindrome"
tags: [算法,leetcode]
comments: true
---
# Valid Palindrome

## Question
leetcode: [Valid Palindrome | LeetCode OJ](https://leetcode.com/problems/valid-palindrome/)

## Problem Statement

Given a string, determine if it is a palindrome,
considering only alphanumeric characters and ignoring cases.
{: .notice}

## Example

"A man, a plan, a canal: Panama" is a palindrome.

"race a car" is not a palindrome.
{: .notice}

## Clarification

Have you consider that the string might be empty?
This is a good question to ask during an interview.
For the purpose of this problem,
we define empty string as valid palindrome.
{: .notice}

## Challenge
O(n) time without extra memory.
{: .notice}

## 题解

## 代码实现

```java
 public boolean isPalindrome(String s) {

        if (s == null || s.equals("")) {
            return true;
        }
        int start = 0;
        int end = s.length() - 1;
        s = s.toLowerCase();
        while (start < end) {
            if (isChar(s, start)) {
                start++;
                continue;
            }
            if (isChar(s, end)) {
                end--;
                continue;
            }
            if (s.charAt(start) != s.charAt(end)) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }

    private boolean isChar(String s, int index) {
        char cur = s.charAt(index);
        return !(96 < cur && cur < 123) && !(47 < cur && cur < 58);
    }

```

## 源码分析

根据回文串的定义用两个指针从两头风别开始遍历，遇到非字母数字的字符跳过。
{: .notice}

## 复杂度分析

两根指针遍历一次，时间复杂度 `O(n)`, 空间复杂度 `O(1)`.
{: .notice}
