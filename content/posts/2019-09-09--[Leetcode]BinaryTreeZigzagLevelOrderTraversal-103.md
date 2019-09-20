---
title: "[Leetcode] Binary Tree Zigzag Level Order Traversal"
date: "2019-09-09"
template: "post"
draft: false
slug: "/posts/leetcode-103/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "Given a binary tree, return the zigzag level order traversal of its nodes' values."
---

# 문제

Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between). [Leetcode](https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/)

# 코드

```c++
struct TreeNode {
  int val;
  TreeNode *left;
  TreeNode *right;
  TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

struct Node {
  TreeNode *node;
  int level;
  Node(TreeNode* n, int l): node(n), level(l) {}
};

vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
  if (root == NULL) return vector<vector<int>>();

  vector<vector<int>> result;
  queue<Node*> q;
  Node* tmp = new Node(root, 0);
  q.push(tmp);
  
  while (!q.empty()) {
    Node* n = q.front();
    q.pop();
    if (result.size() == n->level) {
      result.push_back(vector<int>{n->node->val});
    } else {
      result[n->level].push_back(n->node->val);
    }

    TreeNode* left = n->node->left;
    TreeNode* right = n->node->right;
    if (left != NULL) {
      Node* tmp = new Node(left, n->level + 1);
      q.push(tmp);
    }
    if (right != NULL) {
      Node* tmp = new Node(right, n->level + 1);
      q.push(tmp);
    }
  }
  
  for (int i = 0; i < result.size(); i++) { 
    if (i % 2 == 1) {
      reverse(result[i].begin(), result[i].end());
    }
  }

  return result;
}
```

# 풀이

그냥 BFS 후에 필요한 부분만 뒤집는다.
