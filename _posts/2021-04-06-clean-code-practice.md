---
published: true
layout: post
date: '2021-04-06 10:50:00 -0000'
categories: algo
---
#### Two Ways to Initialize an Array with Default Value

`int[] r = new int[2]; r[0] = 1; r[1] = 1` 

`int[] r = new int[] {1, 1};` 

#### Java Only Passes by Value

```java
ßint a = 1;
r[0] = a;
a = 2;
r[0] == ?
```

Here r[0] is 1. Because array r is an object and its values are stored in the heap. when r[0] = a, r[0] copies a's value to its space in heap. Unlikely, a is a primary type and is stored in the stack. when 'a' changes its value, r[0] won't change.

If r[0] = object, and we change the object's **property**, r[0] 's object's property get changed as well. If the object changes to point to another new object (changes its address). r[0] still saves the old object. Because r[0] saves the old object's address. Here is another example, say we have a` ListNode`. We pass the `ListNode node`as an input parameter of a method.  In the method, if we change node as `node = ohter node` , after the method call, the node won't get changed. If we change node as `node.val = newVal`, after running the method, the node will be updated.

#### 2D array empty check

`if(A == null || A.length == 0 || A[0] == null || A[0].length == 0)`

#### Replace Multiplication and Division with Addition and Decrease

If we can replace the operation of multiplication and division with addition and decrease, we shall do it.

#### Modifying input parameters during a method call

Here are 2 different ways to modify a parameter during a method call,

traverse(...,depth++, ...)

traverse(..., depth + 1, ....)

Two ways will get 2 different results of depth. The program will calculate the input parameter's result , and then call the function. Based on this rule, we can see what have happened for the 2 calls.

- traverse(...,depth++, ...)

  Here, we will calculate the depth value, as depth = depth + 1, then we call traverse() method using the new depth. After the traverse call, depth will still be the updated value.

- traverse(..., depth + 1, ....)

  Here we will calculate the depth + 1 as an anonymous variable, then we call traverse() method using the anonymous variable's value. After the method call, the anonymous variable's value is wiped out because it is saved outside the method call. Therefore, depth will still be the not updated value. 

#### Smallest number of integer and double

```
Integer.MIN_VALUE: negative smallest

Double.MIN_VALUE: a small positive number

-Double.MIN_VALUE: negative smallest
```

#### Set often used functions

Set has a  `containsAll()` function.

set `remove(int i)` function's i is the element's value not its index.

#### Foreach Loop Parameter can not be Null

The list that we pass into the for each can not be null.

#### Data Serialization

Data serialization is a transfer between object and string.

- Why we need it: 
  - We need to save memory's data out permanently. Data in memory will disappear when computer is out of power. We need a way to save the data into hard disk permanently. When we need it, we can load from hard disk into memory again.
  - When the data is transferred through Internet. We we transfer data between 2 computers, we can not let 2 computers read each other's memory directly. We need to serialize data and transfer between 2 computers. Most commonly used serialization format is Xml and Jason. Jason is actually a hash map. Its format is (string: list/integer/hash map/...). 
- Serialization Data Structure:
  - Array
  - Linked List
  - Hash Map
- Serialization Algorithm Measurement:
  - Compression Ration: the less space or data transferred the better. Facebook's Thrift and Google's ProtoBuf serialization algorithms are designed for faster data transfer and using less space.
  - Readability: How well the developer's can understand the serialized data before compression. For example, how well we can understand a Jason data.
- How to serialize a binary tree:
  - BFS
  - DFS(pre-order, in-order, post-order traversal).
  - Any method, as long as the serialized data and de-serialized data keep the same.

#### Boundary Check

Before you visit a linked list node's attribute, you shall check the node is null or not.

Before you visit a tree node's attribute, you shall check the tree node is null or not.

Before you visit an element in array, you shall check its index is out of bound or not.

#### Remove Duplicates Trick Among Consecutive Duplicates 

Comparing the duplicated element with the first element: We need to let the pointer moves first then remove duplicates. Such way we will save the first duplicated element.

Array:

```java
i++;
while(i > 0 && numbers[i] == numbers[first element]) {
    //    remove the duplciate elements
    i++;
}
```

