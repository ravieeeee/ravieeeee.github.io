---
title: "[Leetcode] Swap Nodes in Pairs"
date: "2019-09-04"
template: "post"
draft: false
slug: "/posts/leetcode-24/"
category: "Algorithm"
description: "Given a linked list, swap every two adjacent nodes and return its head."
---

# 문제

Given a linked list, swap every two adjacent nodes and return its head.

You may not modify the values in the list's nodes, only nodes itself may be changed. [Leetcode](https://leetcode.com/problems/swap-nodes-in-pairs/)

# 코드

```c++
#include <iostream>

using namespace std;

struct ListNode {
  int val;
  ListNode *next;
  ListNode(int x) : val(x), next(NULL) {}
};

ListNode* swapPairs(ListNode* head) {
  if (head == NULL) return NULL;
  ListNode* new_head = head->next;
  if (new_head == NULL) return head; 

  ListNode* cur_left = NULL;  
  ListNode* cur = head;
  ListNode* cur_right = head->next;
  
  // 홀수개일때 처리조건
  while (cur_right != NULL) {
    if (cur_left != NULL) {
      cur_left->next = cur_right;
    }
    cur->next = cur_right->next;
    cur_right->next = cur;

    if (cur->next != NULL) {
      cur_left = cur;
      cur = cur_left->next;
      cur_right = cur->next;
    } else {
      // 뒤에 남은게 없다면
      break;
    }
  }

  return new_head;
}
```

# 풀이

left, cur, right를 가리키는 포인터를 두고 left의 next를 right로, cur의 next를 right의 next로, right의 next를 cur로 바꾸는 작업을 계속해서 반복한다.
