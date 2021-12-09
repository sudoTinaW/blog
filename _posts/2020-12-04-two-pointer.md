---
published: true
layout: post
date: '2020-12-04 14:50:00 -0000'
categories: algo
---
Two-pointer technique is commonly used to solve array, string and linked list problem. It often solves the problem in place. We have classified the two-pointer questions by its templates. The two pointer questions include *[sliding window](https://www.tinawang.ca/algo/2020/10/20/sliding-window.html)*,  *[partition(merge sort, quick sort)](https://www.tinawang.ca/algo/2021/01/08/sort.html)*,  *[binary search](https://www.tinawang.ca/algo/2020/06/11/binary-search.html)*, [*fast and slow pointer*](#Fast and Slow Pointer (Hare & Tortoise)), and [*left and right pointer*](Left and Right Pointer) problems. In this article, we will only describe  [*fast and slow pointer*](#Fast and Slow Pointer (Hare & Tortoise)), and [*left and right pointer*](Left and Right Pointer) .

*The main thought in this article is originated from [labuladong](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/shuang-zhi-zhen-ji-qiao), and [jiuzhang](https://www.jiuzhang.com/course/)*

#### Fast and Slow Pointer (Hare & Tortoise):

- It is mostly applied on the linked list or sorted array. The question usually requires O(1) extra space, i.e., no extra spaces.
- It is mostly used by single linked list, whose pointer can not traverse reversely.
- The slow pointer represents the last qualified result before fast pointer. The fast pointer will enumerate all elements. Faster pointer will always move in each iteration no matter slow pointer moves or not. The slow pointer will move only when fast pointer's element satisfied some requirements. Such moving way can make sure the slow pointer always fall behind the fast pointer.
- Since the slow pointer represents the last qualified result, the slow pointer can start from the first element when the first element is a valid result. Slow pointer can also start before the first element such as dummy head or null when the first element may or may not be a valid result.  
- Since the fast pointer represents all possible elements, the fast pointer usually starts from the first element.
- The fast and slow pointer is similar to sliding window. The difference is slow pointer is usually moving one step forward for each right element. In most cases, we need to consider what condition the fast pointer needs to satisfy to make **slow pointer move**. (slow pointer 什么时候走)。


#### Left and Right Pointer:

- It is often used for searching a group of elements satisfying some requirements in sorted array or linked list. The group can be a pair, 3 elements or even a sub array.
- In each iteration, either left and right pointer will move.（要么left pointer走一步， 要么right pointer走一步）。

#### Two Different Array Pointers:

The two pointers are moving on two different array or list.


### Problems

#### Fast and Slow Pointer (Hare & Tortoise):

#### [141. Linked List Cycle (Leetcode)](https://leetcode.com/problems/linked-list-cycle/)

#### [142. Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/)

#### [380. Intersection of Two Linked Lists](https://www.lintcode.com/problem/380/my-submissions)

####  [1609. Middle of the Linked List](https://www.lintcode.com/problem/middle-of-the-linked-list/description)

#### [166. Nth to Last Node in List](https://www.lintcode.com/problem/nth-to-last-node-in-list/description)

#### [112. Remove Duplicates from Sorted List](https://www.lintcode.com/problem/remove-duplicates-from-sorted-list/description)

####  [100. Remove Duplicates from Sorted Array](https://www.lintcode.com/problem/remove-duplicates-from-sorted-array/solution)

#### [172. Remove Element](https://www.lintcode.com/problem/remove-element/description)

####  [539. Move Zeroes](https://www.lintcode.com/problem/539/)

#### Left and Right Pointer:

#### [56. Two Sum](https://www.lintcode.com/problem/two-sum/my-submissions)

#### [587. Two Sum - Unique pairs](https://www.lintcode.com/problem/two-sum-unique-pairs/description)

#### [533. Two Sum - Closest to target](https://www.lintcode.com/problem/two-sum-closest-to-target/description)

#### [57. 3Sum](https://www.lintcode.com/problem/3sum/solution)

#### [59. 3Sum Closest](https://www.lintcode.com/problem/3sum-closest/solution)

#### [58. 4Sum](https://www.lintcode.com/problem/4sum/description)

#### [382. Triangle Count](https://www.lintcode.com/problem/triangle-count/solution)

#### [(Leetcode)977 Squares of a Sorted Array](https://leetcode.com/problems/squares-of-a-sorted-array/)

####  [610. Two Sum - Difference equals to target](https://www.lintcode.com/problem/two-sum-difference-equals-to-target/description)


#### Two Different Array Pointers:

#### [547. Intersection of Two Arrays](https://www.lintcode.com/problem/547/my-submissions)
