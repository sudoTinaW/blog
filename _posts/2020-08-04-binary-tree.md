---
published: true
layout: post
date: '2020-08-04 14:50:00 -0000'
categories: algo
---
Binary Tree is a data structure extended from linked list data structure. It has the character of inconsecutive data storage , and indirect data access(unlike array's direct access). **It suits to traverse by recursion**. 

When we traverse a binary tree, we will visit each node twice, once along the top-down direction, once along the bottom-up direction. The node is actually a pointer to access each element on the tree.

#### Top-down: 

- Along the top-down direction, we often run operations before subtrees' recursive call. The operations often save some data from ancestor's nodes and use it after subtrees' return when we visit the node again in the bottom-up direction. Those data is often saved as local variables. If we want to update any variable permanently during a top-down visit, we need to save the variable as an object's property or an element of a list. 
- The (local or global) variable will only be updated when it is satisfied with the question's requirements. We don't need to consider what needs to be updated to when the variable is not satisfied with the requirements. If the variable is not satisfied with the requirement, we can choose not to update it.

#### Bottom-up:

- Along the bottom-up direction, we often run operations after subtrees' recursive call. The operations are often used to get the final result. The result can be updated in two ways,
  - We can save the result in the return type. This way is actually as a conquer operation. Since binary tree naturally separates the problem into left and right subproblems, we can say this way as **divide and conquer**.
  - We also can save the result as an object's property or an element of a list, similarly to a top-down permanent variable. We use this way to collect the final result is usually because we can not get some intermediate result when we traverse along top-down direction. That is to say, we often apply this bottom-up traversal on intermediate result.
- The result has always to be updated using **divide and conquer** method , no matter whether the result qualified with the question's requirement or not. That is because every subtree needs to have a return result even when the result is not updated. Therefore, we need to consider what to return when the result doesn't need any update. We can think from situations,
  - Both left and right subtree has qualified result.
  - Left or right subtree has a qualified result, but the other subtree doesn't. Here we need to consider what needs to be returned when the result is not qualified with the requirement.
  - Neither left or right subtree has a qualified result. Here we need to consider what needs to be returned when the result is not qualified with the requirement.

#### When to use bottom-up and when to use top-down?

- If a question requires operations differently on left and right subtrees, we can only run those operations along the top-down direction. Only along the top-down direction we can tell a node is on a left or right subtree, but not along the bottom-up direction. 

- If the final result only needs **left or right subtree's result**, top-down or bottom-up will both work.

  ![img](/asset/binaryTreeSingalPath.png)

- If the final result needs to visit **left and right subtree at the same time**, we can use top-down method with **2 pointers**.

- If the final result needs both **left and right subtree's result**, bottom-up method is easier to solve the problem. For example, if we need to get all path sums of a tree, and the path needs to cross a node from the node's left side to its right side as showing in the below picture, bottom-up method can resolve the issue easily but top-down method will be hard.

  ![img](/asset/binaryTreeCrossingPath.png)

  

### Problem

####  [66. Binary Tree Preorder Traversal (Non-recursion Version)](https://www.lintcode.com/problem/binary-tree-preorder-traversal/note/224954)

#### [481. Binary Tree Leaf Sum](https://www.lintcode.com/problem/binary-tree-leaf-sum/description)

#### [482. Binary Tree Level Sum](https://www.lintcode.com/problem/binary-tree-level-sum/description)


#### [469. Same Tree](https://www.lintcode.com/problem/same-tree/description)

#### [175. Invert Binary Tree](https://www.lintcode.com/problem/invert-binary-tree/description)

#### [1360. Symmetric Tree](https://www.lintcode.com/problem/symmetric-tree/description)

#### [480. Binary Tree Paths](https://www.lintcode.com/problem/binary-tree-paths/description)

#### [376. Binary Tree Path Sum](https://www.lintcode.com/problem/binary-tree-path-sum/description)

####  [246. Binary Tree Path Sum II](https://www.lintcode.com/problem/binary-tree-path-sum-ii/description)

#### [97. Maximum Depth of Binary Tree](https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description)

#### [1181 Diameter of Binary Tree](https://www.lintcode.com/problem/1181/)

#### [475. Binary Tree Maximum Path Sum II](https://www.lintcode.com/problem/binary-tree-maximum-path-sum-ii/description)

#### [124. Binary Tree Maximum Path Sum(leetcode)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

#### [596. Minimum Subtree](https://www.lintcode.com/problem/minimum-subtree/solution)

#### [597. Subtree with Maximum Average](https://www.lintcode.com/problem/597/)

#### [595. Binary Tree Longest Consecutive Sequence](https://www.lintcode.com/problem/binary-tree-longest-consecutive-sequence/solution)

#### [93. Balanced Binary Tree](https://www.lintcode.com/problem/balanced-binary-tree/description)

####  [88. Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-tree/description)

#### [474. Lowest Common Ancestor II](https://www.lintcode.com/problem/lowest-common-ancestor-ii/description)

#### [578. Lowest Common Ancestor III](https://www.lintcode.com/problem/lowest-common-ancestor-iii/solution)

