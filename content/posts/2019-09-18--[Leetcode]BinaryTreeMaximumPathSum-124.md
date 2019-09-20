---
title: "[Leetcode] Binary Tree Maximum Path Sum"
date: "2019-09-18"
template: "post"
draft: false
slug: "/posts/leetcode-124/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "Given a non-empty binary tree, find the maximum path sum."
---

# 문제

Given a non-empty binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain at least one node and does not need to go through the root. [Leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

# 코드

```c++
struct TreeNode {
  int val;
  TreeNode *left;
  TreeNode *right;
  TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

int max_val = INT_MIN;

int get_sum(TreeNode* root) {
  if (root->left == NULL && root->right == NULL) return root->val;
  
  int left_val = root->left == NULL ? 0 : get_sum(root->left);
  left_val = max(0, left_val);
  int right_val = root->right == NULL ? 0 : get_sum(root->right);
  right_val = max(0, right_val);

  return root->val + max(left_val, right_val);
}

void pass(TreeNode* root) {
  int left_max = root->left == NULL ? 0 : get_sum(root->left);
  left_max = max(0, left_max);
  int right_max = root->right == NULL ? 0 : get_sum(root->right);
  right_max = max(0, right_max);

  max_val = max(max_val, root->val + left_max + right_max);

  if (root->left != NULL) pass(root->left);
  if (root->right != NULL) pass(root->right);
}

int maxPathSum(TreeNode* root) {
  pass(root);
  return max_val;
}
```

# 풀이

`pass`에 넘어온 root를 기준점으로 해서, 기준점에 대해서 왼쪽의 최대 경로 합의 값과 오른쪽의 최대 경로 합의 값과 기준점의 값을 더한다. 모든 `TreeNode`를 `pass`에 기준점으로 넘겨줘서 최대값을 찾기 위해 모두 검사한다.  
경로의 합은 `get_sum`에서 구하는데, 넘어온 기준점에서 왼쪽과 오른쪽 중 최대값인 부분만 선택하여 기준점의 값과 더한다.