Comparing the duplicated element with the previous element: We need to let the pointer moves first then remove duplicates. Such way we will save the first duplicated element.

Arrays:

```java
i++;
while(i > 0 && numbers[i] == numbers[i - 1]) {
    //    remove the duplciate elements
    i++;
}
```

Comparing the duplicated element with the next element: We need to remove the duplicates first then let the pointer moves. Such way we will save the last duplicated element.

Array:

```java
while(i < numbers.length - 1 && numbers[i] == numbers[i + 1]) {
    //    skip the duplciate elements
    i++;
}
i++;
```



#### Create an Empty Array

We can create an empty array with size 0 as following, `int[] result = new int[0]`

#### Recursion Base Case and Division

- Splitting an input into **2** parts each time

  The recursion base case will be `start == end`, the method will return when there is only one element left. We will divide the elements into `[start, mid]` and `[mid + 1, end]`

  Example is [Merge Sort](./Sort.md)

- Splitting an input into **3** parts each time

  The recursion base case will be `start > end`, the method will return when there is no element left. We will divide the elements into `[start, mid - 1]`, `mid`, and `[mid + 1, end]`.

  Example is [106. Convert Sorted List to Binary Search Tree](./BinarySearchTree.md)

- Why when we split the input into 2 parts, the base case will be  `start == end`, but when we split the input into 3 parts, the base case will be `start > end`?

  - In math, the base case is a truth or theorem that we don't need to proof. In a recursion method, the base case is often the situation we can get the result without running the method. 
  - If we divide the input into 3 parts, the smallest recursion part will be `null | mid | null`. The null situation is  when `start > end`, which is the base case. 
  - If we divide the input into 2 parts, the smallest recursion part will be `start | mid` or `mid + 1 | end`. The one element situation is when `start == end` ,  which is the base case. If we continue to split the one element, for example, split to `start | null` or  `null | start`, the splitting will never stop. There is always one element not nullable. 

#### String `compareTo` Function

String's `compareTo` function will compare two string sequentially and lexicographically. Here are some examples,

`"a".compareTo("b")` return -1.  `'a' - 'b'`

`"a".compareTo("d")` return -3. `'a' - 'd'`

`"ab".compareTo("ac")` return -1. `'b' - 'c'`

`"abc".compareTo("def")` return -3. `'a' - 'd'`

`"abc".compareTo("ab")` return 1 length difference is 1



#### Calculate Modulus of Any Positive Or Negative Integer Without Overflow

`a % b = (a % b + b) % b `

Usually in case we get a modulus as a negative number, we will do as  `(a + b) % b`. However, if `a > - b `, we will still get a negative modules. Therefore, we will let `a % b` to make sure the negative modulus is less than b and apply the same operation  as `(a % b + b) % b `

#### Modify Linked List Node

There are two ways to modify a linked list node.

- Modify the linked list node reference.  

  eg, 

  ```
  ListNode cur = head;
  cur = null;
  Here head is not changed. ListNode cur is changed from head to null.
  ```

  

- Modify the linked list node's properties.

  eg,

  ```
  ListNode cur = head;
  head.val = -1;
  Here cur and head's value both update to -1.
  ```

#### Update an Object as a Property of Another Object(Owner) 

Common scenarios of an object being a property of another owner object are,

- An array saves linked list node as its element
- A map saves a list as its values

When we want to update those elements, there are two cases,

- Those elements are not initialized. Here we need to create the object and update owner object's property, which are array or map, to point to the new created object.
- Those elements are initialized. We can directly update the object without touching owner object.

#### String and Char Array Transformation

String to Char Array : `char[] chars = str.toCharArray()`

Char Array to String: `String str = new String(char[] chars)`

#### Remove A Collection's Elements During Using Its Iterator

Collections like map, set etc., all have iterators. When we use an iterator of a class, we can only remove elements thread-safely using `iterator.remove()`. We can not thread-safely use collection's `collection.remove()` function. Especially, when we are using `foreach` to traverse a collection, the iterator is anonymous class. We cannot get its iterator, which also means, we can not remove elements from collections.

#### Remove all Punctuations(Not Including Space) in a String 

`str.replaceAll("[^a-zA-Z ]", "")`

#### Split a String by Whitespaces

`String[] strs = s.split("\\s+")`
