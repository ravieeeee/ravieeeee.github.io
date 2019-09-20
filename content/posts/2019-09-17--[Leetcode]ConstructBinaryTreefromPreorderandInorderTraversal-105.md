---
title: "[Leetcode] Construct Binary Tree from Preorder and Inorder Traversal"
date: "2019-09-17"
template: "post"
draft: false
slug: "/posts/leetcode-105/"
category: "Algorithm"
tags:
  - "Leetcode"
description: "Given preorder and inorder traversal of a tree, construct the binary tree."
---

# 문제

Given preorder and inorder traversal of a tree, construct the binary tree. [Leetcode](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

# 코드

```c++
struct TreeNode {
  int val;
  TreeNode *left;
  TreeNode *right;
  TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
  if (preorder.size() == 0) return NULL;

  TreeNode* root = new TreeNode(preorder[0]);
  int root_idx;
  for (int i = 0; i < inorder.size(); i++) {
    if (inorder[i] == root->val) {
      root_idx = i;
      break;
    }
  }

  if (root_idx != 0) {
    vector<int> new_preorder(preorder.begin() + 1, preorder.begin() + root_idx + 1);
    vector<int> new_inorder(inorder.begin(), inorder.begin() + root_idx);
    root->left = buildTree(new_preorder, new_inorder);
  }

  if (root_idx != inorder.size() - 1) {
    vector<int> new_preorder(preorder.begin() + 1 + root_idx, preorder.end());
    vector<int> new_inorder(inorder.begin() + root_idx + 1, inorder.end());
    root->right = buildTree(new_preorder, new_inorder);
  }

  return root;
}
```

# 풀이

전위 순회에서 맨 첫번째 값은 무조건 루트다. 루트 값을 알 수 있으니 중위 순회한 결과에서 루트를 찾는다면 그 값을 기준으로 0번째 값부터 왼쪽은 루트의 왼쪽이 되고, 오른쪽 부터 끝까지는 루트의 오른쪽이 된다. 전위 순회값에서도 이를 이용하여 중위 순회에서 찾은 왼쪽을 이루는 것들의 개수만큼을 왼쪽 범위로 넘겨주고 나머지는 오른쪽으로 넘겨준다.
