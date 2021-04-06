---
published: true
layout: post
date: '2020-08-04 14:50:00 -0000'
categories: algo
---
Binary Tree is a data structure extended from linked list data structure. It has the character of inconsecutive data storage , and indirect data access(unlike array's direct access). **It suits to traverse by recursion**. 


### Basic Knowledge

There are 3 ways of traversal. Most of the issues can be resolved using the 3 ways of traversal. 

Pre-order Traversal,

```java
public void traverse(TreeNode root) { 
    //root will reslove the smaller issue for one node.
    traverse(root.left);
    traverse(root.right);
}
```

In-order Traversal,

```java
public void traverse(TreeNode root) { 
    traverse(root.left);
    //root will reslove the smaller issue for one node.
    traverse(root.right);
}
```

Post-order Traversal,

```java
public void traverse(TreeNode root) { 
    traverse(root.left);
    traverse(root.right);
    //root will reslove the smaller issue for one node.
}
```

We can resolve a binary tree problem using traversal way or divide and conquer way.

###### Traversal: Do something for one node and all nodes on its subtrees need to do the same operation.

- Do what a node shall do.  明确一个节点要做的事
- Recursive call a node's subtrees. 递归调用所有子树
- The traversal way usually use with class property.

###### Divide and conquer: The final result will get from its left subtree's and right subtree's result.

- Recursive call the node's left and right tree. 递归调用左右子树
- Use left and right subtree returning result to calculate final result.  
- The divide and conquer way usually has a return type because it needs to use left and right subtree's result.

### Problem

######  [66. Binary Tree Preorder Traversal (Non-recursion Version)](https://www.lintcode.com/problem/binary-tree-preorder-traversal/note/224954)

Given a binary tree, return the preorder traversal of its nodes' values.

Example 1:

```
Input：{1,2,3}
Output：[1,2,3]
Explanation:
   1
  / \
 2   3
it will be serialized {1,2,3}
Preorder traversal
```

Analysis:

There are multiple solution to resolve this question. Here is [an article](https://blog.csdn.net/zhuiyisinian/article/details/107946790) about all methods. I will use a general way that can solve pre-order, in-order, post-order together.

First of all, we need to understand the recursion way to resolve the preorder traversal. In the recursion way, the method are loaded into stack and ready to run line by line. I will draw a stack diagram to demonstrate the details. As the picture see, we will push tree node 3, tree node 2 and add integer 1 to the stack. Then, we will pop integer 1 and save it to the result. Then we will do the same thing, push tree node 5, tree node 4, and integer 2 into the stack, and later pop the integer2.

As the picture described, each method (including its instructions and parameters) are saved in the stack. By iterative way, we will also use stack to save recursion's method's each parameter sets and do instructions iteratively. Inside each method,  we can use queue + while loop to run each instructions, or we can save the instructions reversely into a stack to avoid an extra loop. Here we will use this way. 

Because java can not save 2 types in one stack. We will create a customized object to bundle a tree node and marker(whether it is a value, or a node).

![](E:\study\jiuzhang\Notes\BinaryTreeRecursionStack.JPG)

Since stack is popping in the reversed order of pushing. Here we need to save the nodes in reversed visited order, which means for preorder, we will push the nodes as right, left, and node itself. Right and left are pushed as subtree, node itself will be pushed as wait for visited.



```java
public class Solution {
    /**
     * @param root: A Tree
     * @return: Preorder in ArrayList which contains node values.
     */
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        
        if(root == null) {
            return result;
        }
        Stack<Pair> stack = new Stack<>();
        
        stack.push(new Pair(root, false));
        
        while(!stack.isEmpty()) {
            
            Pair pair = stack.pop();
            
            if(!pair.isVisited) {
                
                if(pair.node.right != null) {
                    stack.push(new Pair(pair.node.right, false));
                }
                
                if(pair.node.left != null) {
                    stack.push(new Pair(pair.node.left, false));
                }
                
                stack.push(new Pair(pair.node, true));
            }else {
                result.add(pair.node.val);
            }
            
        }
        
        return result;
        
        
    }
}

class Pair {
    TreeNode node;
    boolean isVisited;
    
    Pair(TreeNode node, boolean isVisited) {
        this.node = node;
        this.isVisited = isVisited;
    }
}
```



###### [481. Binary Tree Leaf Sum](https://www.lintcode.com/problem/binary-tree-leaf-sum/description)

Description:

Given a binary tree, calculate the sum of leaves.

Example 1:

```
Input：{1,2,3,4}
Output：7
Explanation：
    1
   / \
  2   3
 /
4
3+4=7
```

Example 2:

```
Input：{1,#,3}
Output：3
Explanation：
    1
      \
       3
```

Analysis:

Here we can use both traversal and divide-conquer.

Traversal: 

Check a node, if it is a leaf node, add its value to the final sum. The left subtree and right subtree do the same operation. The traversal method needs to use a class property to remember the final result, so it has a side effect.

```java
public int sum;

public int leafSum(TreeNode root) {
    traverse(root);
    return sum;
}

private void traverse(TreeNode cur) {
    
    if(cur == null) {
        return;
    }
    
    if(cur.right == null && cur.left == null) {
        sum += cur.val;
    }
    
    traverse(cur.left);
    traverse(cur.right);
}
```

Divide-conquer： 

The binary tree's leaf sum equals the sum of its left subtree's leaf sum and right subtree's leaf sum. 

```java
    public int leafSum(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = leafSum(root.left);
        int right = leafSum(root.right);
                
        if(root.right == null && root.left == null) {
            return root.val;
        }
        return left + right;
    }
```

###### [482. Binary Tree Level Sum](https://www.lintcode.com/problem/binary-tree-level-sum/description)

Description:

Given a binary tree and an integer which is the depth of the target level.

Calculate the sum of the nodes in the target level.

Example 1:

```
Input：{1,2,3,4,5,6,7,#,#,8,#,#,#,#,9},2
Output：5 
Explanation：
     1
   /   \
  2     3
 / \   / \
4   5 6   7
   /       \
  8         9
2+3=5
```

Example 2:

```
Input：{1,2,3,4,5,6,7,#,#,8,#,#,#,#,9},3
Output：22
Explanation：
     1
   /   \
  2     3
 / \   / \
4   5 6   7
   /       \
  8         9
4+5+6+7=22
```

Analysis:

Traversal:

Check a node whether it is in the target level, if yes, we will add the node's value to the final sum result. Do the same operations for nodes on the left and right subtree.

```java
int sum;
public int levelSum(TreeNode root, int level) {
    traverse(root, level, 1);
    return sum;
}

private void traverse(TreeNode cur, int level, int height) {
    
    if(cur == null) {
        return;
    }
    
    if(level == height) {
        sum += cur.val;
        return;
    }
    
    traverse(cur.left, level, height + 1);
    traverse(cur.right, level, height + 1);
}
```

Divide-conquer:

以root为顶点的树到targeted level 的level sum = left-subtree's level sum +  right subtree's level sum.

```java
public int levelSum(TreeNode root, int level) {
    if(root == null) {
        return 0;
    }
    
    if(level == 1) {
        return root.val;
    }
    
    int left = levelSum(root.left, level - 1);
    int right = levelSum(root.right, level - 1);
    
    return left + right;
    
}
```

###### [480. Binary Tree Paths](https://www.lintcode.com/problem/binary-tree-paths/description)

Description:

Given a binary tree, return all root-to-leaf paths.

Example 1:

```
Input：{1,2,3,#,5}
Output：["1->2->5","1->3"]
Explanation：
   1
 /   \
2     3
 \
  5
```

Example 2:

```
Input：{1,2}
Output：["1->2"]
Explanation：
   1
 /   
2     
```

Analysis:

Traversal: A node will collect the path with its left child's value for left subtree, and the path with its right child's value for right subtree. Since the returning type is a `List<String>`, we can build the result through input parameter instead of using global variables.

*[5. Modifying input parameters during a method call](CleanCodePractice.md)*

Here we will only post the traversal traversal way since it is easier to think of.

```java
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> result = new ArrayList<>();
        
        if(root == null) {
            return result;
        }
        traverse(root, "" + root.val, result);
        
        return result;
    }
    
    private void traverse(TreeNode cur, String path, List<String> result) {
        
        if(cur.left == null && cur.right == null) {
            result.add(path);
            
            return;
        }
        
        
        if(cur.left != null) {
            traverse(cur.left, path + "->" + cur.left.val, result);

        }
        if(cur.right != null) {
            traverse(cur.right, path + "->" + cur.right.val, result);
        }

    }
```

Divide-conquer: 

Get all left subtree's paths, and all right subtree's paths. Add the cur value to all left subtree's paths, and right subtree's paths in the front of all paths iteratively. 

###### [469. Same Tree](https://www.lintcode.com/problem/same-tree/description)

Description:

Check if two binary trees are identical. Identical means the two binary trees have the same structure and every identical position has the same value.

Example 1:

```
Input:{1,2,2,4},{1,2,2,4}
Output:true
Explanation:
        1                   1
       / \                 / \
      2   2   and         2   2
     /                   /
    4                   4

are identical.
```

Example 2:

```
Input:{1,2,3,4},{1,2,3,#,4}
Output:false
Explanation:

        1                  1
       / \                / \
      2   3   and        2   3
     /                        \
    4                          4

are not identical.
```

Analysis:

Traversal:

Traverse all the nodes same time of tree a and tree b. Use a global variable to remember if they are the same or not.

Divide-conquer:

If left subtree and right subtree are identical, and the cur value from a and b are the same, we can return true. Otherwise, return false.

```java
    public boolean isIdentical(TreeNode a, TreeNode b) {
        
        if(a == null && b == null) {
            return true;
        }
        
        if(a == null && b != null || a != null && b == null) {
            return false;
        }
        
        boolean isLeftTreeIdentical = isIdentical(a.left, b.left);
        boolean isRightTreeIdentical = isIdentical(a.right, b.right);
        
        return isLeftTreeIdentical && isRightTreeIdentical && a.val == b.val;
    }
```

###### [175. Invert Binary Tree](https://www.lintcode.com/problem/invert-binary-tree/description)

Description:

Invert a binary tree.left and right subtrees exchange.

Example 1:

```
Input: {1,3,#}
Output: {1,#,3}
Explanation:
	  1    1
	 /  =>  \
	3        3
```

Example 2:

```
Input: {1,2,3,#,#,4}
Output: {1,3,2,#,4}
Explanation: 
	
      1         1
     / \       / \
    2   3  => 3   2
       /       \
      4         4
```

Analysis:

Traversal: this question doesn't need a return type, so we will use traversal directly. We will swap left and right sub trees, and call recursions.  

Even though after the swap the traverse order of left and right will change, it won't influence the  final result. The question only requires invert all left and sub trees.

```java
    public void invertBinaryTree(TreeNode root) {
        
        if(root == null) {
            return;
        }
        
        TreeNode temp = root.right;
        root.right = root.left;
        root.left = temp;
        invertBinaryTree(root.left);
        invertBinaryTree(root.right);
    }
```

###### [97. Maximum Depth of Binary Tree](https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description)

Description:

Given a binary tree, find its maximum depth.

The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

Example 1:

```
Input: tree = {}
Output: 0
Explanation: The height of empty tree is 0.
```

Example 2:

```
Input: tree = {1,2,3,#,#,4,5}
Output: 3	
Explanation: Like this:
   1
  / \                
 2   3                
    / \                
   4   5
it will be serialized {1,2,3,#,#,4,5}
```

Analysis:

Divide-conquer:

A tree's max depth is the max depth of its left subtree and right subtree + 1. 

```java
    public int maxDepth(TreeNode root) {
        if(root == null) {
            return 0;
        }
        
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        
        return Math.max(left, right) + 1;
    }
```

###### [93. Balanced Binary Tree](https://www.lintcode.com/problem/balanced-binary-tree/description)

Description:

Given a binary tree, determine if it is height-balanced.

For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

```
Example  1:
	Input: tree = {1,2,3}
	Output: true
	
	Explanation:
	This is a balanced binary tree.
		  1  
		 / \                
		2  3

	
Example  2:
	Input: tree = {3,9,20,#,#,15,7}
	Output: true
	
	Explanation:
	This is a balanced binary tree.
		  3  
		 / \                
		9  20                
		  /  \                
		 15   7 

	
Example  3:
	Input: tree = {1,#,2,3,4}
	Output: false
	
	Explanation:
	This is not a balanced tree. 
	The height of node 1's right sub-tree is 2 but left sub-tree is 0.
		  1  
		   \                
		   2                
		  /  \                
		 3   4
```

Analysis:

Since this question requires return value and the value type is a primary type. We will use divide-conquer method to resolve this question.

Divide-conquer:

Left subtree is balanced, right subtree is balanced, and their heights' differences are not greater than 1, then we can say the tree is a balanced tree. Therefore, we need to return both heights and whether the current tree is a balanced tree. **If we need to return 2 different primary types for one method call, we can create an object and save those 2 return results into object's properties.**

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: True if this Binary tree is Balanced, or false.
     */
     
    
    public boolean isBalanced(TreeNode root) {
        
        ResultType result = helper(root);
        return result.isBalanced;
    }
    
    private ResultType helper(TreeNode cur) {
    
        if(cur == null) {
            return new ResultType(0, true);
        }
        
        ResultType left = helper(cur.left);
        ResultType right = helper(cur.right);
        
        if(!left.isBalanced || !right.isBalanced || Math.abs(left.depth - right.depth) > 1) {
            return new ResultType(Math.max(left.depth, right.depth) + 1, false);
        }
        
        return new ResultType(Math.max(left.depth, right.depth) + 1, true);
        
    } 
    
    
    
}

class ResultType {
    int depth;
    boolean isBalanced;
    
    ResultType(int depth, boolean isBalanced) {
        
        this.depth = depth;
        this.isBalanced = isBalanced;
    }
}
```

###### [376. Binary Tree Path Sum](https://www.lintcode.com/problem/binary-tree-path-sum/description)

Description:

Given a binary tree, find all paths that sum of the nodes in the path equals to a given number `target`.

A valid path is from root node to any of the leaf nodes.

Example 1:

```plain
Input:
{1,2,4,2,3}
5
Output: [[1, 2, 2],[1, 4]]
Explanation:
The tree is look like this:
	     1
	    / \
	   2   4
	  / \
	 2   3
For sum = 5 , it is obviously 1 + 2 + 2 = 1 + 4 = 5
```



Example 2:

```plain
Input:
{1,2,4,2,3}
3
Output: []
Explanation:
The tree is look like this:
	     1
	    / \
	   2   4
	  / \
	 2   3
Notice we need to find all paths from root node to leaf nodes.
1 + 2 + 2 = 5, 1 + 2 + 3 = 6, 1 + 4 = 5 
There is no one satisfying it.
```

Analysis:

Traversal:

Calculate sum and collect paths from root to all nodes during traversal traversal. When we reach to leaves and the sum is exactly target, we will collect this path to the final result. 

This question includes a backtracking. Before we call the next level recursion, we will modify some properties. After the recursion return to the current level, we need to recover those modified properties.

```java
public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {
    List<List<Integer>> result = new ArrayList<>();
    
    if(root == null) {
        return result;
    }
    
    List<Integer> path = new ArrayList<>();
    path.add(root.val);
    
    traverse(root, target, path,  result, root.val);
    return result;
}

public void traverse(TreeNode cur, int target, List<Integer> path, List<List<Integer>> result, int sum) {
    
    if(cur == null) {
        return;
    }
    
    if(cur.left == null && cur.right == null && sum == target) {
        result.add(new ArrayList<>(path));
    }
    
    if(cur.left != null) {
        path.add(cur.left.val);
        traverse(cur.left, target, path, result, sum + cur.left.val);
        path.remove(path.size() - 1);
    }
    
    if(cur.right != null) {
        path.add(cur.right.val);
        traverse(cur.right, target, path, result, sum + cur.right.val);
        path.remove(path.size() - 1);
    }
}
```

Divide-conquer:

Find all paths in left subtree and right subtree their sum equals target - root's value. Collect all left and right subtree's result.

######  [246. Binary Tree Path Sum II](https://www.lintcode.com/problem/binary-tree-path-sum-ii/description)

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.

Example 1:

```plain
Input:
{1,2,3,4,#,2}
6
Output:
[[2, 4],[1, 3, 2]]
Explanation:
The binary tree is like this:
    1
   / \
  2   3
 /   /
4   2
for target 6, it is obvious 2 + 4 = 6 and 1 + 3 + 2 = 6.
```

Example 2:

```plain
Input:
{1,2,3,4}
10
Output:
[]
Explanation:
The binary tree is like this:
    1
   / \
  2   3
 /   
4   
for target 10, there is no way to reach it.
```



Analysis:

This question requires returning  a list of all paths. We can use both traversal and divide-conquer. However, divide-conquer needs to consider the question from result to requirements. Also, traversal won't introduce extra global variables because the result is an object already. Therefore, we will use traversal for this question.

We will build a path from root to leaf. For each node, we will iteratively calculate sum back up from each node to get sums not starting from root.

This question is actually a typical DFS question. DFS is actually a traversal for multiple-branch trees. In the requirements, we can see the question is looking for **all paths**. DFS is looping each element in the same level horizontally and call recursion for each node.  this question, horizontally, we only have 2 subtrees. We will enumerate them by left and right. Here we need to pay attention, this question's iteration is different from the DFS template iteration. 

Time complexity is O(n^2).

```java
public List<List<Integer>> binaryTreePathSum2(TreeNode root, int target) {
    List<List<Integer>> result = new ArrayList<>();
    helper(root, target, new ArrayList<>(), result);
    return result;
}

private void helper(TreeNode cur, int target, List<Integer> subset, List<List<Integer>> result) {
    
    if(cur == null) {
        return;
    }
    
    subset.add(cur.val);
    
    int subSum = 0;
    for(int i = subset.size() - 1; i >= 0; i--) {
        subSum += subset.get(i);
        if(subSum == target) {
            result.add(new ArrayList<Integer>(subset.subList(i, subset.size())));
        }    
    }
    
    helper(cur.left, target, subset, result);
    helper(cur.right, target, subset, result);
    subset.remove(subset.size() - 1);
}
```

######  

###### [124. Binary Tree Maximum Path Sum(leetcode)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

Given a **non-empty** binary tree, find the maximum path sum.

For this problem, a path is defined as any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The path must contain **at least one node** and does not need to go through the root.

```
Example 1:
Input: [-10,9,20,null,null,15,7]

   -10
   / \
  9  20
    /  \
   15   7

Output: 42
	
Example 2:
	Input:  For the following binary tree:

      1
     / \
    2   3
		
	Output: 6
```

Analysis: To solve this question, we shall know how to do the question [475. Binary Tree Maximum Path Sum II](#475. Binary Tree Maximum Path Sum II). The max path sum is the max value of left,right subtree and crossing root path sum. the max crossing root sum =(max of (with left subtree's top as root, max path sum) and 0) + max of (with right subtree's top as root, max path sum) and 0) + root's value.

```java
public class Solution {
    /**
     * @param root: The root of binary tree.
     * @return: An integer
     */
    public int maxPathSum(TreeNode root) {
        
        ResultType result = helper(root);
        return result.maxPathSum;
    }
    
    private ResultType helper(TreeNode cur) {
        
        if(cur == null) {
            return new ResultType(Integer.MIN_VALUE, 0);
        }
        
        ResultType left = helper(cur.left);
        ResultType right = helper(cur.right);
        
        int singlePathSum = Math.max(left.singlePathSum, right.singlePathSum) + cur.val;
        singlePathSum = Math.max(singlePathSum, 0);
        
        int maxPathSum = Math.max(left.maxPathSum, right.maxPathSum);
        maxPathSum = Math.max(maxPathSum, left.singlePathSum + cur.val + right.singlePathSum);
        
        return new ResultType(maxPathSum, singlePathSum);
    }
}

class ResultType {
    int maxPathSum;
    int singlePathSum;
    
    ResultType(int maxPathSum, int singlePathSum) {
        this.maxPathSum = maxPathSum;
        this.singlePathSum = singlePathSum;
    }
    
}
```

###### [475. Binary Tree Maximum Path Sum II](https://www.lintcode.com/problem/binary-tree-maximum-path-sum-ii/description)

Given a binary tree, find the maximum path sum from root.

The path may end at any node in the tree and contain at least one node in it.

Example 1:

```
Input: {1,2,3}
Output: 4
Explanation:
    1
   / \
  2   3
1+3=4
```

Example 2:

```
Input: {1,-1,-1}
Output: 1
Explanation:
    1
   / \
  -1 -1
```

Analysis: 

Divide-conquer:

We can get left subtree's max path sum and right subtree's max path sum. Compare the left subtree and right subtree. If the greater path sum is **> 0**, we will add the path sum to with the root value and return. Otherwise, we will return root value itself.

```java
    public int maxPathSum2(TreeNode root) {
        
        if(root == null) {
            return 0;
        }
        
        int left = maxPathSum2(root.left);
        int right = maxPathSum2(root.right);
        
        return Math.max(Math.max(0, left), right) + root.val;
    }
```

###### [596. Minimum Subtree](https://www.lintcode.com/problem/minimum-subtree/solution)

Description:

Given a binary tree, find the subtree with minimum sum. Return the root of the subtree.

Example 1:

```
Input:
{1,-5,2,1,2,-4,-5}
Output:1
Explanation:
The tree is look like this:
     1
   /   \
 -5     2
 / \   /  \
1   2 -4  -5 
The sum of whole tree is minimum, so return the root.
```

Example 2:

```
Input:
{1}
Output:1
Explanation:
The tree is look like this:
   1
There is one and only one subtree in the tree. So we return 1.
```

Notice:

LintCode will print the subtree which root is your return node.
It's guaranteed that there is only one subtree with minimum sum and the given binary tree is not an empty tree.

Analysis: Get sums for all subtrees. Compare each sum and maintain a minimum for all trees.

```java
public class Solution {
    private TreeNode subtree = null;
    private int subtreeSum = Integer.MAX_VALUE;
    /**
     * @param root the root of binary tree
     * @return the root of the minimum subtree
     */
    public TreeNode findSubtree(TreeNode root) {
        helper(root);
        return subtree;
    }
    
    private int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int sum = helper(root.left) + helper(root.right) + root.val;
        if (sum <= subtreeSum) {
            subtreeSum = sum;
            subtree = root;
        }
        return sum;
    }
}
```

Divide-conquer:

minimum sum of a subtree is the minimum of (left, right, and left + right + root value).

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */
     
    public TreeNode findSubtree(TreeNode root) {
        ResultType result = traverse(root);
        return result.minNode;
    }
    
    private ResultType traverse(TreeNode cur) {
        
        if(cur == null) {
            return new ResultType(0, Integer.MAX_VALUE, null);
        }
        
        ResultType left = traverse(cur.left);
        ResultType right = traverse(cur.right);
        
        int sum = left.sum + right.sum + cur.val;
        
        TreeNode minNode = cur;
        int min = sum;
        
        if(left.min < right.min && left.min < sum) {
            min = left.min;
            minNode = left.minNode;
        }else if(left.min > right.min && right.min < sum){
            min = right.min;
            minNode = right.minNode;
        }
        
        return new ResultType(sum, min, minNode);
    }
}

class ResultType {
    int sum;
    int min;
    TreeNode minNode;
    
    ResultType(int sum, int min, TreeNode minNode) {
        this.sum = sum;
        this.min = min;
        this.minNode = minNode;
    }
}
```

###### [597. Subtree with Maximum Average](https://www.lintcode.com/problem/subtree-with-maximum-average/solution)

Descriptions:

Given a binary tree, find the subtree with maximum average. Return the root of the subtree.

Example 1:

```
Input：
{1,-5,11,1,2,4,-2}
Output：11
Explanation:
The tree is look like this:
     1
   /   \
 -5     11
 / \   /  \
1   2 4    -2 
The average of subtree of 11 is 4.3333, is the maximun.
```

Example 2:

```
Input：
{1,-5,11}
Output：11
Explanation:
     1
   /   \
 -5     11
The average of subtree of 1,-5,11 is 2.333,-5,11. So the subtree of 11 is the maximun.
```

Notice:

 the subtree which root is your return node.
It's guaranteed that there is only one subtree with maximum average.

Analysis:

Get all subtree's sum and its size to calculate the subtree's average. Maintain a class property of max average. This question needs to pay attention when node is null. The max average doesn't need to be calculated, instead, the leaf value is the avg. 

We shall avoid using division as much as possible.

*[6. Smallest number of integer and double](CleanCodePractice.md)*

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the root of the maximum average of subtree
     */
    
    double maxAvg;
    TreeNode maxNode;
    
    public TreeNode findSubtree2(TreeNode root) {
        maxAvg = -Double.MIN_VALUE;
        maxNode = null;
        
        traverse(root);
        
        return maxNode;
        
    }
    
    private ResultType traverse(TreeNode cur) {
        
        if(cur == null) {
            
            return new ResultType(0, 0);
        }
        
        ResultType left = traverse(cur.left);
        ResultType right = traverse(cur.right);
        
        int sum = left.sum + right.sum + cur.val;
        int count = left.count + right.count + 1;
        
        double avg = ((double) sum) / count;
        if(avg > maxAvg || maxNode == null) {
            maxAvg = avg;
            maxNode = cur;
        }
        
        return new ResultType(sum, count);
        
                
    }
    
    
}

