---
published: true
layout: post
date: '2021-03-10 10:50:00 -0000'
categories: linked list
---
A linked list is composed of multiple `ListNode` nodes. `ListNode` is an object and it occupies 8 bytes (integer value 4 bytes + `ListNode` reference next 4 bytes) in heap memory. Each node is a reference of a `ListNode` object. It is similar to next reference. The node/next reference saves an address of a `ListNode` object in stack memory and occupies 4 bytes as well.

- Steps to Resolve Signal Linked List Problem,

  - Consider previous/slow/last node as the last valid result node
  - Consider current node as a new enumerating node, and update current node.
  - Connect the updated current node to the previous/slow/last node
  - Move the previous/slow/last node forward to maintain its last position
  - We need to consider all possible previous/slow/last node situation and cover all updates of current nodes.
  - If we need to update the previous/slow/last node's next, we need to consider use dummy node.

- Three Ways to Update a Node:

  - Update a node's value property(rarely)
  - Update a node's next property
  - Update a node's reference

- One-Pointer Vs Two-Pointer:

  Most of signally linked list questions can be resolved by both one-pointer or two-pointer. Personally, I will use two-pointer over one-pointer because two pointer's variable name (slow and fast) is clearer than one pointer's(`cur`and `cur.next`).

  - One-Pointer skips the disqualified element one by one by modifying **current node's next property** to point to the elements after the next element. When the element find a valid element, **current node's property** will point to the valid element to maintain the last valid position.

  - Two-Pointer use the fast pointer to find the next qualified element and point the **slow pointer's next property** to the qualified element. Then the **slow pointer's reference** will move to the qualified element to maintain the last valid position. 

  - The way to transfer two-point solution to one pointer is 

    - Consider slow pointer's next property's update as the one node next property's update. 

    - Consider the slow pointer's reference's move as the one node reference's move.

- Fast and Slow Pointer

  The Fast and Slow Pointer technique is widely used in linked list problem. The details can be found in [TwoPointer](./TwoPointer.md).

- Dummy Node:

  Dummy node is designed for simplifying corner cases brought by editing previous node's property before head or next node's property after tail. **If a solution requires updating a node's previous( or next for double linked list) node's properties, which are value and next, we often need to use the dummy node.** If we only need to update a node's previous node reference, we often don't need to use the dummy node.

  -  Single Linked List: When we update nodes in a single linked list, we often need to update 2 node, the node to be updated, and its previous node. There is one node in the list is special, which is head. It doesn't have a previous node. Therefore, head node's operations usually need to be dealt separately, and become corner cases. If we can add a dummy node before head, we can deal with head as all other nodes who have previous node. What is more, if we know a node, we can always find its next. That is to say, even we add a dummy node before head, we still can find head through dummy.
  -  Double Linked List: When we update a node in a double linked list, we often need to update 3 nodes, the node that needs to be updated, the node before it, and the node after it. There are 2 nodes in the list are special, which are head and tail. The head doesn't have a previous node, and the tail doesn't have a next node. If we can add a dummy node before head and a dummy node after tail, we can deal with head and tail as all other nodes.

### Problems:

###### [112. Remove Duplicates from Sorted List](https://www.lintcode.com/problem/112/)

Description:

Given a sorted linked list, delete all duplicates such that each element appear only *once*.

```
Example 1:
	Input:  null
	Output: null


Example 2:
	Input:  1->1->2->null
	Output: 1->2->null
	
Example 3:
	Input:  1->1->2->3->3->null
	Output: 1->2->3->null
```

Analysis: 

First we can use fast and slow pointer to solve this question. To use fast and slow pointer template, we need to think of what situation can make slow pointer move. When the fast pointer is different from slow pointer, we will update slow pointer's next reference to the fast pointer's element to collect new result and move slower pointer's reference to the fast to maintain the last position. When the fast pointer is same as slow pointer, we will only move fast pointer to skip the current invalid element and to enumerate the next possible element.

Other than use two-pointer way, we will use comparing each duplicated element with the first element way. Details can be found in [Remove Duplicates Trick Among Consecutive Duplicates](./CleanCodePractice.md).

