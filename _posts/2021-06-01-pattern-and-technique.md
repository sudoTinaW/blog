---
published: false
layout: post
date: '2021-06-01 14:50:00 -0000'
categories: algo
---
This article is inspired by [Smilence Coding Notes](https://zybuluo.com/smilence/note/76)

#### String and Array

1. **Calculate each element's frequency. We often use hash table. If the question only cares about whether an element exit or not, we can use boolean value as hash table's value.** 
   - Implement an algorithm to determine if a string has all unique characters.
   - Write a method to decide if one string is a permutation of the other.
   - Given a newspaper and message as two strings, check if the message can be composed using letters in the newspaper.
   - Find a line which passes the most number of points.
2. **For each element `i`, we need to search all its former elements between `[0,i]` to build the result of element `i`.   If we know what values we are looking for, we can use hash table to save all its former elements as keys, so that for `i` the search time will be O(1)** 
   - [Given an array of integers, find two numbers such that they add up to a specific number.](https://www.lintcode.com/problem/two-sum/description)
   - [Given an unsorted array of integers, find the length of the longest consecutive elements sequence.](https://www.lintcode.com/problem/longest-consecutive-sequence/description)
3. **The common way to generate a unique value(usually a number) of any input, which is also known as a hash value of the input, is translating the input into a m-scale(m 进制) number. The m is the number of possibilities of each cell in the hash table. During the calculation, in case of overflow, we need to mod hash table size(how many cells in the hash table), of each calculation.**
   - Design a hash function that is suitable for words in a dictionary.
   - A hash function for the state of a chess game.
   - Implement a method rand7() given rand5(). (Representing a number(0 to 7) using 5-scale )
4. **If we need to handle 2 different size array or string, we often handle the part `aIndex < aSize && bIndex < bSize` first, then handle whatever left array or string **
5. **If we need to update or delete elements in-place, we can use two indexes to remember original index and updated index. If the updated version's size is bigger, we will enlarge the input size and then update elements in reverse order. If the updated version's size is smaller, we will update elements in order, and then shrink the input size.**
   * Given input "Mr John Smith", output "Mr%20John%20Smith" 
   * Given input "Mr%20John%20Smith" , output "Mr John Smith"
   * Given two sorted arrays, merge B into A in sorted order.
6. **Most of revising string problems can be resolved by one-time (in order or reverse order)traverse, the time complexity will often be `O(n)`. The common technique is using 2 indexes, one pointing to each piece's starting point, and one pointing to each piece's ending point. If the question is requiring to find sub array or string, we shall think of using sliding window techinuque**
   - Given input -> "I have 36 books, 40 pens2."; output -> "I evah 36 skoob, 40 2snep."
   - [Compress the string "aabcccccaaa" to "a2b1c5a3"](https://leetcode.com/problems/string-compression/)
   - [Longest Substring Without Repeating Characters](https://www.lintcode.com/problem/384/)
   - [Minimum Window Substring: Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).](https://www.lintcode.com/problem/32/) 
7. **If the question requires constant space and you can modify input, we can always try to sort it first and use with binary search or two pointer techniques.**
   - [Two Sum: Given an array of integers, find two numbers such that they add up to a specific number.](https://www.lintcode.com/problem/two-sum/my-submissions)
   - [Three Sum: Find all unique triplets in the array which gives the sum of zero.](https://www.lintcode.com/problem/3sum/solution)
   - [Remove Duplicates from Sorted Array](https://www.lintcode.com/problem/remove-duplicates-from-sorted-array/solution)
8. **To get max or min value of all elements or get sum of some elements, we can update the result as we traverse all elements without traversing multiple times. **
9. **2D array is often traversed, row by row, column by column or from outside to inside**
   - Given an NxN matrix, rotate the matrix by 90 degrees.
   - If `matrix[i][j]` is 0, then set the entire row and column to 0.

#### LinkedList

1. **Most of linked list problem need to update a node, or a pair of nodes (previous node, and current node). When we traverse a linked list node, we often update one node or a pair of node per each loop**

- [Reverse the linked list and return the new head.](https://www.lintcode.com/problem/35/)

2. **When we need to modify a linked list node, most of time, we need to find its previous node and modify its previous node's next property.**
3. **When we need to swap two nodes, no matter where the two nodes' location are, we need to update two node's previous nodes' next property, and then update two nodes' `next` property.**
   - [Given a linked list, swap every two adjacent nodes and return its head](https://leetcode.com/problems/swap-nodes-in-pairs/)

#### Tree & Graph

1. **Subtree or subgraph's optimal solution can induce an overall optimal solution. It is a greedy algorithm. Those problems can often use divide and conquer techniques. Sub problems are independent to each other. Subtree problem can think as we need to visit all subtrees whose tops are all nodes, to get a final result**

   - [Check if a binary tree is balanced.](https://www.lintcode.com/problem/balanced-binary-tree/description)
   - [Check if a binary tree is a BST.](https://www.lintcode.com/problem/validate-binary-search-tree/description)
   - [Check if T2 is a subtree of T1.](https://leetcode.com/problems/subtree-of-another-tree/) 
   - [Given a binary tree, find its minimum depth.](https://leetcode.com/problems/minimum-depth-of-binary-tree/)

2. **Tree path problems often use top-down traverse with backtracking to collect result. The path here is the single path, which means, each level only pass one node.**

   - [Given a binary tree, print all paths which sum to a give value.](https://www.lintcode.com/problem/binary-tree-path-sum-ii/description)
   - [Given a binary tree, find all root-to-leaf paths which sum to a given value.](https://leetcode.com/problems/path-sum-ii/)

3. **Converting a tree into other data structure**

   - We can use BFS to convert a tree into other data structure.
     - Given a binary tree, creates linked lists of all the nodes at each depth.
   - We can use top-down traverse or divide and conquer to convert a tree into other data structure. 
     - [Given a binary tree, flatten it to a linked list in-place.](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)
     - Convert a binary tree into a doubly linked list in place. 
   - We can build other data structure into a tree by top-down traverse or divide and conquer.
     - [Convert a sorted array (increasing order) to a balanced BST.](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

4. **Find a specific node**

   - To find a specific node, we will start searching along bottom-up direction. If neither a left child nor right child of a node is the specific node, we will keep looking upwards.

     - If we have a parent node, we can look up through its parent node.
       - Find the in-order successor of a given node, with parent links. 
       - Find the immediate right neighbour of the given node, with parent links given, but without root node.
     - If we don't have a parent node, we will keep looking up through backtracking.
       - [Find the first common ancestor of two nodes in a binary tree.](https://www.lintcode.com/problem/lowest-common-ancestor-of-a-binary-tree/description)ß

   - To find a specific node on a BST, we can start searching along top-down direction because we can pick either left or right child branch to look up when we visit the parent.

     - [Find the in-order successor of a given node in BST, without parent links.](https://www.lintcode.com/problem/inorder-successor-in-bst/)

5. **Traverse a Graph**

   - If the question is about getting the shortest path. We will often use BFS. We can remember each node's previous node and build the path using recursion.

     - [Given two words (start and end), and a dictionary, find all shortest transformation sequence(s) from start to end, such that:](https://www.lintcode.com/problem/word-ladder/) 

       a. Only one letter can be changed at a time

       b. Each intermediate word must exist in the dictionary

   - If the question is about traversing of a graph, we can use both BFS and DFS.

     - Given a directed graph, find out whether there is a route between two nodes

#### Sorting and Searching

1. **Dynamically manage data**

   - BST can be considered as a dynamically managed sorted array. It is easy to update and search.
     - [Implement data structures to look up the rank of a number x( the number of values less than or equal to x)](https://www.geeksforgeeks.org/rank-element-stream/) 
   - Heap can mange dynamic changed data stream's min, max, or `kth` element. It is easy to push and pop elements, but it is hard to update or search an element.(Elements are partial sorted).
     - [Design an algorithm for computing the k-th largest element in a sequence of elements, one presented at a time.](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
   - Linked List is easy to update, add or delete elements, but it is hard to search. 
   - Table: it is easy to search in `O(1)` time.
   - LRU Cache: If the objects need to be sorted by time, we can often use this data structure. We can use Linked List to store data, and hash table to assist quick look-up. Each linked list node are sorted by updating or adding order. It is easy to update, delete and search. LRU cache is like a cache pool where you can store data in order and access or add data quickly. 
     - Design a caching mechanism for the most recent queries of a search engine.
     - [Given a server that has requests coming in. Design a data structure such that you can fetch the count of the number requests in the last second, minute and hour.](http://stackoverflow.com/questions/18364490/implementation-of-a-hits-in-last-second-minute-hour-data-structure) 

2. **The sorted element's key has a range or has many duplications, or we only need sort elements partially, we can use bucket sort. Key is the index of a table. If the key's length is limited, we can use radix sort.**

   - Sort a large number of people by their ages.
   - [Sort an array of strings that all the anagrams are next to each other.](https://leetcode.com/problems/group-anagrams/)

3. **Scalability & Memory Limits Problems**

   Those problems are often solved by divide & conquer. We can divide, or sort input into chunks, handle their result and merge all chunks result. We often use hash table to lookup each chunk.

   - Find all documents that contain a list of words
   - Design the data structures for a very large social network and an algorithm to show the connection between two people.
   - Given an input file with one billion distinct non-negative integers, provide an algorithm to generate an integer not contained in the file.Assume you have only 10MB of Memory
   - You have 10 billion URLS.How do you detect the duplicate ones?

4. **Binary Search**

   For any sorted container, if its data structure can not be changed, binary search will be the most efficient way.

   - [Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.](https://leetcode.com/problems/search-insert-position/) 
   - [Implement int sqrt(int x)](https://leetcode.com/problems/sqrtx/)

   Even the container is only partially sorted. We can still use binary search.

   - A magic index is defined to be an index such that `A[i] = i`.Find a magic index in a sorted array.
   - [Find an element in a rotated array ( was originally sorted).](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)
   - [Given a sorted array of integers, find the starting and ending position of a given target value.](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/)

   Implement a function which takes as input a float variable x and return its square root.

5. **Quick sort partition thought is often used to group elements into 2 or 3 parts. One of its famous extension is quick select**

   - [Compute the k-th largest element in an array A that runs in expected time.](https://leetcode.com/problems/kth-largest-element-in-an-array/)
   - [There are n points on a 2D plan, find the k points that are closest to origin ( x= 0, y= 0)](https://leetcode.com/problems/k-closest-points-to-origin/).

#### Bit Manipulation

1. **Basic bit manipulation target is counting `1` distribution**
   - `^` has two famous property, `0 ^ a = a`, and `a ^ a = 0`
   - `n & (n - 1) can clear the lowest bit of 1`
   - `>>  `is divide by 2
   - `>>` is multiplied by 2.

#### Stack And Queues

1. **If we need to use stack to access data in order, we often use two stacks in same order or in two reversed order to implement it. If the two stacks are accessing data in reversed order, we often need to dump the data between 2 stacks.**
   - [Design a stack with `push()`, `pop()` and `min()` in time.](https://www.lintcode.com/problem/12/description)
   - [Implement a `queue` with two stacks](https://www.lintcode.com/problem/40/)
   - Sort a stack in ascending order with an additional stack
2. **If one element's result depends on its later element, we can use stack to save the former element and fetch it until all its later elements collected to form the final result.**
   - [Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.](https://www.lintcode.com/problem/423/)
   - [Evaluate a Postfix Notation](https://www.lintcode.com/problem/424/)

#### Dynamic Programming
