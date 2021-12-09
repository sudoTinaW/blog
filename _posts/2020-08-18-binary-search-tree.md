---
published: true
layout: post
date: '2020-08-18 14:50:00 -0000'
categories: algo
---
Binary Search Tree is one of the commonly used binary tree. Its definition is that all nodes on the left subtree are <= the node, and all nodes on the right subtree are >= the node.

BST's value has orders. Therefore, most of its operations doesn't need to search both right and left subtree, and its template is a bit different from the general traverse.

The blog is written based on [labuladong](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/er-cha-sou-suo-shu-cao-zuo-ji-jin) git book.

```java
void BST(TreeNode root, int target) {
    if (root.val == target)
        // 找到目标，做点什么
    if (root.val < target) 
        BST(root.right, target);
    if (root.val > target)
        BST(root.left, target);
}
```

Also, BST's in-order traverse is sorted. It is commonly used with in-order traversal.

### Problems

#### [95. Validate Binary Search Tree](https://www.lintcode.com/problem/validate-binary-search-tree/description)

#### [1524. Search in a Binary Search Tree](https://www.lintcode.com/problem/search-in-a-binary-search-tree/description)


#### [11. Search Range in Binary Search Tree](https://www.lintcode.com/problem/search-range-in-binary-search-tree/)

####  [85. Insert Node in a Binary Search Tree](https://www.lintcode.com/problem/insert-node-in-a-binary-search-tree/description)

#### [(Leetcode) 1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

#### [87. Remove Node in Binary Search Tree](https://www.lintcode.com/problem/remove-node-in-binary-search-tree/solution)

#### [106. Convert Sorted List to Binary Search Tree](https://www.lintcode.com/problem/convert-sorted-list-to-binary-search-tree)

#### [448. Inorder Successor in BST](https://www.lintcode.com/problem/inorder-successor-in-bst/)

#### [(Leetcode) 897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/)