class ResultType{
    int sum;
    int count;
    
    ResultType(int sum, int count) {
        this.sum = sum;
        this.count = count;
    }
}
```

###### [595. Binary Tree Longest Consecutive Sequence](https://www.lintcode.com/problem/binary-tree-longest-consecutive-sequence/solution)

Descriptions:

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (`cannot be the reverse`).

Example 1:

```
Input:
   1
    \
     3
    / \
   2   4
        \
         5
Output:3
Explanation:
Longest consecutive sequence path is 3-4-5, so return 3.
```

Example 2:

```
Input:
   2
    \
     3
    / 
   2    
  / 
 1
Output:2
Explanation:
Longest consecutive sequence path is 2-3,not 3-2-1, so return 2.
```

Analysis:

A node's max consecutive length is the max of left subtree's consecutive length and right subtree's consecutive length + 1 or 1. We need to traverse all nodes to get the longest length.  

 

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the length of the longest consecutive sequence path
     */
     
    int longest;
    
    public int longestConsecutive(TreeNode root) {
        helper(root);
        return longest;
    }
    
    private int helper(TreeNode root) {
                
        if(root == null) {
            return 0;
        }
        
        int left = helper(root.left);
        int right = helper(root.right);
        
        int length = 1;
        
        if(root.left != null && root.val + 1 == root.left.val) {
            length = left + 1;
        }
        if(root.right != null && root.val + 1 == root.right.val) {
            length = Math.max(length, right + 1);
        }
        
        longest = Math.max(length, longest);
        
        return length;
    }
    
}
```



