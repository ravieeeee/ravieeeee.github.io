---
title: "[Leetcode] Unique Paths"
date: "2019-09-09"
template: "post"
draft: false
slug: "/posts/leetcode-62/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "How many possible unique paths are there?"
---

# 문제

A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).

The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).

How many possible unique paths are there? [Leetcode](https://leetcode.com/problems/unique-paths/)

# 코드

```c++
int uniquePaths(int m, int n) {
  vector<vector<long long>> v(n, vector<long long>(m, -1));
  if (m == 0 && n == 0) return 0;
  if (m == 1 && n == 1) return 1;
  
  v[0][0] = 0;
  if (m != 1) v[0][1] = 1;
  if (n != 1) v[1][0] = 1;

  for (int i = 0; i < n; i++) {
    for (int j = 0; j < m; j++) {
      if (v[i][j] == -1) {
        long long val = 0;
        if (i - 1 >= 0) val += v[i - 1][j];
        if (j - 1 >= 0) val += v[i][j - 1];
        v[i][j] = val;
      }
    }
  }
  return v[n - 1][m - 1];
}
```

# 풀이

현재값 = 왼쪽에서 오는 값(v[i][j - 1]) + 위에서 오는 값(v[i - 1][j])

## 메모리를 줄일 수 있는 방법

1. 결국 필요한 건 이전 row와 현재 row임

```c++
int pre[n - 1], cur[n - 1];
...
cur[j] = pre[j] + cur[j - 1];
swap(pre, cur);
```

2. pre는 결국 cur로 대체할 수 있음(이전 row를 날리지 않고 cur에 쌓아가면 됨)

```c++
int cur[n - 1];
...
cur[j] += cur[j - 1];
```
