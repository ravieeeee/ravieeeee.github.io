---
title: "[Leetcode] 2 Keys Keyboard"
date: "2019-09-21"
template: "post"
draft: false
slug: "/posts/leetcode-650/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "You have to get exactly n 'A' on the notepad by performing the minimum number of steps permitted."
---

# 문제

Initially on a notepad only one character 'A' is present. You can perform two operations on this notepad for each step:

1. Copy All: You can copy all the characters present on the notepad (partial copy is not allowed).
2. Paste: You can paste the characters which are copied last time.
 
Given a number n. You have to get exactly n 'A' on the notepad by performing the minimum number of steps permitted. Output the minimum number of steps to get n 'A'. [Leetcode](https://leetcode.com/problems/2-keys-keyboard/)

# 코드

```c++
int result = INT_MAX;

void get_steps(int cur, int pasted, int step, int n) {
  if (cur == n) {
    result = min(result, step);
    return;
  } else if (cur > n) return;

  // paste
  get_steps(cur + pasted, pasted, step + 1, n);
  // copy
  if (cur != pasted) get_steps(cur, cur, step + 1, n);
}

int minSteps(int n) {
  if (n == 1) return 0;
  
  get_steps(1, 1, 1, n);
  return result;
}
```

# 풀이

2가지 동작 중 선택해서 step을 늘려가면 된다. `cur`을 현재 개수, `pasted`를 복사된 개수라고 했을 때, `Paste`의 경우 `cur = cur + pasted`가 되며 `Copy All`의 경우 `pasted = copy`가 된다.

규칙을 찾아서 해결하는 풀이도 보인다.

```c++
int res = 0;
for (int i = 2; i <= n; i++){
  while (n % i == 0) {
    res += i;
    n /= i;
  }
}
return res;
```

81 -> `81 / 3 = 27` -> `27 / 3 = 9` -> `9 / 3 = 3` -> 3 이런 방식인 것 같다. [solution](https://leetcode.com/problems/2-keys-keyboard/discuss/105908/Very-Simple-Java-Solution-With-Detail-Explanation)