######  [88. Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-tree/description)

Description:

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

Example 1:

```
Input：{1},1,1
Output：1
Explanation：
 For the following binary tree（only one node）:
         1
 LCA(1,1) = 1
```

Example 2:

```
Input：{4,3,7,#,#,5,6},3,5
Output：4
Explanation：
 For the following binary tree:

      4
     / \
    3   7
       / \
      5   6
			
 LCA(3, 5) = 4
```

Notice:

Assume two nodes are exist in tree.

Analysis:

The ancestor is the first node that diverse 2 nodes into left and right sub tree. **If we find a node, whose left subtree contains one target node, and right subtree contains another target node, the node is the lowest common ancestor** However, there is a special case, which is,  one of the target node is another target node's child. For this case, we shall return the first target node we found during the top-down traverse.  

We shall compare from the root to the leaf to see weather any node equal the target node (A or B). If we find any target node, we shall return it. if a node's left subtree contains a node, right subtree contains another node, the node is the lowest common ancestor. If we find target A and B are in the same subtree, then the first node we find in the top-down direction is the lowest common ancestor. 

This question is a variation of finding a node from binary tree. Once we find a target, we will return that target directly from that node. If a node contains both targets on its left and right subtrees alone bottom-up returning direction, the node is the lowest common ancestor. We won't search the whole tree to the bottom, instead, we will return if we find any node A or B. If A or B is on the same side of tree, we need to find the topper one. Therefore, we have to return the first finding in the top-down direction, i.e., if we find a target, we shall return before we call the recursion.

