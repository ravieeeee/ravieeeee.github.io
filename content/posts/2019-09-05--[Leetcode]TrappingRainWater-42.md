---
title: "[Leetcode] Trapping Rain Water"
date: "2019-09-05"
template: "post"
draft: false
slug: "/posts/leetcode-42/"
category: "Algorithm"
description: "compute how much water it is able to trap after raining."
---

# 문제

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining. [Leetcode](https://leetcode.com/problems/trapping-rain-water/)

# 코드

```c++
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

int get_size(vector<int>& height, int idx) {
  int left_max_value = -1, left_max_idx = -1;
  for (int i = idx - 1; i >= 0; i--) {
    if (height[i] > height[idx] && height[i] > left_max_value) {
      left_max_value = height[i];
      left_max_idx = i;
    }
  }
  if (left_max_idx == -1) return 0;

  int right_max_value = -1, right_max_idx = -1;
  for (int i = idx + 1; i < height.size(); i++) { 
    if (height[i] > height[idx] && height[i] > right_max_value) {
      right_max_value = height[i];
      right_max_idx = i;
    }
  }
  if (right_max_idx == -1) return 0;

  int max_height = min(left_max_value, right_max_value);
  return max_height - height[idx];
}

int trap(vector<int>& height) {
  int result = 0;
  for (int i = 1; i < height.size(); i++) {
    result += get_size(height, i);
  }
  return result;
}
```

# 풀이

각각의 위치에서 왼쪽과 오른쪽에 대하여 현재 위치보다 높으면서 가장 높은 값을 찾는다. 왼쪽의 가장 높은 값과 오른쪽의 가장 높은 값 중 작은 값만큼 물이 채워지게 된다.