Pointer `slow` is a type of previous node, and we need to update `slow.next` element. Therefore, we shall think of using dummy node. However, the update operation `slow.next = fast` will start from the second element, not from head element. All elements after head will have a not null previous node, we don't need dummy node here.

```java
public ListNode deleteDuplicates(ListNode head) {

    ListNode slow = head;
    ListNode fast = head;

    while(fast != null) {

        if(slow.val != fast.val) {
            slow.next = fast;
            slow = fast; 
        }

        fast = fast.next;

    }

    if(slow != null) {
        slow.next = null;
    }


    return head;
}
```

Second, we will still use two-pointer to solve the problem, and we will use comparing each duplicated element with the next element way. Details can be found in [Remove Duplicates Trick Among Consecutive Duplicates](./CleanCodePractice.md).

The difference of `slow` pointer from the above solution is update operation `slow.next = fast`can start from the head element. Therefore, we need a dummy node.

```java
public ListNode deleteDuplicates(ListNode head) {
    if(head == null || head.next == null) {
        return head;
    }

    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode slow = dummy;
    ListNode fast = head;

    while(fast != null) {

        if(fast.next == null || fast.next.val != fast.val) {
            slow.next = fast;
            slow = fast;

        }

        fast = fast.next;
    }


    return dummy.next;
}
```

We can also use one-pointer to resolve this question. We can consider when one-pointer's `next` property needs to be updated, and when one-pointer's reference itself needs to move. 

Here `slow.next`update is similar to `cur.next`update. Since `cur.next` update will start from second element, we don't need dummy node here.

```java
public ListNode deleteDuplicates(ListNode head) {

    ListNode cur = head;
    while(cur != null) {

        if(cur.next != null && cur.next.val == cur.val) {
            cur.next = cur.next.next;
        }else {
            cur = cur.next;

        }

    }

    return head;
}
```



######  [113. Remove Duplicates from Sorted List II](https://www.lintcode.com/problem/113/)

Description:

Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.

Example 1

```
Input : 1->2->3->3->4->4->5->null
Output : 1->2->5->null
```

Example 2

```
Input : 1->1->1->2->3->null
Output : 2->3->null
```



Analysis:

We can try to use fast and slow pointer to resolve this question. To use the fast and slow pointer template, we shall think of what situation can make slow  pointer moves. When the element is not a duplicate element, we shall move slow pointer. To tell whether an element is a duplicated element, we need to compare the element with its consecutive previous and next element. However, linked list node can not get its consecutive previous element. When an element is not equal to its next element, we can not tell whether this element is a duplicated number. However, when an element is equal to its next element, we are sure the element is a duplicated element. If we skip all elements equals to that duplicated elements, we can assure that the rest elements are the elements that slower pointer wants to collect.

Update operation `slow.next = fast`can start from the head element. Therefore, we need a dummy node.

Two-Pointer Solution:

```java
public ListNode deleteDuplicates(ListNode head) {

    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode slow = dummy;
    ListNode fast = head;

    while(fast != null) {

        if(fast.next != null && fast.val == fast.next.val) {
            int val = fast.val;

            while(fast != null && fast.val == val) {
                fast = fast.next;
            }

        }else {
            slow.next = fast;
            slow = fast;
            fast = fast.next;
        }



    }
    slow.next = null;

    return dummy.next;
}
```



Another way to tell whether an element is a duplicated element, we can introduce a third pointer `base` pointer always pointing to the first non-duplicated element. Since the `base` element is the first unique number, we can make sure it is not equal to its previous element. If the base element is not equal its next element, we can let slow pointer collect the base element's result.

Update operation `last.next = base`can start from the head element. Therefore, we need a dummy node.

```java
public ListNode deleteDuplicates(ListNode head) {
    if(head == null || head.next == null) {
        return head;
    }

    ListNode dummy = new ListNode(0);
    ListNode last = dummy;

    ListNode cur = head;
    ListNode base = head;

    while(cur != null) {

        if(cur.val != base.val) {
            base = cur;
        }

        if(base.next == null || base.val != base.next.val) {
            last.next = base;
            last = base;
        }


        cur = cur.next;
    }

    last.next = null;

    return dummy.next;
}
```