```java
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {
        
        if(root == null) {
            return null;
        }
        
        if(root == A || root == B) {
            return root;
        }
        
        TreeNode left = lowestCommonAncestor(root.left, A, B);
        TreeNode right = lowestCommonAncestor(root.right, A, B);
        
        if(left != null && right != null) {
            return root;
        }
        
        if(left != null) {
            return left;
        }
        
        if(right != null) {
            return right;
        }
        return null;
        
        
    }
```

###### [474. Lowest Common Ancestor II](https://www.lintcode.com/problem/lowest-common-ancestor-ii/description)

Description:

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.

The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.

The node has an extra attribute `parent` which point to the father of itself. The root's parent is null.

Example 1:

```
Input：{4,3,7,#,#,5,6},3,5
Output：4
Explanation：
     4
     / \
    3   7
       / \
      5   6
LCA(3, 5) = 4
```

Example 2:

```
Input：{4,3,7,#,#,5,6},5,6
Output：7
Explanation：
      4
     / \
    3   7
       / \
      5   6
LCA(5, 6) = 7
```



Analysis:

This question is  [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) 's another solution. We can save both target nodes' parents into 2 lists. Then we find the first same parent which is the lowest common ancestor. Here we have a small trick to simplify the return condition. We can compare the 2 list from the end, and find the last same elements which are the result. If we compare from the beginning, the corner case, one node being another node's father, has to be considered as an extra.

