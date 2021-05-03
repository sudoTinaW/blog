---
published: true
layout: post
date: '2020-08-04 14:50:00 -0000'
categories: algo
---
## Binary Tree

### Basic Knowledge

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

  ![img](binaryTreeSingalPath.png)

- If the final result needs to visit **left and right subtree at the same time**, we can use top-down method with **2 pointers**.

- If the final result needs both **left and right subtree's result**, bottom-up method is easier to solve the problem. For example, if we need to get all path sums of a tree, and the path needs to cross a node from the node's left side to its right side as showing in the below picture, bottom-up method can resolve the issue easily but top-down method will be hard.

  ![img](binaryTreeCrossingPath.png)

  

### Problem

####  [66. Binary Tree Preorder Traversal (Non-recursion Version)](https://www.lintcode.com/problem/binary-tree-preorder-traversal/note/224954)

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

![img](BinaryTreeRecursionStack.JPG)

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



#### [481. Binary Tree Leaf Sum](https://www.lintcode.com/problem/binary-tree-leaf-sum/description)

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

It is an easy problem. We can collect the sum as a class property and run the sum-up calculation along the top-down direction. 

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

We can also collect the sum in the return result and run sum-up calculation along the bottom-up direction. The sum is the total of right subtree's leaf sum and left subtree's leaf sum. This is the divide-conquer way.

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

#### [482. Binary Tree Level Sum](https://www.lintcode.com/problem/binary-tree-level-sum/description)

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

We can check a node whether it is in the target level and collect the satisfied level's sum as a class property along the top-down direction. 

```java
private int sum;
public int levelSum(TreeNode root, int level) {

  sum = 0;
  findSum(root, level);
  return sum;
}


public void findSum(TreeNode root, int level) {

  if(root == null) {
    return;
  }

  if(level == 1) {
    sum += root.val;
  }

  findSum(root.left, level - 1);
  findSum(root.right, level - 1);
}
```

Divide-conquer:

We can also collect the sum in the return result and run sum-up calculation along the bottom-up direction. The sum is the total of right subtree's targeted level's sum and left subtree's targeted level's sum. Here `level `variable is updated along the top-down direction, and  is for assisting to find the final result.

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


#### [469. Same Tree](https://www.lintcode.com/problem/same-tree/description)

Description:

Check if two binary trees are identical. Identical means the two binary trees have the same structure and every identical position has the same value.

Example 1:

```
Input:{1,2,2,4},{1,2,2,4}Output:trueExplanation:        1                   1       / \                 / \      2   2   and         2   2     /                   /    4                   4are identical.
```

Example 2:

```
Input:{1,2,3,4},{1,2,3,#,4}Output:falseExplanation:        1                  1       / \                / \      2   3   and        2   3     /                        \    4                          4are not identical.
```

Analysis:

We can compare each node from tree `a` and tree `b` at the same time when we traverse each node in the top-down direction, and use a global variable to remember if they are the same or not.

Divide-conquer:

We can also start compare each node from tree `a` and tree `b` when we return visit from bottom-up. If left subtree and right subtree are identical, and the cur value from a and b are the same, we can return true. Otherwise, return false.

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

#### [175. Invert Binary Tree](https://www.lintcode.com/problem/invert-binary-tree/description)

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

This question needs update left child node update to right child node, and right child node update to left child node. The left child and right child's operations are different. We will run updates along the top-down traversal.

Even though after the swap the traverse order of left and right will change, the code can still make sure traversing both left and right subtree only once. 

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

#### [1360. Symmetric Tree](https://www.lintcode.com/problem/symmetric-tree/description)

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

If a node's right subtree and left subtree are symmetric, the tree is symmetric. Normal top-down or bottom-up traversal will visit node one by one, which means we can not visit both left and right subtree at the same time. However, this question needs to visit left and right subtree at the same time so that we can compare nodes in some way to check whether they are symmetric. Therefore, we will use 2 pointers as input to let them traverse at the same time. 

