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

# Solution 1: DP

각 위치마다 왼쪽의 높은 값과 오른쪽의 높은 값을 찾는 것을 메모이제이션 할 수 있다.

```c++
#include <iostream>
#include <vector>

using namespace std;

int trap(vector<int>& height) {
	if (height == null) return 0;

  int ans = 0;
  int size = height.size();
  vector<int> left_max(size), right_max(size);

  left_max[0] = height[0];
  for (int i = 1; i < size; i++) {
    left_max[i] = max(height[i], left_max[i - 1]);
  }

  right_max[size - 1] = height[size - 1];
  for (int i = size - 2; i >= 0; i--) {
    right_max[i] = max(height[i], right_max[i + 1]);
  }

  for (int i = 1; i < size - 1; i++) {
    ans += min(left_max[i], right_max[i]) - height[i];
  }
  return ans;
}
```

# Solution 2: stack

```c++
#include <iostream>
#include <vector>
#include <stack>

using namespace std;

int trap(vector<int>& height) {
  int ans = 0, current = 0;
  stack<int> st;
  while (current < height.size()) {
    while (!st.empty() && height[current] > height[st.top()]) {
      int top = st.top();
      st.pop();
      if (st.empty()) break;
      int distance = current - st.top() - 1;
      int bounded_height = min(height[current], height[st.top()]) - height[top];
      ans += distance * bounded_height;
    }
    st.push(current++);
  }
  return ans;
}
```

# Solution 3: 2 pointers

어차피 right > left인 경우 left에 의해서 높이가 결정되고 left > right인 경우 right에 의해 높이가 결정되므로 오른쪽에 높은게 있으면 왼쪽 포인터를 이동시키고 왼쪽에 높은게 있으면 오른쪽 포인터를 이동시킨다.

```c++
#include <iostream>
#include <vector>

using namespace std;

int trap(vector<int>& height) {
  int left = 0, right = height.size() - 1;
  int ans = 0;
  int left_max = 0, right_max = 0;
  
  while (left < right) {
    if (height[left] < height[right]) {
      height[left] >= left_max ? (left_max = height[left]) : ans += (left_max - height[left]);
      ++left;
    }
    else {
      height[right] >= right_max ? (right_max = height[right]) : ans += (right_max - height[right]);
      --right;
    }
  }
  return ans;
}
```