```java
    public ParentTreeNode lowestCommonAncestorII(ParentTreeNode root, ParentTreeNode A, ParentTreeNode B) {
        
        ArrayList<ParentTreeNode> aPath = getRootPath(A);
        ArrayList<ParentTreeNode> bPath = getRootPath(B);
        
        
        int aIndex = aPath.size() - 1;
        int bIndex = bPath.size() - 1;
        
        ParentTreeNode result = null;
        
        while(aIndex >= 0 && bIndex >= 0) {
            
            if(aPath.get(aIndex) != bPath.get(bIndex)) {
                break;
            }
            
            result = aPath.get(aIndex);
            aIndex--;
            bIndex--;
        }
        
        return result;
        
    }
    
    private ArrayList<ParentTreeNode> getRootPath(ParentTreeNode node) {
        
        ArrayList<ParentTreeNode> result = new ArrayList<>();
        
        while(node != null) {
            result.add(node);
            node = node.parent;
            
        }
        
        return result;
    }
```

###### [578. Lowest Common Ancestor III](https://www.lintcode.com/problem/lowest-common-ancestor-iii/solution)

Description:

Given the root and two nodes in a Binary Tree. Find the lowest common ancestor(LCA) of the two nodes.
The lowest common ancestor is the node with largest depth which is the ancestor of both nodes.
Return `null` if LCA does not exist.

