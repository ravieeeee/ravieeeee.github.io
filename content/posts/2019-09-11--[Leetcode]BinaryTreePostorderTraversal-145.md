---
title: "[Leetcode] Binary Tree Postorder Traversal"
date: "2019-09-11"
template: "post"
draft: false
slug: "/posts/leetcode-145/"
category: "Algorithm"
description: "Given a binary tree, return the postorder traversal of its nodes' values."
---

# 문제

Given a binary tree, return the postorder traversal of its nodes' values. [Leetcode](https://leetcode.com/problems/binary-tree-postorder-traversal/)

# 코드

```c++
struct TreeNode {
  int val;
  TreeNode *left;
  TreeNode *right;
  TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

void traverse(vector<int> &result, TreeNode* current) {
  if (current->left != NULL) traverse(result, current->left);
  if (current->right != NULL) traverse(result, current->right);
  result.push_back(current->val);
}

vector<int> postorderTraversal(TreeNode* root) {
  if (root == NULL) return vector<int>();

  vector<int> result;
  traverse(result, root);
  return result;
}
```

# 풀이

왼쪽 방문 -> 오른쪽 방문 -> 현재(root) 방문
