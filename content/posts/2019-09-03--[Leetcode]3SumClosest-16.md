---
title: "[Leetcode] 3Sum Closest"
date: "2019-09-03"
template: "post"
draft: false
slug: "/posts/leetcode-16/"
category: "Algorithm"
description: "Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target."
---

# 문제

Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution. [Leetcode](https://leetcode.com/problems/3sum-closest/)

# 코드

```c++
#include <vector>
#include <iostream>
#include <algorithm>
#include <limits.h>

using namespace std;

int threeSumClosest(vector<int>& nums, int target) {
  if (nums.size() < 3) return 0;
  sort(nums.begin(), nums.end());

  int before = nums[0], result, dist = INT_MAX;
  for (int i = 0; i < nums.size() - 2; i++) { 
    int cur = nums[i];
    if (before == cur && i != 0) continue;
    int l = i + 1, r = nums.size() - 1;

    int value;
    while (l < r) {
      value = cur + nums[l] + nums[r];
      if (value == target) {
        return target;
      } else {
        int new_dist = value > target ? value - target : target - value;
        if (new_dist < dist) {
          result = value;
          dist = new_dist;
        }

        if (value < target) l++;
        else r--;
      }
    }
    before = cur;
  }
  return result;
}
```

# 풀이

i가 가리키는 것을 기준으로 두고, 왼쪽 포인터와 오른쪽 포인터를 이동해가며 sum과 주어진 target과의 거리를 비교해서 더 가까운 값으로 업데이트한다.
