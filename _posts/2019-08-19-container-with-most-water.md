---
layout: post
title: "11. Container With Most Water"
date:   2019-08-19
categories: [Algorithm]
icon: icon-cplusplus
---

# 문제
Given n non-negative integers a1, a2, ..., an , where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2. [leetcode](https://leetcode.com/problems/container-with-most-water/)

![](https://s3-lc-upload.s3.amazonaws.com/uploads/2018/07/17/question_11.jpg)

<br>

# 코드
```c++
#include "solution.h"
#include <iostream>

using namespace std;

int maxArea(vector<int> &height) {
    int result = 0;

    for (int i = 0; i < height.size() - 1; i++) {
        for (int j = i + 1; j < height.size(); j++) {
            int min_value = min(height[i], height[j]);
            if (min_value * (j - i) > result) {
                result = min_value * (j - i);
            }
        }
    }

    return result;
}

int maxArea2(vector<int> &height) {
    int result = 0, l = 0, r = height.size() - 1;

    while (l < r) {
        result = max(result, min(height[l], height[r]) * (r - l));

        if (height[l] < height[r]) {
            l++;
        } else {
            r--;
        }
    }

    return result;
}
```

<br>

# 풀이
.