Example1

```
Input: 
{4, 3, 7, #, #, 5, 6}
3 5
5 6
6 7 
5 8
Output: 
4
7
7
null
Explanation:
  4
 / \
3   7
   / \
  5   6

LCA(3, 5) = 4
LCA(5, 6) = 7
LCA(6, 7) = 7
LCA(5, 8) = null
```

Example2

```
Input:
{1}
1 1
Output: 
1
Explanation:
The tree is just a node, whose value is 1.
```

Notice

node A or node B may not exist in tree.
Each node has a different value

Analysis: This question is question [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) + if we can find both nodes. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) 's solution has been posted. Here we need to make sure whether we can find both nodes in  bottom-up order. We need 2 `boolean` values in the returning result to remember whether we found target A or B. If left subtree contains A, or right subtree contains A, or root == A, A exists in the tree. Same for the target B. After we can find both targets, [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) 's returning is the answer. If any of them doesn't exist, we return null. The two recursion can be combined into one method. Besides we need to create a returning object, we need to pay attention the variables updating orders. Some variables has to be updated before recursion, some variables will update after recursion.

```java
public class Solution {
    /*
     * @param root: The root of the binary tree.
     * @param A: A TreeNode
     * @param B: A TreeNode
     * @return: Return the LCA of the two nodes.
     */
    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode A, TreeNode B) {
        
        ResultType result = helper(root, A, B);
        
        if(!result.foundA || !result.foundB) {
            return null;
        }
        
        return result.cur;
        
    }
    
    private ResultType helper(TreeNode root, TreeNode A, TreeNode B) {
        
        if(root == null) {
            return  new ResultType(false, false, root);
        }
        
        ResultType left = helper(root.left, A, B);
        ResultType right = helper(root.right, A, B);
        
        boolean foundA = left.foundA || right.foundA || root == A;
        boolean foundB = left.foundB || right.foundB || root == B;
        
        if(root == A || root == B) {
            return new ResultType(foundA, foundB, root);
        }
        if(left.cur != null && right.cur != null) {
            return new ResultType(foundA, foundB, root);
        }
        
        if(left.cur != null) {
            return new ResultType(foundA, foundB, left.cur);
        }
        if(right.cur != null) {
            return new ResultType(foundA, foundB, right.cur);
        }
        
        return new ResultType(foundA, foundB, null);
        
    }
}

class ResultType {
    boolean foundA;
    boolean foundB;
    TreeNode cur;
    
    ResultType(boolean foundA, boolean foundB, TreeNode cur) {
        this.foundA = foundA;
        this.foundB = foundB;
        this.cur = cur;
    }
}
```