This question can also use one pointer to resolve. We only need to split the slow pointer's update as one-pointer's `next` property update and slow pointer's move as one pointer's move.

Here `slow.next` update is similar to `cur.next` update. Since update of `cur.next`can start from head, `cur` can be a node before head. Therefore, we need to use dummy node. 

One pointer:

```java
public ListNode deleteDuplicates(ListNode head) {
    if(head == null || head.next == null)
        return head;

    ListNode dummy = new ListNode(0);
    dummy.next = head;

    ListNode cur = dummy;
    while (cur.next != null && cur.next.next != null) {
        if (cur.next.val == cur.next.next.val) {
            int val = cur.next.val;
            while (cur.next != null && cur.next.val == val) {
                cur.next = cur.next.next;
            }            
        } else {
            cur = cur.next;
        }
    }

    return dummy.next;
}
```

###### [35. Reverse Linked List](https://www.lintcode.com/problem/35/)

Description:

Reverse a linked list.

Example 1:

```
Input: 1->2->3->null
Output: 3->2->1->null
```

Reverse it in-place and in one-pass

Analysis:

This is a basic operation. We only have one previous situation. Since we don't need to update `pre.next`, we don't need to use dummy node. The code of move is a cycle.

```java
public ListNode reverse(ListNode head) {

    if(head == null) {
        return null;
    }

    ListNode pre = null;
    ListNode cur = head;

    while(cur != null) {
        ListNode next = cur.next;
        cur.next = pre;
        pre = cur;
        cur = next;
    }

    return pre;

}
```

###### [36. Reverse Linked List II](https://www.lintcode.com/problem/36/)

Description:

Reverse a linked list from position m to n.

Example 1:

```
Input: 1->2->3->4->5->NULL, m = 2 and n = 4, 
Output: 1->4->3->2->5->NULL.
```

Example 2:

```
Input: 1->2->3->4->NULL, m = 2 and n = 3, 
Output: 1->3->2->4->NULL.
```

Reverse it in-place and in one-pass

Given m, n satisfy the following condition: 1 ≤ m ≤ n ≤ length of list.



Analysis:

We need to find the previous node of m, reverse the node between m and n, and connect the previous node of m with the first reversed node, and connect the reversed last node with the node after n.

```java
    public ListNode reverseBetween(ListNode head, int m, int n) {
        
        ListNode dummy = new ListNode(-1);
        dummy.next = head;
		//move cur to previous node of m
        ListNode cur = dummy;
        for(int i = 0; i < m - 1; i++) {
            cur = cur.next;
        }

        ListNode preMNode = cur;
        ListNode mNode = cur.next;
		//reverse node from m to n
        ListNode reversePre = preMNode;
        ListNode reverseCur = mNode;

        for(int i = 0; i < n - m + 1; i++) {
            ListNode reverseNext = reverseCur.next;
            reverseCur.next = reversePre;
            reversePre = reverseCur;
            reverseCur = reverseNext;
        }
		//Connect the previous m node with the reversed first node
        //Connect the reversed last node with the node after n.
        preMNode.next = reversePre;
        mNode.next = reverseCur;

        return dummy.next;

    }
```



###### [450. Reverse Nodes in k-Group](https://www.lintcode.com/problem/450/)

Description:

Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.

If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.

You may not alter the values in the nodes, only nodes itself may be changed.
Only constant memory is allowed.

Example 1

```plain
Input:
list = 1->2->3->4->5->null
k = 2
Output:
2->1->4->3->5
```

Example 2

```plain
Input:
list = 1->2->3->4->5->null
k = 3
Output:
3->2->1->4->5
```

Analysis: 

