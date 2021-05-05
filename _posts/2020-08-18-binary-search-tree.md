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

#### [95. Validate Binary Search Tree](https://www.lintcode.com/problem/validate-binary-search-tree/description)

Description:

Given a binary tree, determine if it is a valid binary search tree (BST).

Assume a BST is defined as follows:

- The left subtree of a node contains only nodes with keys **less than** the node's key.
- The right subtree of a node contains only nodes with keys **greater than** the node's key.
- Both the left and right subtrees must also be binary search trees.
- A single node tree is a BST

Example 1:

```
Input:  {-1}
Output：true
Explanation：
For the following binary tree（only one node）:
	      -1
This is a binary search tree.
```

Example 2:

```
Input:  {2,1,4,#,#,3,5}
Output: true
For the following binary tree:
	  2
	 / \
	1   4
	   / \
	  3   5
This is a binary search tree.
```

Analysis:

Each node shall be greater than left subtree's right most node and less than right subtree's left most node. Therefore, we update different boundaries of left and right subtree, we need to update the boundaries along-top-down traversal. As showing in the picture, we can see the right boundary will be updated when we visit a node's left subtree, and the left boundary will be updated when we visit a node's right subtree. Since we need both right subtree and left subtree valid, we shall collect the result bottom-up.This question can not use the above template to choose only one subtree to traverse.

![img](./validateBST.jpg)



```java
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MAX_VALUE, Long.MIN_VALUE);
    }

    private boolean isValidBST(TreeNode cur, long max, long min) {

        if(cur == null) {
            return true;
        }

        boolean left = isValidBST(cur.left, cur.val, min);
        boolean right = isValidBST(cur.right, max, cur.val);

        if(cur.val <= min || cur.val >= max) {
            return false;
        }

        return left && right;
    }
```

At the beginning we don't know the root range. Root range can be any integer number, even the biggest integer. Therefore, we set the root range bigger than possible. This is not the best way. If we only allow integer type (same data range as the result), we shall set the root as `NULL`.  Setting Root range as `NULL`  makes more sense, because root's true range is not between biggest and smallest number but unknown. Here I will post another version using `NULL` as the root range.

```java
public boolean isValidBST(TreeNode root) {
  return isValidBST(root, null, null);
}

private boolean isValidBST(TreeNode cur, Integer max, Integer min) {

  if(cur == null) {
    return true;
  }

  boolean left = isValidBST(cur.left, cur.val, min);
  boolean right = isValidBST(cur.right, max, cur.val);

  if((min != null && cur.val <= min) || (max != null && cur.val >= max)) {
    return false;
  }

  return left && right;
}
```

#### [1524. Search in a Binary Search Tree](https://www.lintcode.com/problem/search-in-a-binary-search-tree/description)

Description:

Given the root of a binary search tree (BST) and a `value`.

Return the node whose value equals the given `value`. If such node doesn't exist, return `null`.

Example 1:

```
Input: value = 2
        4
       / \
      2   7
     / \
    1   3
Output: node 2
```

Example 2:

```
Input: value = 5
        4
       / \
      2   7
     / \
    1   3
Output: null
```

Analysis:

It is the template. if we traverse to the bottom and there isn't such node, we will return null. Otherwise, it will return directly without going to the bottom.

```java
public TreeNode searchBST(TreeNode root, int val) {
    
    if(root == null) {
        return null;
    }
    
    if(root.val == val) {
        return root;
    }
    
    if(root.val > val) {
        return searchBST(root.left, val);
    }
    
    if(root.val < val) {
        return searchBST(root.right, val);
    }
    
    return null;
    
}
```


#### [11. Search Range in Binary Search Tree](https://www.lintcode.com/problem/search-range-in-binary-search-tree/)

Given a binary search tree and a range `[k1, k2]`, return node values within a given range in ascending order.

Example 1:

```
Input：{5},6,10
Output：[]
        5
it will be serialized {5}
No number between 6 and 10
```

Example 2:

```
Input：{20,8,22,4,12},10,22
Output：[12,20,22]
Explanation：
        20
       /  \
      8   22
     / \
    4   12
it will be serialized {20,8,22,4,12}
[12,20,22] between 10 and 22
```

Analysis:

This question can be split into 2 questions. First, we need to search the first parent node which is in the range of `[k1, k2]`. Second, we need to in-order traverse all nodes as the child of that parent node. The first question we can use template to find the node. After we find the first parent node, we can use in-order traversal to collect the all its children.

```java
public List<Integer> searchRange(TreeNode root, int k1, int k2) {

  List<Integer> result = new ArrayList<>();

  traverse(root, k1, k2, result);

  return result;
}

private void traverse(TreeNode cur, int k1, int k2, List<Integer> result) {

  if(cur == null) {
    return;
  }

  if(cur.val > k2) {
    traverse(cur.left, k1, k2, result);
    return;
  }

  if(cur.val < k1) {
    traverse(cur.right, k1, k2, result);
    return;
  }

  traverse(cur.left, k1, k2, result);
  result.add(cur.val);
  traverse(cur.right, k1, k2, result);
}

```

####  [85. Insert Node in a Binary Search Tree](https://www.lintcode.com/problem/insert-node-in-a-binary-search-tree/description)

Description:

Given a binary search tree and a new tree node, insert the node into the tree. You should keep the tree still be a valid binary search tree.

```
Example 1:
	Input:  tree = {}, node = 1
	Output:  1
	
	Explanation:
	Insert node 1 into the empty tree, so there is only one node on the tree.

Example 2:
	Input: tree = {2,1,4,3}, node = 6
	Output: {2,1,4,3,6}
	
	Explanation: 
	Like this:



	  2             2
	 / \           / \
	1   4   -->   1   4
	   /             / \ 
	  3             3   6
		
```

Analysis:

Insert a node we need to modify the node's parent's node. We can check whether each node's child's position is the proper inserting point along top-down traversal. **We will always add new nodes at the bottom of Binary Search Tree.** Therefore, the insertion point must be a `null` position.

```java
public TreeNode insertIntoBST(TreeNode root, int val) {

  if(root == null) {
    return new TreeNode(val);
  }

  insert(root, val);
  return root;

}

private void insert(TreeNode cur, int val) {

  if(cur.left == null && val <= cur.val) {
    cur.left = new TreeNode(val);
    return;
  }

  if(cur.right == null && val > cur.val) {
    cur.right = new TreeNode(val);
    return;
  }

  if(val > cur.val) {
    insert(cur.right, val);
  }

  if(val <= cur.val) {
    insert(cur.left, val);
  }

}
```

 We can also remember the direct child in a result and link each direct child with its parent(current) node along bottom-up return. This way we will reconnect each parent with one of its direct child except for the new node. 

```java
    public TreeNode insertNode(TreeNode root, TreeNode node) {
        
        if(root == null) {
            return node;
        }

        if(root.val > node.val) {
            TreeNode left = insertNode(root.left, node);
            root.left = left;

        }else {
            TreeNode right = insertNode(root.right, node);
            root.right = right;
        }
        
        return root;
        
    }
    
```

#### [(Leetcode) 1008. Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

Description:

Return the root node of a binary **search** tree that matches the given `preorder` traversal.

*(Recall that a binary search tree is a binary tree where for every node, any descendant of `node.left` has a value `<` `node.val`, and any descendant of `node.right` has a value `>` `node.val`. Also recall that a preorder traversal displays the value of the `node` first, then traverses `node.left`, then traverses `node.right`.)*

It's guaranteed that for the given test cases there is always possible to find a binary search tree with the given requirements.

Example 1:

```
Input: [8,5,1,7,10,12]
Output: [8,5,10,1,7,null,12]
```

Constraints:

- `1 <= preorder.length <= 100`
- `1 <= preorder[i] <= 10^8`
- The values of `preorder` are distinct.

Analysis:

This questions is a variation of [85. Insert Node in a Binary Search Tree](#85. Insert Node in a Binary Search Tree) . We need to loop the node list + insert each node into BST.

```java
public TreeNode bstFromPreorder(int[] preorder) {
    
    if(preorder == null || preorder.length == 0) {
        return null;
    }
    
    TreeNode root = new TreeNode(preorder[0]);
    
    for(int i = 1; i < preorder.length; i++) {
        TreeNode newNode = new TreeNode(preorder[i]);
        insert(root, newNode);
    }
    
    return root;
}

private TreeNode insert(TreeNode cur, TreeNode newNode) {
    
    if(cur == null) {
        return newNode;
    }
    
    if(newNode.val < cur.val) {
        cur.left = insert(cur.left, newNode);
    }else if(newNode.val > cur.val) {
        cur.right = insert(cur.right, newNode);
    }
    
    return cur;
}
```

#### [87. Remove Node in Binary Search Tree](https://www.lintcode.com/problem/remove-node-in-binary-search-tree/solution)

Description:

Given a root of Binary Search Tree with unique value for each node. Remove the node with given value. If there is no such a node with given value in the binary search tree, do nothing. You should keep the tree still a binary search tree after removal.

Example 1

```plain
Input: 
Tree = {5,3,6,2,4}
k = 3
Output: {5,2,6,#,4} or {5,4,6,2}
Explanation:
Given binary search tree:
    5
   / \
  3   6
 / \
2   4
Remove 3, you can either return:
    5
   / \
  2   6
   \
    4
or
    5
   / \
  4   6
 /
2
```

Example 2

```plain
Input: 
Tree = {5,3,6,2,4}
k = 4
Output: {5,3,6,2}
Explanation:
Given binary search tree:
    5
   / \
  3   6
 / \
2   4
Remove 4, you should return:
    5
   / \
  3   6
 /
2
```

Analysis:

First, we can use the template to find the node that we are going to delete. Since we need to delete the node, we need to modify the node's parent. We can still use two ways find the removing node's parent and modify its link or saving the removed child in the return result and reconnect all parents with its direct child. Here we will use the second way.

When we find the removing node, we will delete the node in the following situations,

- If the node doesn't have any child, set the node to null and return it.

- If the node has only one child, set the node equals to the only child, and only child becomes null.

- If the node has 2 children,

  - Find the right sub tree min node.

  - exchange the value in the min node and the current node. 

  - delete the min node.

    Note: Usually, we don't exchange the value inside the node. Instead we will exchange the node. Because the value can be very big chuck of data, the exchange will take a lot of time. Exchange node reference is usually faster. However, if we want to change the reference here, we need to get each deleted node's parent node. It complicates the main thought of the problem. We will not use it here.

```java
    public TreeNode removeNode(TreeNode root, int value) {
        
        if(root == null) {
            return null;
        }
        
        if(root.val == value) {
            
            if(root.left == null) {
                return root.right;
            }
            
            if(root.right == null) {
                return root.left;
            }
            
            if(root.left != null && root.right != null) {
                
                TreeNode node = getMin(root.right);
                root.val = node.val;
                root.right = removeNode(root.right, node.val);
                
            }
            
        } else if(root.val < value) {
            root.right = removeNode(root.right, value);
        } else if(root.val >= value) {
            root.left = removeNode(root.left, value);
        }
        
        return root;
        
    }
    
    
    private TreeNode getMin(TreeNode cur) {
        
        while(cur.left != null) {
            cur = cur.left;
        }
        
        return cur;
        
    }
```



#### [106. Convert Sorted List to Binary Search Tree](https://www.lintcode.com/problem/convert-sorted-list-to-binary-search-tree)

Description:

Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.

```
Example 1:
	Input: array = 1->2->3
	Output:
		 2  
		/ \
	 1   3
		
Example 2:
	Input: 2->3->6->7
	Output:
		 3
		/ \
	 2   6
	      \
         7

	Explanation:
	There may be multi answers, and you could return any of them.
```

 Analysis:

This question can be resolved by 2 ways. 

First, we can split the list from middle, the left side of middle point will be left subtree, and the right side of the middle point will be right subtree. Such way, we build BST from top to bottom. Time complexity will be O(n^2). It takes each node O(n) time to find the middle point of a linked list, and there are n nodes in total.

Second, if we want to sequential access the linked list, we need to create the left subtree, the parent node, and right subtree in order. To get a balanced BST, we will use half elements to build the left subtree, and the other half - 1 to build the right subtree. Therefore, we will use the length of subtree to mark the boundary of each subtree. The linked list pointer shall move whenever we create a new node, and we will only create a new node when we create a new parent tree node. The new parent tree node can only be accessed after we build the left subtree. Therefore, the linked list node will be used after left subtree's call. The pointer can be saved in the left subtree's result or it can be saved as a global variable. Here we will save it as a global variable, since right subtree needs to share the return result and it doesn't need this variable as a result.

The below picture will show how we divide an array every time in 3 parts,

![img](ConvertSortListToBST.png)

```java
public class Solution {
    /*
     * @param head: The first node of linked list.
     * @return: a tree node
     */
     
    private ListNode cur;
    
    public TreeNode sortedListToBST(ListNode head) {
        int length = getLength(head);
        cur = head;
        TreeNode root = build(length);
        return root;
        
    }
    
    private TreeNode build(int length) {
        
        if(length == 0) {
            return null;
        }
      
        TreeNode left = build(length / 2);
        TreeNode root = new TreeNode(cur.val);
        cur = cur.next;
        TreeNode right = build(length - length / 2 - 1);
        
        root.left = left;
        root.right = right;
        
        return root;
    }
    
    private int getLength(ListNode head) {
        
        int size = 0;
        while(head != null) {
            head = head.next;
            size++;
        }
        return size;
    }
    
}
```



#### [448. Inorder Successor in BST](https://www.lintcode.com/problem/inorder-successor-in-bst/)

Description:

Given a binary search tree ([See Definition](http://www.lintcode.com/problem/validate-binary-search-tree/)) and a node in it, find the in-order successor of that node in the BST.

If the given node has no in-order successor in the tree, return `null`.

Example 1:

```
Input: {1,#,2}, node with value 1
Output: 2
Explanation:
  1
   \
    2
```

Example 2:

```
Input: {2,1,3}, node with value 1
Output: 2
Explanation: 
    2
   / \
  1   3
```

Challenge:

O(h), where h is the height of the BST.

Notice:

It's guaranteed *p* is one node in the given tree. (You can directly compare the memory address to find p)

Analysis:

If the node p doesn't have a right subtree, the successor will be its first left turn parent. If the node has a right subtree, the successor will be right subtree's left most node. Here we will draw those 3 conditions. 

- From the picture, we can see, the stop condition is the pointer becomes null. When we traverse the tree, we either traverse left subtree or right subtree, not both. That is to say, when we traverse left subtree, we don't need to save the right subtree's operation, which will run after left subtree.  Therefore, we can use loop easily, instead of recursion. 
- From the picture, we can see, the successor is the parent of last left child (left child may be null). Therefore, we need to save the parent node every time when we move to the left subtree. 
- One thing we need to pay attention, we are not stopping the loop even when we find the target p. We will continue to move to its right.

![img](InorderSuccessorInBST.png)

```java
    
    public TreeNode inorderSuccessor(TreeNode root, TreeNode p) {
        
        TreeNode next = null;
        while(root != null) {
            
            if(root.val <= p.val) {
                root = root.right;
            }else {
                next = root;
                root = root.left;
            }
        
        }
        
        return next;
        
    }
```

#### [(Leetcode) 897. Increasing Order Search Tree](https://leetcode.com/problems/increasing-order-search-tree/)

Description:

Given the `root` of a binary search tree, rearrange the tree in **in-order** so that the leftmost node in the tree is now the root of the tree, and every node has no left child and only one right child.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2020/11/17/ex1.jpg)

Constraints:

- The number of nodes in the given tree will be in the range `[1, 100]`.
- `0 <= Node.val <= 1000`

Analysis:

If we can in-order traverse the tree, we can get nodes in the order we want. However, if we want to chain a new node to the result when we visit it, we need to remember the last chained node and link the last chained node with the current visit node. Since we can only get the last visited node after we visit it, the last visited node must be either returned as a result or saved outside the function as a class property. What is more, we need to return the left most node as the result. Therefore, we shall make a dummy node to link to the leftmost node. We can pass this dummy node as a parameter when we search or we can make it as a class property. Here since we need to pass in a dummy node pointer, and need to return the last node of each visit. We can combine the two parameters as the class property. We can initialize the class property as the dummy node, link it to the new node, and move it to the new node as the last visited node.



```java
class Solution {
    
    TreeNode last;
    public TreeNode increasingBST(TreeNode root) {
        
        TreeNode dummy = new TreeNode();
        last = dummy;
        traverse(root);
        return dummy.right;
        
    }
    
    
    
    private void traverse(TreeNode cur) {
        
        if(cur == null) {
            return;
        }
        
        traverse(cur.left);
        
        last.right = cur;
        cur.left = null;
        last = cur;
        
        traverse(cur.right);
    }
}
```