###### [1360. Symmetric Tree](https://www.lintcode.com/problem/symmetric-tree/description)

Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).

Example1

```
Input: {1,2,2,3,4,4,3}
Output: true
Explanation:
    1
   / \
  2   2
 / \ / \
3  4 4  3
This binary tree {1,2,2,3,4,4,3} is symmetric
```

Example2

```
Input: {1,2,2,#,3,#,3}
Output: false
Explanation:
    1
   / \
  2   2
   \   \
   3    3
This is not a symmetric tree
```

Analysis:

If left and right subtree's value are equal, a left.right subtree and right.left subtree are symmetrical, and a left.left and right.right are symmetrical, the tree is symmetrical. The reuqirment doesn't need any info from root, but only left and right subtree. We will pass node 1 and node2 as parameter.

```java
public boolean isSymmetric(TreeNode root) {
  if(root == null) {
    return true;
  }

  return helper(root.left, root.right);

}


private boolean helper(TreeNode node1, TreeNode node2) {

  if(node1 == null && node2 == null) {
    return true;
  }

  if(node1 == null && node2 != null || node1 != null && node2 == null) {
    return false;
  }

  if(node1.val != node2.val) {
    return false;
  }

  boolean isLeft = helper(node1.right, node2.left); 
  boolean isRight = helper(node1.left, node2.right);

  return isLeft && isRight;
}
```



