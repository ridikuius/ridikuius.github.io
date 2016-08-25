---
layout: post
title: "leetcode_Word Ladder II"
date: 2016-08-25
excerpt: "算法解析日记之leetcode_Word Ladder II"
tags: [算法,leetcode]
comments: true
---
# Word Ladder II

## Question
leetcode: [Word Ladder II | LeetCode OJ](https://leetcode.com/problems/word-ladder-ii/)

## Problem Statement

Given two words (beginWord and endWord), and a dictionary's word list, find all shortest transformation sequence(s) from beginWord to endWord, such that:
1. Only one letter can be changed at a time
2. Each intermediate word must exist in the word list
{: .notice}

## Example

Given:
beginWord = `"hit"`
endWord = `"cog"`
wordList = `["hot","dot","dog","lot","log"]`
Return
```
  [
    ["hit","hot","dot","dog","cog"],
    ["hit","hot","lot","log","cog"]
  ]
```
{: .notice}

## Clarification

All words have the same length.
All words contain only lowercase alphabetic characters.
{: .notice}

## 题解

## 代码实现

```java
public List<List<String>> findLadders(String start, String end,
                                          Set<String> dict) {
        List<List<String>> res = new ArrayList<List<String>>();
        List<String> path = new ArrayList<String>();
        //当前词与可选择转换路径
        Map<String, List<String>> c_path = new HashMap<String, List<String>>();
        //当前词与之和start之间转换距离
        Map<String, Integer> distance = new HashMap<String, Integer>();

        dict.add(start);
        dict.add(end);

        bfs(c_path, distance, start, end, dict);

        dfs(res, path, end, start, distance, c_path);

        return res;
    }

    /**
     * 深度优先搜索算法
     */
    void dfs(List<List<String>> ladders, List<String> path, String cur,
             String start, Map<String, Integer> distance,
             Map<String, List<String>> map) {
        path.add(cur);
        if (cur.equals(start)) {
            Collections.reverse(path);
            ladders.add(new ArrayList<String>(path));
            Collections.reverse(path);
        } else {
            for (String next : map.get(cur)) {
                if (distance.containsKey(next) && distance.get(cur) == distance.get(next) + 1) {
                    dfs(ladders, path, next, start, distance, map);
                }
            }
        }
        path.remove(path.size() - 1);
    }

    /**
     * 广度优先搜索算法,获取每个词的可选路径和与start的转换次数
     */
    void bfs(Map<String, List<String>> map, Map<String, Integer> distance,
             String start, String end, Set<String> dict) {
        Queue<String> q = new LinkedList<String>();
        q.offer(start);
        distance.put(start, 0);
        for (String s : dict) {
            map.put(s, new ArrayList<String>());
        }

        while (!q.isEmpty()) {
            String cur = q.poll();

            List<String> c_path = next_path(cur, dict);
            for (String next : c_path) {
                map.get(next).add(cur);
                if (!distance.containsKey(next)) {
                    distance.put(next, distance.get(cur) + 1);
                    q.offer(next);
                }
            }
        }
    }

    /**
     * 返回每个词的可选择路径
     */
    List<String> next_path(String crt, Set<String> dict) {
        List<String> c_path = new ArrayList<String>();

        for (int i = 0; i < crt.length(); i++) {
            for (char ch = 'a'; ch <= 'z'; ch++) {
                if (ch != crt.charAt(i)) {
                    String next = crt.substring(0, i) + ch
                                  + crt.substring(i + 1);
                    if (dict.contains(next)) {
                        c_path.add(next);
                    }
                }
            }
        }

        return c_path;
    }

```

## 源码分析

先`BFS`生成找到`end`时的生成树，标记出每个单词所在的层数。然后从目标用`DFS`往回找。`BFS`爆搜大数据会超时。
{: .notice}