This question is an extension of  [36. Reverse Linked List II](#36. Reverse Linked List II). We need to find the previous node of each consecutive k nodes `ListNode pre`, the first node of each consecutive k nodes `ListNode cur`, and the kth node `kthNode` . Then we will reverse k consecutive nodes, connect the `ListNode pre` with the first node of reversed k nodes, and connect the last node of reversed k nodes with node after `kthNode`.

```java
    public ListNode reverseKGroup(ListNode head, int k) {
        if(head == null || k == 1) {
            return head;
        }

        ListNode dummy = new ListNode(0);
        dummy.next = head;
 
        ListNode preGroup = dummy;
        ListNode groupFirst = head;

        
        ListNode kthNode = findKth(groupFirst, k);

        while(kthNode != null) {
            
            ListNode kthNext = kthNode.next;

            reverse(groupFirst, k);

            preGroup.next = kthNode;
            groupFirst.next = kthNext;

            preGroup = groupFirst;
            groupFirst = kthNext;

            kthNode = findKth(groupFirst, k);

        }

        return dummy.next;

        
    }
    
    private ListNode findKth(ListNode head, int k) {
        
        for(int i = 0; i < k - 1; i++){
            
            if(head == null) {
                return null;
            }
            head = head.next;
        }
        
        return head;
    }

    private void reverse(ListNode first, int k) {

        ListNode pre = null; 
        ListNode cur = first;

        for(int i = 0; i < k; i++) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;
        }
    }
```

###### [(Leetcode) 138.Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer/)

Description:

A linked list of length `n` is given such that each node contains an additional random pointer, which could point to any node in the list, or `null`.

Construct a [**deep copy**](https://en.wikipedia.org/wiki/Object_copying#Deep_copy) of the list. The deep copy should consist of exactly `n` **brand new** nodes, where each new node has its value set to the value of its corresponding original node. Both the `next` and `random` pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. **None of the pointers in the new list should point to nodes in the original list**.

For example, if there are two nodes `X` and `Y` in the original list, where `X.random --> Y`, then for the corresponding two nodes `x` and `y` in the copied list, `x.random --> y`.

Return *the head of the copied linked list*.

The linked list is represented in the input/output as a list of `n` nodes. Each node is represented as a pair of `[val, random_index]` where:

- `val`: an integer representing `Node.val`
- `random_index`: the index of the node (range from `0` to `n-1`) that the `random` pointer points to, or `null` if it does not point to any node.

Your code will **only** be given the `head` of the original linked list.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2019/12/18/e1.png)

```
Input: head = [[7,null],[13,0],[11,4],[10,2],[1,0]]
Output: [[7,null],[13,0],[11,4],[10,2],[1,0]]
```

Example 2:

```
Input: head = []
Output: []
Explanation: The given linked list is empty (null pointer), so return null.
```

Constraints:

- `0 <= n <= 1000`
- `-10000 <= Node.val <= 10000`
- `Node.random` is `null` or is pointing to some node in the linked list.

Analysis:

If this question doesn't have a random pointer, we can create the deep copy list as the order of traversing the original list. However, when a node has a random pointer, we can not create a deep copy only by the order of normal traverse. There is a chance when we create a new copied node, its random node is not created yet. Therefore, we need to create all new nodes, and then, connect each node's with its random node. 

Here is another problem, when we find its original node's random node, we need to search that random new copied node. Here we can use hash table to save an original node and its copied node as a pair (which is common way widely used in clone graph). We can also attach the copied new node to each original node for copying random pointer, and split the new nodes with the original list later. 

Here we will only use the second way to implement this question.

After insertion, the list becomes as following picture,

![](E:\study\jiuzhang\Notes\CopyListRandomPointer.JPG)

After we copied all random pointers, we will split the list from the combined list.

```java
public Node copyRandomList(Node head) {

    if(head == null) {
        return head;
    }

    //Create new nodes and link new nodes with orignal nodes
    Node cur = head;

    while(cur != null) {

        Node newNode = new Node(cur.val);
        newNode.next = cur.next;
        cur.next = newNode;
        cur = cur.next.next;

    }
    //Copy random pointer
    cur = head;
    while(cur != null) {

        if(cur.random != null) {
            cur.next.random = cur.random.next;
        }

        cur = cur.next.next;

    }
    //Split the new nodes with oirgnal nodes
    Node dummy = new Node(-1);
    Node last = dummy;

    cur = head;
    while(cur != null) {
        last.next = cur.next;
        last = last.next;
        cur.next = cur.next.next;
        cur = cur.next;

    }

    return dummy.next;

}
```

###### [96. Partition List](https://www.lintcode.com/problem/96/)

Description:

Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.

You should preserve the original relative order of the nodes in each of the two partitions.

Example 1:

```
Input:  list = null, x = 0
Output: null	
Explanation: The empty list Satisfy the conditions by itself.
```

Example 2:

```
Input:  list = 1->4->3->2->5->2->null, x = 3
Output: 1->2->2->4->3->5->null	
Explanation:  keep the original relative order of the nodes in each of the two partitions.
```

Analysis:

This question is a basic operation of quick sort applied on the linked list. Not like partition applied on array mainly depending on swapping numbers in place, partition applied on linked list will separate the original list into 2 new lists (one contains all numbers less than the pivot, and the other contains all numbers greater than or equal to the pivot). Then we can combine the 2 new lists into a new list. Even we create 2 new lists, we just create 2 new dummy nodes. Therefore, the space complexity is still O(1).

```java
public ListNode partition(ListNode head, int x) {

    if(head == null) {
        return null;
    }

    ListNode lessDummy = new ListNode(0);
    ListNode lessCur = lessDummy;
    ListNode greaterDummy = new ListNode(0);
    ListNode greaterCur = greaterDummy;

    ListNode cur = head;
    while(cur != null) {

        if(cur.val < x) {
            lessCur.next = cur;
            lessCur = cur;
        }else {
            greaterCur.next = cur;
            greaterCur = cur;
        }

        cur = cur.next;
    }

    lessCur.next = greaterDummy.next;
    greaterCur.next = null;

    return lessDummy.next;

}
```

###### [165. Merge Two Sorted Lists](https://www.lintcode.com/problem/165/solution)

Description:

Merge two sorted (ascending) linked lists and return it as a new sorted list. The new sorted list should be made by splicing together the nodes of the two lists and sorted in ascending order.

```
Example 1:
	Input: list1 = null, list2 = 0->3->3->null
	Output: 0->3->3->null


Example 2:
	Input:  list1 = 1->3->8->11->15->null, list2 = 2->null
	Output: 1->2->3->8->11->15->null
```

Analysis:

This is a basic operation. We will use 2 pointers to go along the two lists and always pick the smaller one to add to the final result. Then we need to append whatever left either in list1 or list2 to the final result as well. 

```java
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {

    ListNode dummy = new ListNode(0);
    ListNode cur = dummy;

    while(l1 != null && l2 != null) {

        if(l1.val > l2.val) {
            cur.next = l2;
            l2 = l2.next;
        }else {
            cur.next = l1;
            l1 = l1.next;
        }

        cur = cur.next;
    }

    if(l1 != null) {
        cur.next = l1;
    }

    if(l2 != null) {
        cur.next = l2;
    }
    return dummy.next;
}
```



###### [98. Sort List](https://www.lintcode.com/problem/98/)

Description:

Quick Sort a linked list in O(*n* log *n*) time using constant space complexity.

Example 1:

```
Input:  1->3->2->null
Output:  1->2->3->null
```

Analysis:

This question is a good practice of linked list basic operations, but it is often asked in interviews. We will use quick sort and merge sort 2 ways to implement this question. However, merge sort is more suitable to sort linked list. Quick sort's time complexity relies on how random we pick the pivot. Linked list is harder than array to pick a random pivot. Also, quick sort on an array has many swap operations to avoid extra space, but merge sort on an array need extra space. Merge sort on linked list can avoid extra space by modifying node's links.

Quick sort :

- Partition an linked list into separate 3 lists (`< pivot, =pivot, >pivot`). To split into 3 parts is to make sure both `< pivot and >pivot` recursion part will decrease for sure after every call. Here we separate three lists by setting each list's tail to null. We also can use end node, but then we need consider the end node is null or not. The code will be more complicated.
- We pick the pivot as the head of the list.
- Then we will call recursion to sort both less than pivot and greater than pivot lists.
- After we get three lists' heads, we can concatenate the 3 separate lists back to a ordered list.

```java
    public ListNode sortList(ListNode head) {
        if(head == null) {
            return head;
        }

        return quickSort(head);
    }

    private ListNode quickSort(ListNode start) {
        if(start == null) {
            return null;
        }

        ListNode pivot = start;

        ListNode lessDummy = new ListNode(0);
        ListNode pivotDummy = new ListNode(0);
        ListNode greaterDummy = new ListNode(0);

        ListNode lessCur = lessDummy;
        ListNode greaterCur = greaterDummy;
        ListNode pivotCur = pivotDummy;

        ListNode cur = start;
        while(cur != null) {
            if(cur.val > pivot.val) {
                greaterCur.next = cur;
                greaterCur = greaterCur.next;
            }else if(cur.val < pivot.val) {
                lessCur.next = cur;
                lessCur = lessCur.next;
            }else if(cur.val == pivot.val) {
                pivotCur.next = cur;
                pivotCur = pivotCur.next;
            }
            cur = cur.next;
        }

        lessCur.next = null;
        pivotCur.next = null;
        greaterCur.next = null;    

        ListNode lessHead = quickSort(lessDummy.next);
        ListNode greaterHead = quickSort(greaterDummy.next);
		
        //In this method, we will both use head and tail of all 3 seperated lists. Here we will find the tail from head again rather than to use the lessCur, greaterCur, pivotCur as tail, becuase lessCur, greaterCur, pivotCur might not be the tail any more after recursion call. We need to search them again.
        return concat(lessHead, pivotDummy.next, greaterHead);
    }

    private ListNode findLast(ListNode head) {

        while(head != null && head.next != null) {
            head = head.next;
        }

        return head;
    }

    private ListNode concat(ListNode lessHead, ListNode pivot, ListNode greaterHead) {

        ListNode dummy = new ListNode(0);
        ListNode tail = dummy;

        tail.next = lessHead;
        tail = findLast(tail);
        tail.next = pivot;
        tail = findLast(tail);
        tail.next = greaterHead;

        return dummy.next;
    }
```

Merge Sort:

Sort linked list using merge sort can avoid extra O(n) space. Here we need to pay attention to one detail. The `findMiddle(ListNode head)` is different from [1609. Middle of the Linked List](./TwoPointer.md). The`fast` pointer starts from different position. It will lead to `slow` pointer stops at different positions when the list elements' size are even. Since we split the list as `(head, mid)`, `(mid.next, null)`. When there are 2 elements, we need mid shall be the first element. If mid stops at the second, the 2 elements won't be minimized into 0 or 1 element any more. The recursion will become infinite.

```java
public class Solution {
    /**
     * @param head: The head of linked list.
     * @return: You should return the head of the sorted linked list, using constant space complexity.
     */
    public ListNode sortList(ListNode head) {
        if(head == null) {
            return head;
        }
        return mergeSort(head);

    }

    private ListNode mergeSort(ListNode head) {
        if(head == null || head.next == null) {
            return head;
        }

        ListNode mid = findMiddle(head);
        ListNode next = mid.next;
        mid.next = null;


        ListNode l1 = mergeSort(head);
        ListNode l2 = mergeSort(next);

        ListNode merged = merge(l1, l2);


        return merged;

    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;

        while(l1 != null && l2 != null) {
            if(l1.val < l2.val) {
                cur.next = l1;
                l1 = l1.next;
            }else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
        }

        if(l1 != null) {
            cur.next = l1;
        }
        if(l2 != null) {
            cur.next = l2;
        }
        return dummy.next;
    }

    private ListNode findMiddle(ListNode head) {
        
        ListNode slow = head;
        ListNode fast = head.next;


        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;
    }
}
```

###### [99. Reorder List](https://www.lintcode.com/problem/99/solution)

Description:

Given a singly linked list L: L0 → L1 → … → Ln-1 → Ln

reorder it to: L0 → Ln → L1 → Ln-1 → L2 → Ln-2 → …

```
Example 1:
	Input:  1->2->3->4->null
	Output: 1->4->2->3->null

Example 2:
	Input: 1->2->3->4->5->null
	Output: 1->5->2->4->3->null
```

Analysis:

This question is a combination of many sub questions of linked list.

- We need to find the middle point (if the list size is even, we will find the first of the two elements in middle). Here we need slow and fast are starting from different point.
- We will reverse the list from `mid.next` to the tail of the list.
- We need to cut the list from head to mid by set `mid.next = null`
- We will merge the two list and set the original head to the merged head.

```java
    public void reorderList(ListNode head) {

        if(head == null || head.next == null) {
            return;
        }

        ListNode mid = findMid(head);
        

        ListNode l2 = reverse(mid.next);
        ListNode l1 = head;
        mid.next = null;
        head = merge(l1, l2);



    }

    private ListNode findMid(ListNode head) {

        ListNode slow = head;
        ListNode fast = head.next;

        while(fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        return slow;
    }

    private ListNode reverse(ListNode head) {

        ListNode pre = null;
        ListNode cur = head;

        while(cur != null) {
            ListNode next = cur.next;
            cur.next = pre;
            pre = cur;
            cur = next;

        }

        return pre;

    }

    private ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;

        int count = 0;
        while(l1 != null && l2 != null) {
            if(count % 2 == 0) {
                cur.next = l1;
                l1 = l1.next;
            }else {
                cur.next = l2;
                l2 = l2.next;
            }
            cur = cur.next;
            count++;
        }

        if(l1 != null) {
            cur.next = l1;
        }
        
        if(l2 != null) {
            cur.next = l2;
        }

        return dummy.next;

    }


```

###### [453. Flatten Binary Tree to Linked List](https://www.lintcode.com/problem/453/solution)

Description:

Flatten a binary tree to a fake "linked list" in pre-order traversal.

Here we use the *right* pointer in `TreeNode` as the *next* pointer in `ListNode`.

Example 1:

```
Input:{1,2,5,3,4,#,6}
Output：{1,#,2,#,3,#,4,#,5,#,6}
Explanation：
     1
    / \
   2   5
  / \   \
 3   4   6

1
\
 2
  \
   3
    \
     4
      \
       5
        \
         6
```

Analysis:

We will preorder traverse the list and maintain the last node as a class property. Here we use a similar dummy node to avoid adding root node corner case. Also, we need to save the original right child before we modify the root's right child.

```java
public class Solution {
    /**
     * @param root: a TreeNode, the root of the binary tree
     * @return: nothing
     */

    private TreeNode last;

    public void flatten(TreeNode root) {
        last = new TreeNode(0);
        traverse(root);

    }

    private void traverse(TreeNode root) {
        if(root == null) {
            return;
        }

        TreeNode right = root.right;
        last.right = root;
        last = root;
        
        traverse(root.left);
        root.left = null;
        traverse(right);

    }
}
```



###### [104. Merge K Sorted Lists](https://www.lintcode.com/problem/104/solution)

Description:

Merge k sorted linked lists and return it as one sorted list.

Analyze and describe its complexity.

Example 1:

```
Input:   [2->4->null,null,-1->null]
Output:  -1->2->4->null
```

Example 2:

```
Input: [2->6->null,5->null,7->null]
Output:  2->5->6->7->null
```

Analysis:

We can dynamically manage a heap of all current nodes of all linked list. First we will insert all not null list head into a min heap. Every time when we poll the minimum linked list node, we will add its next not null node into heap. If the list is already null, we can ignore it. After we pop all nodes in order, we can get a merged sorted list. If there is k lists, and total n nodes. Time Complexity will be `O(nlogk)`. Space Complexity will be `O(k)`

```java
public class Solution {
    /**
     * @param lists: a list of ListNode
     * @return: The head of one sorted list.
     */
    public ListNode mergeKLists(List<ListNode> lists) { 

        if(lists == null || lists.size() == 0) {
            return null;
        } 
        
        ListNode dummy = new ListNode(0);
        ListNode cur = dummy;


        PriorityQueue<ListNode> minHeap = new PriorityQueue<>(lists.size(), new ListNodeComnparator());

        //add all heads into heap first
        for(ListNode ln : lists) {
            if(ln != null) {
                minHeap.offer(ln);
            }

        }

        while(minHeap.size() != 0) {

            ListNode temp = minHeap.poll();
            cur.next = temp;
            cur = cur.next;
            temp = temp.next;
            if(temp != null) {
                minHeap.offer(temp);
            }
        
            
        }

        return dummy.next;



    }
}

class ListNodeComnparator implements Comparator<ListNode> {

    public int compare(ListNode a, ListNode b) {
        return a.val - b.val;
    }

}
```