### Summery: 

- If an operation's requirement doesn't need any left or right subtree's info but only use root's info, we can use **traversal** directly. 
  - [481. Binary Tree Leaf Sum](# 481. Binary Tree Leaf Sum): We only need to check whether a node is a leaf and calculate the sum. The calculation doesn't need any info from left or right subtree.
- If an operation's requirement need all nodes info from left or right subtree, even though, we need all nodes on left or right subtree, we still need to run operation on all nodes. We still can use **traversal**.
  - [95. Validate Binary Search Tree](https://www.lintcode.com/problem/validate-binary-search-tree/description): One of the requirements is all nodes on left subtree must be less than the root and right subtree must be greater than root. Here we need to check whether each node is in a range using traversal.
- If an operation's requirment need left or right subtree's result, and result usually contains countable properties( most of them is 1) not a list, we will use **divide-conquer**.
  - [97. Maximum Depth of Binary Tree](#97. Maximum Depth of Binary Tree): Maximum depth is max of left subtree's maximum depth and right subtree's maximum depth + 1. Here we need to use left subtree's result, and right subtree's result. We will use divide-conquer.
- If an operration's requirment only need left or right subtree info but not root's info, we shall **pass 2 nodes into the method** and find rules with left and right subtree's children. It can use **traversal or divide-conquer**