One subtree needs to traverse int order of right and left, the other subtree needs to traverse in order of left and right. We compare each node on 2 different trees when we traverse along top-down direction, and save the comparing result in the return type. We will conquer the return result along the bottom-up direction.

If left and right subtree's value are equal, left.right subtree and right.left subtree are symmetrical, and left.left and right.right subtree are symmetrical, the tree is symmetrical. 

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

#### [480. Binary Tree Paths](https://www.lintcode.com/problem/binary-tree-paths/description)

Description:

Given a binary tree, return all root-to-leaf paths.

Example 1:

```
Input：{1,2,3,#,5}Output：["1->2->5","1->3"]Explanation：   1 /   \2     3 \  5
```

Example 2:

```
Input：{1,2}Output：["1->2"]Explanation：   1 /   2     
```

Analysis:

We can collect all nodes along the top-down directional visit and collect the path as the node reaches to the leaf. 

This question has 2 points that need to pay attention,

- when we traverse along top-down direction and aim to visit all nodes, we will return after visiting `null` node, not after leaf node.  If we want to return after leaf node, we need pay attention to the node who only has one child when we call its subtrees. 
- Any variable who is not a property of a list or an element of a list, its update will only exit in the the scope of its level's call. After it returns, the update will disappear. For example, `path` variable will be updated when we visit the current node, and becomes its parent value after the call ends. 

*[5. Modifying input parameters during a method call](CleanCodePractice.md)*

```java
public List<String> binaryTreePaths(TreeNode root) {  List<String> result = new ArrayList<>();  findPath(root, "", result);  return result;}private void findPath(TreeNode cur, String path, List<String> result) {  if(cur == null) {    return;  }  path = path + "->" + cur.val;  if(cur.left == null && cur.right == null) {    result.add(path.substring(2, path.length()));  }  findPath(cur.left, path, result);  findPath(cur.right, path, result);}
```

Divide-conquer:

We can collect all nodes along the bottom-up return visit. We can add the cur value to all left subtree's paths, and right subtree's paths in the front of all paths iteratively. 

#### [376. Binary Tree Path Sum](https://www.lintcode.com/problem/binary-tree-path-sum/description)

Description:

Given a binary tree, find all paths that sum of the nodes in the path equals to a given number `target`.

A valid path is from root node to any of the leaf nodes.

Example 1:

```plain
Input:{1,2,4,2,3}5Output: [[1, 2, 2],[1, 4]]Explanation:The tree is look like this:	     1	    / \	   2   4	  / \	 2   3For sum = 5 , it is obviously 1 + 2 + 2 = 1 + 4 = 5
```



Example 2:

```plain
Input:{1,2,4,2,3}3Output: []Explanation:The tree is look like this:	     1	    / \	   2   4	  / \	 2   3Notice we need to find all paths from root node to leaf nodes.1 + 2 + 2 = 5, 1 + 2 + 3 = 6, 1 + 4 = 5 There is no one satisfying it.
```

Analysis:

This question is almost the same as [480. Binary Tree Paths](https://www.lintcode.com/problem/binary-tree-paths/description). Here we need an extra variable to collect the sum of all paths. We will only return paths' sum equal to target.

This question has one thing different from the [480. Binary Tree Paths](https://www.lintcode.com/problem/binary-tree-paths/description) .We will collect path by list. Before we call recursions, we will update path list. It won't revert its value after the recursion call. We need to manually revert its value back.

```java
public List<List<Integer>> binaryTreePathSum(TreeNode root, int target) {  List<List<Integer>> result = new ArrayList<>();  if(root == null) {    return result;  }  getPathSum(root, target, new ArrayList<>(), result, 0);  return result;}private void getPathSum(TreeNode cur, int target, List<Integer> path, List<List<Integer>> result, int sum) {  if(cur == null) {    return;  }  path.add(cur.val);  sum += cur.val;  if(cur.left == null && cur.right == null && sum == target) {    result.add(new ArrayList<>(path));  }  getPathSum(cur.left, target, path, result, sum);  getPathSum(cur.right, target, path, result, sum);  path.remove(path.size() - 1);}
```

####  [246. Binary Tree Path Sum II](https://www.lintcode.com/problem/binary-tree-path-sum-ii/description)

Your are given a binary tree in which each node contains a value. Design an algorithm to get all paths which sum to a given value. The path does not need to start or end at the root or a leaf, but it must go in a straight line down.

Example 1:

```plain
Input:{1,2,3,4,#,2}6Output:[[2, 4],[1, 3, 2]]Explanation:The binary tree is like this:    1   / \  2   3 /   /4   2for target 6, it is obvious 2 + 4 = 6 and 1 + 3 + 2 = 6.
```

Example 2:

```plain
Input:{1,2,3,4}10Output:[]Explanation:The binary tree is like this:    1   / \  2   3 /   4   for target 10, there is no way to reach it.
```

Analysis:

This question requires to get all combinations of paths going downwards from parent to child. We can build paths either along top-bottom, or bottom-up directions. We can calculate all combinations as following situations,

![img](BinaryTreePathSumII.png)

Here I will post top-down solution only,

```java
public class Solution {    /*     * @param root: the root of binary tree     * @param target: An integer     * @return: all valid paths     */    public List<List<Integer>> binaryTreePathSum2(TreeNode root, int target) {        List<List<Integer>> result = new ArrayList<>();        if(root == null) {            return result;        }        traverseAllNodes(root, target, result);        return result;    }        private void traverseAllNodes(TreeNode cur, int target, List<List<Integer>> result) {        if(cur == null) {            return;        }        getAllPathSumFromOneNode(cur, target, new ArrayList<>(), result, 0);        traverseAllNodes(cur.left, target, result);        traverseAllNodes(cur.right, target, result);    }    private void getAllPathSumFromOneNode(TreeNode cur, int target, ArrayList<Integer> path, List<List<Integer>> result, int sum) {        if(cur == null) {            return;        }        path.add(cur.val);        sum += cur.val;        if(sum == target) {            result.add(new ArrayList<>(path));        }        getAllPathSumFromOneNode(cur.left, target, path, result, sum);        getAllPathSumFromOneNode(cur.right, target, path, result, sum);        path.remove(path.size() - 1);    }}
```

#### [97. Maximum Depth of Binary Tree](https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description)

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

We can get the max depth only after we visit the bottom. Therefore, we will collect result along bottom-up direction.A tree's max depth is the max depth of its left subtree and right subtree + 1. 

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

#### [1181 Diameter of Binary Tree](https://www.lintcode.com/problem/1181/)

Description:

Given a binary tree, you need to compute the length of the diameter of the tree. The diameter of a binary tree is the length of the longest path between any two nodes in a tree.

The length of path between two nodes is represented by the number of edges between them.

Example 1:

```
Given a binary tree 
          1
         / \
        2   3
       / \     
      4   5    
Return 3, which is the length of the path [4,2,1,3] or [5,2,1,3].
```

Example 2:

```
Input:[2,3,#,1]
Output:2

Explanation:
      2
    /
   3
 /
1
```

Analysis:

A subtree's longest path shall be the left subtree's max height plus right subtree's max height plus 1. If we can compare all subtree's longest path, we can get the longest path of a tree. We can get a tree's max height by [97. Maximum Depth of Binary Tree](https://www.lintcode.com/problem/maximum-depth-of-binary-tree/description)

```java
public class Solution {    /**     * @param root: a root of binary tree     * @return: return a integer     */    private int longest;    public int diameterOfBinaryTree(TreeNode root) {        longest = 0;        maxHeight(root);        return longest - 1;    }    private int maxHeight(TreeNode cur) {        if(cur == null) {            return 0;        }        int left = maxHeight(cur.left);        int right = maxHeight(cur.right);        longest = Math.max(left + right + 1, longest);        return Math.max(left, right) + 1;            }
```

#### [475. Binary Tree Maximum Path Sum II](https://www.lintcode.com/problem/binary-tree-maximum-path-sum-ii/description)

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

Since this question requires collecting paths starting from root, It is easy to collect the sum along top-down traversal.

```java
public class Solution {
    /**
     * @param root: the root of binary tree.
     * @return: An integer
     */

    private int max;
    public int maxPathSum2(TreeNode root) {
        max = root.val;

        traverse(root, 0);

        return max;
    }

    private void traverse(TreeNode root, int sum) {
        
        if(root == null) {
            return;
        }

        sum = sum + root.val;
        max = Math.max(sum, max);

        traverse(root.left, sum);
        traverse(root.right, sum);
    }
}
```

Even we requires collection paths starting from root, we still can use bottom-up traversal. Here we need to notice that left and right subtree might not always return qualified answer. We need to find what to return when the result is not qualified with requirement. When a subtree's returned sum is less than 0, it is not a qualified result. Since we are getting sum of a path, we will return 0 as the disqualified subtree's result. The value 0 won't influence the final result. Therefore, a max path sum will be the max path sum of (left subtree, right subtree, and 0), plus the node's current value.

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



#### [124. Binary Tree Maximum Path Sum(leetcode)](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

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

Analysis: 

This question can be split to 2 different sub problems. One is to get the max path sum along parent-child downwards direction. The path doesn't need to start from root, neither end with leaf. The other sub problem is to get the max sum of path crossing from a node's left and right subtree. We can solve the first sub problem by either top-down or bottom-up method, because the max sum is either on the left subtree **or **right subtree. Meanwhile, the second problem is better to use bottom-up method to resolve since we need both max sums of left **and** right subtree. 

Here is the bottom-up method and the time complexity is `O(n)`. The max path sum is the `max(max(right subtree's max path sum, 0) + parent node  max(left subtree's max path sum, 0) + parent node, and max(right subtree's max path sum, 0) + parent node + max(left subtree's max path sum, 0))`

```java
class Solution {
    private int maxPathSum;
    
    public int maxPathSum(TreeNode root) {
        
        maxPathSum = Integer.MIN_VALUE;
        
        getMaxPathSumOfSingalPath(root);
        
        return maxPathSum;
    }
    
    private int getMaxPathSumOfSingalPath(TreeNode cur) {
        
        if(cur == null) {
            return 0;
        }
        
        int left = getMaxPathSumOfSingalPath(cur.left);
        int right = getMaxPathSumOfSingalPath(cur.right);
        
        if(left < 0) {
            left = 0;
        }
        if(right < 0) {
            right = 0;
        }
        
        int maxCrossingCurSum = cur.val + left + right;
        int maxSingalCurSum = Math.max(left + cur.val, right + cur.val);
        
        int maxCurSum = Math.max(maxCrossingCurSum, maxSingalCurSum);
        
        maxPathSum = Math.max(maxPathSum, maxCurSum);
        
        return maxSingalCurSum;
    }
}
```

#### [596. Minimum Subtree](https://www.lintcode.com/problem/minimum-subtree/solution)

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

Analysis: to get a tree's sum, we need both left and right subtree's sum result. Therefore, we can build subtree's sum from bottom, while we get the min subtree during the back traverse of the whole tree.

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the root of the minimum subtree
     */

    private TreeNode subtree = null;
    private int minSum = Integer.MAX_VALUE;

    public TreeNode findSubtree(TreeNode root) {
        getSum(root);
        return subtree;
    }
    
    private int getSum(TreeNode root) {
        if (root == null) {
            return 0;
        }
        
        int sum = getSum(root.left) + getSum(root.right) + root.val;
        
        if (sum <= minSum) {
            minSum = sum;
            subtree = root;
        }
        return sum;
    }
}
```

#### [597. Subtree with Maximum Average](https://www.lintcode.com/problem/597/)

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

This question needs both left and right subtree's sum and number of nodes to calculate the whole tree's average. Therefore, we will calculate the average for each subtree along bottom-up direction. We will use a class property to get the max average of all subtrees during the backwards traverse.

*[6. Smallest number of integer and double](CleanCodePractice.md)*

```java
public class Solution {
    /**
     * @param root: the root of binary tree
     * @return: the root of the maximum average of subtree
     */

    private TreeNode maxNode;
    private double maxAvg;

    public TreeNode findSubtree2(TreeNode root) {
        
        maxAvg = -Double.MAX_VALUE;
        System.out.println();
        maxNode = null;

        getSumAndCount(root);
        return maxNode;
    }

    private ResultType getSumAndCount(TreeNode cur) {
        if(cur == null) {
            return new ResultType(0, 0);
        }

        ResultType left = getSumAndCount(cur.left);
        ResultType right = getSumAndCount(cur.right);

        int sum = left.sum + right.sum + cur.val;
        int count = left.count + right.count + 1;

        double curAvg = ((double)sum / count);
        

        if(maxAvg < curAvg) {
            maxAvg = curAvg;
            maxNode = cur;
        }

        return new ResultType(sum, count);
    }
}

class ResultType {
    int sum;
    int count;

    ResultType(int sum, int count) {
        this.sum = sum;
        this.count = count;
    }
}
```

#### [595. Binary Tree Longest Consecutive Sequence](https://www.lintcode.com/problem/binary-tree-longest-consecutive-sequence/solution)

Descriptions:

Given a binary tree, find the length of the longest consecutive sequence path.

The path refers to any sequence of nodes from some starting node to any node in the tree along the parent-child connections. The longest consecutive path need to be from parent to child (`cannot be the reverse`).

Example 1:

```
Input:   1    \     3    / \   2   4        \         5Output:3Explanation:Longest consecutive sequence path is 3-4-5, so return 3.
```

Example 2:

```
Input:   2    \     3    /    2      /  1Output:2Explanation:Longest consecutive sequence path is 2-3,not 3-2-1, so return 2.
```

Analysis:

A node's consecutive length is either on the left or right subtree. Therefore, we can resolve the issue along top-down or bottom-up direction. Here we will post top-down direction.

 Top-down: If a node's subtree's value is consecutive, we will pass the consecutive length to subtrees. Otherwise, the consecutive length will be 1. 

```java
public class Solution {    /**     * @param root: the root of binary tree     * @return: the length of the longest consecutive sequence path     */    private int longestLen;    public int longestConsecutive(TreeNode root) {                longestLen = 0;        traverse(root, 1);        return longestLen;    }    private void traverse(TreeNode cur, int consecutiveLen) {        if(cur == null) {            return;        }        int curConsecutiveLen = 1;        if((cur.left != null && cur.val + 1 == cur.left.val) || (cur.right != null && cur.val + 1 == cur.right.val)) {            curConsecutiveLen = consecutiveLen + 1;        }        longestLen = Math.max(longestLen, curConsecutiveLen);               traverse(cur.left, curConsecutiveLen);        traverse(cur.right, curConsecutiveLen);    }}
```

#### [93. Balanced Binary Tree](https://www.lintcode.com/problem/balanced-binary-tree/description)

Description:

Given a binary tree, determine if it is height-balanced.For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.

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

This question requires both left and right subtrees' results for each node to judge whether a subtree is balanced or not. We will use bottom-up method.

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

####  [88. Lowest Common Ancestor of a Binary Tree](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-tree/description)

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

The ancestor is the first node that diverse 2 nodes into left and right sub tree. **If we find a node, whose left subtree contains one target node, and right subtree contains another target node, the node is a common ancestor. If search from bottom to top, the first met common ancestor is the lowest common ancestor** 

If we need to find node A from one subtree and B from the other subtree, which is finding result from both left and right subtrees, we shall visit nodes along bottom-up direction. What is more, if we want to find the lowest common ancestor, we shall collect the result along bottom-up direction as well.

The returned result has following situation,

- The current node satisfied requirements, if current node is A or B, they are their own ancestor
- The current node not satisfied requirements, current node is neither A nor B, we need to continue to check its left subtree and right subtree's result.
- The left subtree and right subtree both satisfied requirements, the current node is the common ancestor
- Only the left subtree satisfied requirements, the left subtree lowest common ancestor will be its former(lower) result
- Only the right subtree satisfied requirements, the right subtree lowest common ancestor will be its former(lower) result
- Neither left nor right subtree satisfied requirements, `Null` will be the result

```java
public TreeNode lowestCommonAncestor(TreeNode root, TreeNode A, TreeNode B) {

  TreeNode lowestCommonAncestor = findLowestAncestor(root, A, B);
  return lowestCommonAncestor;
}

private TreeNode findLowestAncestor(TreeNode cur, TreeNode A, TreeNode B) {

  if(cur == null) {
    return null;
  }

  TreeNode left = findLowestAncestor(cur.left, A, B);
  TreeNode right = findLowestAncestor(cur.right, A, B);

  if(cur == A || cur == B) {
    return cur;
  }

  if(left != null && right != null) {
    return cur;
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

#### [474. Lowest Common Ancestor II](https://www.lintcode.com/problem/lowest-common-ancestor-ii/description)

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

#### [578. Lowest Common Ancestor III](https://www.lintcode.com/problem/lowest-common-ancestor-iii/solution)

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

Analysis: This question is question [88. Lowest Common Ancestor of a Binary Tree](#88. Lowest Common Ancestor of a Binary Tree) + if we can find both nodes. Since we need to find both nodes, we will need extra two boolean variables to save whether we foundA or foundB.

```java
/**
 * Definition of TreeNode:
 * public class TreeNode {
 *     public int val;
 *     public TreeNode left, right;
 *     public TreeNode(int val) {
 *         this.val = val;
 *         this.left = this.right = null;
 *     }
 * }
 */


public class Solution {
    /*
     * @param root: The root of the binary tree.
     * @param A: A TreeNode
     * @param B: A TreeNode
     * @return: Return the LCA of the two nodes.
     */
    public TreeNode lowestCommonAncestor3(TreeNode root, TreeNode A, TreeNode B) {
        
        ResultType result = findAorB(root, A, B);

        if(result.foundA && result.foundB) {
            return findAncestor(root, A, B);
        }

        return null;

    }

    private ResultType findAorB(TreeNode cur, TreeNode A, TreeNode B) {

        if(cur == null) {
            return new ResultType(false, false);
        }


        ResultType left = findAorB(cur.left, A, B);
        ResultType right = findAorB(cur.right, A, B);

        boolean foundA = left.foundA || right.foundA || cur == A;
        boolean foundB = left.foundB || right.foundB || cur == B;

        return new ResultType(foundA, foundB);



    }

    private TreeNode findAncestor(TreeNode cur, TreeNode A, TreeNode B) {

        if(cur == null) {
            return null;
        }

        TreeNode left = findAncestor(cur.left, A, B);
        TreeNode right = findAncestor(cur.right, A, B);
        
      	if(cur == A || cur == B) {
            return cur;
        }
        
      	if(left != null && right != null) {
            return cur;
        }

        if(left != null) {
            return left;
        }

        if(right != null) {
            return right;
        }

        return null;
    }

}

class ResultType{
    boolean foundA;
    boolean foundB;

    ResultType(boolean foundA, boolean foundB) {
        this.foundA = foundA;
        this.foundB = foundB;
    }
}
```



Here is the combination version of the two functions.

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

