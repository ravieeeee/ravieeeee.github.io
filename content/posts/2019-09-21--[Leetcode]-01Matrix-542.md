---
title: "[Leetcode] 01 Matrix"
date: "2019-09-21"
template: "post"
draft: false
slug: "/posts/leetcode-542/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "find the distance of the nearest 0 for each cell."
---

# 문제

Given a matrix consists of 0 and 1, find the distance of the nearest 0 for each cell.

The distance between two adjacent cells is 1. [Leetcode](https://leetcode.com/problems/01-matrix/)

# 코드

```c++
int dir_x[4] = {1, -1, 0, 0};
int dir_y[4] = {0, 0, 1, -1};
int row, col;

void bfs(vector<vector<int>>& matrix, vector<vector<int>>& result) {
  queue<pair<int, int>> q;
  for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
      if (matrix[i][j] == 0) q.push(make_pair(i, j));
    }
  }

  while (!q.empty()) {
    pair<int, int> node = q.front();
    q.pop();

    for (int i = 0; i < 4; i++) {
      int new_x = node.first + dir_x[i], new_y = node.second + dir_y[i];
      bool is_valid = new_x >= 0 && new_y >= 0 && new_x < row && new_y < col;

      if (is_valid && result[new_x][new_y] > result[node.first][node.second] + 1) {
        result[new_x][new_y] = result[node.first][node.second] + 1;
        q.push(make_pair(new_x, new_y));
      }
    }
  }
}

vector<vector<int>> updateMatrix(vector<vector<int>>& matrix) {
  row = matrix.size(), col = matrix[0].size();

  vector<vector<int>> result(row, vector<int>(col));

  for (int i = 0; i < row; i++) {
    for (int j = 0; j < col; j++) {
      result[i][j] = matrix[i][j] == 0 ? 0 : INT_MAX;
    }
  }
  bfs(matrix, result);

  return result;
}
```

# 풀이

0에 대해서 상하좌우에 위치한 값들을 업데이트 하는 방식이다. 먼저, 0으로 인해 업데이트가 일어나니까 0을 일단 큐에 전부 넣는다. 상하좌우 접한 지점의 거리를 최소값으로 업데이트 해간다. 새로 업데이트 된 셀로 인해서 또다시 주위에 영향을 끼칠 수 있으므로 큐에 업데이트된 셀을 넣는다.

*실패기 & 해결 방안*
1. 1인 점에 대해서 BFS -> 시간 초과  
각 1마다 BFS를 돌면서 가장 가까운 0을 만날때마다 위치값을 업데이트 하는 방식이다. 한번의 BFS로 한 위치값만 계산할 수 밖에 없어서 비효율적이다.

2. DP  
`result[i][j] = min(result[i - 1][j], result[i + 1][j], result[i][j - 1], result[i][j + 1])`이라 생각해서 구현했지만 이게 순서가 없다보니 재귀가 순환에 빠진다.

하지만 DP로 해결할 수 있는 방법이 있다.

```c++
// left and top
for (int i = 0; i < rows; i++) {
  for (int j = 0; j < cols; j++) {
    if (matrix[i][j] == 0) dist[i][j] = 0;
    else {
      if (i > 0) dist[i][j] = min(dist[i][j], dist[i - 1][j] + 1);
      if (j > 0) dist[i][j] = min(dist[i][j], dist[i][j - 1] + 1);
    }
  }
}

// bottom and right
for (int i = rows - 1; i >= 0; i--) {
  for (int j = cols - 1; j >= 0; j--) {
    if (i < rows - 1) dist[i][j] = min(dist[i][j], dist[i + 1][j] + 1);
    if (j < cols - 1) dist[i][j] = min(dist[i][j], dist[i][j + 1] + 1);
  }
}
```

![](/media/leetcode-542.png)

저 순서로 순회하게 된다면 각각 특정 점에서 왼쪽 위는 모두 최소 거리라고 보장할 수 있고, 마찬가지로 2번째 방향으로 순회하는 것 또한 특정 점에서 오른쪽 아래는 모두 최소 거리로 업데이트 되었다고 보장할 수 있다.
