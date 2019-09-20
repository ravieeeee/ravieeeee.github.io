---
title: "[Leetcode] Add Two Numbers"
date: "2019-09-09"
template: "post"
draft: false
slug: "/posts/leetcode-2/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "Add the two numbers and return it as a linked list."
---

# 문제

You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself. [Leetcode](https://leetcode.com/problems/add-two-numbers/)

# 코드

```c++
struct ListNode {
  int val;
  ListNode *next;
  ListNode(int x) : val(x), next(NULL) {}
};

ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
  ListNode* root = NULL;
  int before = 0;
  while (l1 != NULL || l2 != NULL || before != 0) {
    int val1 = l1 == NULL ? 0 : l1->val;
    int val2 = l2 == NULL ? 0 : l2->val;
    ListNode* node = new ListNode((val1 + val2 + before) % 10);
    before = (val1 + val2 + before) / 10;

    if (root == NULL) root = node;
    else {
      ListNode* cur_node = root;
      while (cur_node->next != NULL) cur_node = cur_node->next;
      cur_node->next = node;
    }

    if (l1 != NULL) l1 = l1->next;
    if (l2 != NULL) l2 = l2->next;
  }
  return root;
}
```

# 풀이

1. `new`로 생성하지 않으면 같은 메모리 주소를 재활용하는 걸 주의해야 한다.
2. `l1`, `l2`를 대응하는 수로 변환하려 했지만 long long의 범위도 넘어간다!
