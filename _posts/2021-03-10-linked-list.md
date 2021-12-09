---
published: true
layout: post
date: '2021-03-10 10:50:00 -0000'
categories: algo
---
A linked list is composed of multiple `ListNode` nodes. `ListNode` is an object and it occupies 8 bytes (integer `value` 4 bytes + `ListNode` reference `next` 4 bytes) in heap memory. Each node is a reference of a `ListNode` object. It is similar to `next`reference. The node reference saves an address of a `ListNode` object in stack memory and occupies 4 bytes as well.

- Steps to Resolve Single Linked List Problem,

  - Consider previous/slow/last node as the last valid result node
  - Consider current node as a new enumerating node, and update current node.
  - Connect the updated current node to the previous/slow/last node
  - Move the previous/slow/last node forward to maintain its last position
  - We need to consider all possible previous/slow/last node's situations and cover all updates of current nodes.
  - If we need to update any node's previous node (such as slow node, or one node before the current node)'s `next` property, we need to consider use dummy node.

- Three Ways to Update a Node:

  - Update a node's value property(rarely)
  - Update a node's next property
  - Update a node's reference

- One-Pointer Vs Two-Pointer:

  Most of singly linked list questions can be resolved by both one-pointer or two-pointer. Personally, I will use two-pointer over one-pointer because two pointer's variable name (slow and fast) is clearer than one pointer's(`cur`and `cur.next`).

  - One-Pointer skips the disqualified element one by one by modifying **current node's next property** to point to the elements after the next element. When the element find a valid element, **current node's property** will point to the valid element to maintain the last valid position.

  - Two-Pointer use the fast pointer to find the next qualified element and point the **slow pointer's next property** to the qualified element. Then the **slow pointer's reference** will move to the qualified element to maintain the last valid position. 

  - The way to transfer two-point solution to one pointer is 

    - Consider slow pointer's next property's update as the one node next property's update. 

    - Consider the slow pointer's reference's move as the one node reference's move.

- Fast and Slow Pointer

  The Fast and Slow Pointer technique is widely used in linked list problem. The details can be found in [TwoPointer](./TwoPointer.md).

- Dummy Node:

  Dummy node is designed for simplifying corner cases brought by editing previous node's property before head or next node's property after tail. **If a solution requires updating a node's previous( or next for double linked list) node's properties, which are value and next, we often need to use the dummy node.** If we only need to update a node's previous node reference, we often don't need to use the dummy node.

  -  Single Linked List: When we update nodes in a single linked list, we often need to update 2 nodes, the node to be updated, and its previous node. There is one node in the list is special, which is head. It doesn't have a previous node. Therefore, head node's operations usually need to be dealt separately, and become corner cases. If we can add a dummy node before head, we can deal with head as all other nodes who have previous node. What is more, if we know a node, we can always find its next. That is to say, even we add a dummy node before head, we still can find head through dummy.
  -  Double Linked List: When we update a node in a double linked list, we often need to update 3 nodes, the node that needs to be updated, the node before it, and the node after it. There are 2 nodes in the list are special, which are head and tail. The head doesn't have a previous node, and the tail doesn't have a next node. If we can add a dummy node before head and a dummy node after tail, we can deal with head and tail as all other nodes.

### Problems:

#### [112. Remove Duplicates from Sorted List](https://www.lintcode.com/problem/112/)

####  [113. Remove Duplicates from Sorted List II](https://www.lintcode.com/problem/113/)

#### [35. Reverse Linked List](https://www.lintcode.com/problem/35/)

#### [36. Reverse Linked List II](https://www.lintcode.com/problem/36/)

#### [450. Reverse Nodes in k-Group](https://www.lintcode.com/problem/450/)

#### [(Leetcode)24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

#### [(Leetcode) 138.Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

#### [96. Partition List](https://www.lintcode.com/problem/96/)

#### [165. Merge Two Sorted Lists](https://www.lintcode.com/problem/165/solution)

#### [98. Sort List](https://www.lintcode.com/problem/98/)

#### [99. Reorder List](https://www.lintcode.com/problem/99/solution)

#### [453. Flatten Binary Tree to Linked List](https://www.lintcode.com/problem/453)

#### [104. Merge K Sorted Lists](https://www.lintcode.com/problem/104)
