---
published: true
layout: post
date: '2020-01-28 10:50:00 -0000'
categories: hash table
---
A hash table is a data structure that is used to store key-value pairs. It uses a hash function to compute an index into an array in which an element will be inserted or searched. By using a good hash function, hashing can work well. Its **advantage** over other data structures is the average **search** time for an element is **`O(1)`**.

- Time Complexity of Operations:

  - Insert: `O(key size) , which is O(1)`
  - Delete:`O(key size) , which is O(1)`
  - Find: `O(key size) , which is O(1)`

- Java's Hash Table:

  - `HashSet`: it is an extension from `HashMap`. It only has key but no value. It is not thread safe, and often used in removing duplicates.
  - `HashMap`: It saves key and value pairs. It is invented later than `HashTable` and more often used. It is not thread safe either. If multiple threads use a shared `HashMap`, the `hashMap` can screw up a process.
  - `HashTable`: it is the first invented map data structure. It is thread safe. Multiple threads can share a hash table. Since `HashTable`needs to lock and unlock each thread,  its efficiency is lower than `HashMap`.

- Hash function:

  - It is a function to calculate arbitrary key to an index (a fixed integer from 0 to capacity - 1). Hash table is like a huge array, hash function is used to calculate a key into an index so that we can access that element in constant time.  Most of time, key is a string, which is also known as a char array, because all other types can be serialized into a string. 

  - A good hash function shall avoid as much collision as possible.(不同string计算得到的hash value 越乱，越没有规律越好。)

  - Famous Hash function:

    - If the input is a simple char, we can use its Unicode as a hash function, as long as the table is size bigger than 255 bytes, it won't have collision.

    - If the input is a string, to avoid high frequent collision, and to produce unique hashed index, we will do the following, 

      - If we just sum up the chars inside a string, there will be high collisions when the letters are the same, but the order are different. For example, `stop` and `pots` will share the same hash value. Therefore, to make order matters, we shall use at least 26-base. Such way, `stop` will be `18 19 14 15` and `pots` will be `15 14 19 18`.  Such way, as long as the position are different, even with the same chars, the hash value will be different. What is more, there are more characters than 26. The experience has shown that 31-base or 33-base will have more possibilities to produce unique number. In java, it uses 33-base. 

      - If the sum is overflow, Java will cut off the overflow part automatically. If the sum are overflow, we might have many same hash value with cut-off. To avoid such thing happen, when we are calculating the final 31-base number, we will do modulo for every sum-up.

      - Java's basic implementation :(变成33进制后，取模)

        ```
        int hasfunction(String key) {
        
        	int sum = 0;
        	for(int i = 0; i < key.length(); i++) {
        		sum = sum * 33 + (int)(key.charAt(i));
        		sum = sum % HASH_TABLE_SIZE;
        	}
        	return sum;
        }
        ```

        

    - MD5, SHA1, SHA2: Its calculation is complex and mostly used for cryptographic.

- Two Ways to Avoid Collisions:

  - Open Hash Table: (拉链法) If there is a collision, save the collided elements into a list. If we need to look for an element, we shall traverse the linked list. It is similar to checkout at supermarket and you only want to line up for the line with lottery service. Even the line's queue is very long, you can not change to other lines. You can only wait for your turn.

  - Closed Hash Table: 

    - If there is a collision, we will look for its neighbor until we find an empty spot. It is  It is similar to checkout at supermarket and you will go to the first available cashier. If another person goes to your aimed cashier, you will go to the next available cashier and the people after you will go to the cashier next cashier after yours.

    - One thing we need to pay attention is, if we delete an element, we need to mark that spot differently from just empty . That is because even the element is deleted, it can still influence the collided element saving after it and before it is deleted. 

      For example, if we have 2 collided elements `0` and `31`. `0` is saved at index 0 and `31` is saved at index 1. When we delete `0`, we need to mark index 0 position as available after deleted, not just as available. Because when look for `31` after `0` is deleted, we will start from index 0 and stop the search at the first available spot, which shall be index 1. If we did not mark the index 0 as available after deleted, we will stop at index 0 and miss the search for index 1.

- Rehashing:

  If we allocate a huge space to a hash table, it might waste the space. If we allocate a small space to a hash table, the average look-up time will become very long. Therefore, we will make a trade off by increasing the hash table size as needs. However, every time we increase the size of hash table, we need a rehashing no matter which way we are using to avoid collisions. Industrial standard is if the usage of a hash table is more than 10%, the table needs to be rehashed. 

- Hash Table Rebuild:

  Since hash table will only increase but not decrease, after a while of add and delete operation, the table can become very big. Therefore, we will destroy the hash table once a while and rebuild it to save some space.

  

### Problems:

##### Hash Table Basic Operation:

###### [129. Rehashing](https://www.lintcode.com/problem/rehashing/description)

Description:

The size of the hash table is not determinate at the very beginning. If the total size of keys is too large (e.g. size >= capacity / 10), we should double the size of the hash table and rehash every keys. Say you have a hash table looks like below:

```
size=3`, `capacity=4
[null, 21, 14, null]
       ↓    ↓
       9   null
       ↓
      null
```

The hash function is:

```
int hashcode(int key, int capacity) {
    return key % capacity;
}
```

here we have three numbers, 9, 14 and 21, where 21 and 9 share the same position as they all have the same hashcode 1 (21 % 4 = 9 % 4 = 1). We store them in the hash table by linked list.

rehashing this hash table, double the capacity, you will get:

```
size=3`, `capacity=8
index:   0    1    2    3     4    5    6   7
hash : [null, 9, null, null, null, 21, 14, null]
```

Given the original hash table, return the new hash table after rehashing .

Example:

```
Input : [null, 21->9->null, 14->null, null]
Output : [null, 9->null, null, null, null, 21->null, 14->null, null]
```

Notice:

For negative integer in hash table, the position can be calculated as follow:

- C++/Java: if you directly calculate -4 % 3 you will get -1. You can use function: a % b = (a % b + b) % b to make it is a non negative integer.
- Python: you can directly use -1 % 3, you will get 2 automatically.

Analysis:

This question shows a way of how a hash table can rehash when its usage is bigger than 10%  capacity. We will traverse all `not null `nodes in the old hash table, and rearrange those nodes into the bigger hash table. The difficult of this question is how to manipulate an array which contains linked list node as its elements. 

[Calculate Modulus of Any Positive Or Negative Integer Without Overflow](./CleanCodePractice.md)

[Modify Linked List Node](./CleanCodePractice.md)

[Update an Object as a Property of Another Object(Owner)](./CleanCodePractice.md)

```java
public ListNode[] rehashing(ListNode[] hashTable) {

    int newSize = hashTable.length * 2;
    ListNode[] newTable = new ListNode[newSize];

    for(ListNode node : hashTable) {

        if(node != null) {
            ListNode cur = node;
            while(cur != null) {

                int newKey = (cur.val % newSize + newSize) % newSize;
                ListNode newNode = new ListNode(cur.val);
                ListNode newCur = newTable[newKey];

                while(newCur != null && newCur.next != null) {
                    newCur = newCur.next;
                }

                if( newCur == null) {
                    newCur = newNode;
                    newTable[newKey] = newCur;

                }else {
                    newCur.next = newNode;
                }

                cur = cur.next;
            }

        }
    }

    return newTable;

}
```

##### Hash Table Application:

Hash table is often used as one step in one algorithm.  It is usually used for looking up some values by keys. When we build the table, we often have to consider 2 situations, the key exists in the table, and the key doesn't exist in the table.

###### [644. Strobogrammatic Number](https://www.lintcode.com/problem/strobogrammatic-number/solution)

Description:

A mirror number is a number that looks the same when rotated 180 degrees (looked at upside down).For example, the numbers "69", "88", and "818" are all mirror numbers.

Write a function to determine if a number is mirror. The number is represented as a string.

Example 1:

```
Input : "69"
Output : true
```

Example 2:

```
Input : "68"
Output : false
```

Analysis:

This questions needs to translate each integer composing the number to its mirror integer and compare the original number to the new composed number in reverse order. Here we need to find an integer's mirror number. We will use hash map  to save data as (integer, integer mirror number) .

###### [487. Name Deduplication](https://www.lintcode.com/problem/name-deduplication/description)

Description:

Given a list of names, remove the duplicate names. Two name will be treated as the same name if they are equal ignore the case.

Return a list of names without duplication, all names should be in lowercase, and keep the order in the original list.

Example 1:

```
Input:["James", "james", "Bill Gates", "bill Gates", "Hello World", "HELLO WORLD", "Helloworld"]


Output:["james", "bill gates", "hello world", "helloworld"]
```

Example 2:

```
Input:["cmy","Cmy"]

Output:["cmy"]
```

Notice:

You can assume that the name contains only uppercase and lowercase letters and spaces.

Analysis:

Set is a data structure that has no duplications. We can put all elements into a set to remove all duplications and output the set as a list.

```java
public List<String> nameDeduplication(String[] names) {


    Set<String> set = new HashSet<>();

    for(String na : names) {
        if(!set.contains(na.toLowerCase())) {
            set.add(na.toLowerCase());
        }
    }

    List<String> result = new ArrayList<>(set);

    return result;


}
```

###### [171. Anagrams](lintcode.com/problem/anagrams/description)

Description:

Given an array of strings, return all groups of strings that are anagrams.

Example 1:

```
Input:["lint", "intl", "inlt", "code"]
Output:["lint", "inlt", "intl"]
```

Example 2:

```
Input:["ab", "ba", "cd", "dc", "e"]
Output: ["ab", "ba", "cd", "dc"]
```

What is Anagram?

​	Two strings are anagram if they can be the same after change the order of characters.

Notice:

All inputs will be in lower-case.

Analysis:

This question needs to classify strings with same characters. We can sort each word in alphabetical order and save all words with same sorted alphabetical order into a list.  To place each word into its corresponding list, we need to find the list. Here we can use hash map to save data as  (word in alphabetical order, list of words) .

[Update an Object as a Property of Another Object(Owner)](./CleanCodePractice.md)

[String and Char Array Transformation](./CleanCodePractice.md)

```java
public List<String> anagrams(String[] strs) {

    List<String> result = new ArrayList<>();

    if(strs == null || strs.length == 0) {
        return result;
    }

    Map<String, List<String>> map = new HashMap<>();
    for(String s : strs) {

        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);

        if(!map.containsKey(key)) {
            List<String> values = new ArrayList<>();
            values.add(s);
            map.put(key, values);
        }else {
            List<String> values = map.get(key);
            values.add(s);
        }

    }

    for(String key : map.keySet()) {

        List<String> values = map.get(key); 
        if(values.size() > 1) {
            result.addAll(values);
        }
    }

    return result;
}
```

###### [56. Two Sum](https://www.lintcode.com/problem/two-sum/description)

Description:

Given an array of integers, find two numbers such that they add up to a specific target number.

The function `twoSum` should return *indices* of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are zero-based.

```
Example1:
numbers=[2, 7, 11, 15], target=9
return [0, 1]
Example2:
numbers=[15, 2, 7, 11], target=9
return [1, 2]
```

Notice:

O(n) Space, O(n) Time

You may assume that each input would have exactly one solution

Analysis:

If traverse each element in the array, we want to find whether the `target - element` is in the array as well.  If we can save all elements in a hash table, we can find an element in `O(1)`.  What is more, since the question is looking for index, we have to save the index with each element as well. Here we will use hash map to save data as (number[i], i).

```java
public int[] twoSum(int[] numbers, int target) {

    Map<Integer, Integer> map = new HashMap<>();

    for(int i = 0; i < numbers.length; i++) {
        if(!map.containsKey(target - numbers[i])) {
            map.put(numbers[i], i);
        }else {
            return new int[]{map.get(target - numbers[i]), i};
        }
    }


    return new int[]{-1 , -1};
}
```

###### [134. LRU Cache](https://www.lintcode.com/problem/lru-cache/description)

Description:

Design and implement a data structure for Least Recently Used (LRU) cache. It should support the following operations: `get` and `set`.

- `get(key)` Get the value (will always be positive) of the key if the key exists in the cache, otherwise return -1.
- `set(key, value)` Set or insert the value if the key is not already present. When the cache reached its capacity, it should invalidate the least recently used item before inserting a new item.

Finally, you need to return the data from each get.

Example1

```
Input:
LRUCache(2)
set(2, 1)
set(1, 1)
get(2)
set(4, 1)
get(1)
get(2)
Output: [1,-1,1]
Explanation：
cache cap is 2，set(2,1)，set(1, 1)，get(2) and return 1，set(4,1) and delete (1,1)，because （1,1）is the least use，get(1) and return -1，get(2) and return 1.
```

Example 2:

```
Input：
LRUCache(1)
set(2, 1)
get(2)
set(3, 2)
get(2)
get(3)
Output：[1,-1,2]
Explanation：
cache cap is 1，set(2,1)，get(2) and return 1，set(3,2) and delete (2,1)，get(2) and return -1，get(3) and return 2.
```

Analysis:

This question uses Hash Table and [LinkedList](./LinkedList.md) basic data structure. The main thought are copied from [labuladong](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/shou-ba-shou-she-ji-shu-ju-jie-gou/lru-suan-fa)

- Background: Least Recently Used (LRU) is a strategy to manage cache. Cache is just a fast storage. it is usually small. When it is full, LRU cache will delete the least recently used cache first.


- The LRU cache data structure shall have following features,

  - The data structure shall order caches by time. When caching space is full, the oldest cache will be deleted.
  - We need to find key and its value in O(1).
  - We shall be able to delete or insert elements in any position of the data structure because when we visited a key, we shall move that key to the most used element.

- Design Ideas:

  Hash table can find elements in O(1), but elements hash no order. Linked list elements have order, and the elements can be deleted or inserted in any position, but its search time is O(n). Therefore, we will combine the two data structure (Java has such data structure as `LinkedHashMap`). 

  Here is how the combined data structure can implement's question's feature,

  - We will add new elements from tail. The head of the linked list will be the least recently used cache element. When the caching space is full, it can be deleted first.
  - We can search any key's value in O(1).
  - Since we use linked list, we can move a node to anywhere by only change the pointer. What is more, not like a simple linked list, we can search any list node in O(1) time as well.

  The combined data structure is as following picture (copied from [labuladong](https://labuladong.gitbook.io/algo/shu-ju-jie-gou-xi-lie/shou-ba-shou-she-ji-shu-ju-jie-gou/lru-suan-fa)) 



![](E:\study\jiuzhang\Notes\lrucache.jpg)

- Implementation 

  1. To implement linked list, we need a `ListNode`class

  2. We need to build a double linked list using  `ListNode` . It is better to abstract `DoubleList` class. Here we will write lazier by writing all properties `ListNode head, LisNode tail, and int size`and methods `addNode(),and removeNode`belonging `DoubleList`in the same class`LRUCache`.

     **Why do we need a double linked list?** Not like a simple linked list which we often delete node at head, we want to be able to remove nodes in any position. To delete a node fast, we need to know any node's previous. Therefore, we will use double linked list here.

  3. Since we need to manage Linked List and Hash table together, it is better to encapsulate another layer interface as Java's interface `LinkedHashMap`. Here we will only create methods `addLast(), moveToLast(), and removeFirst()` in `LRUcache`

     **Data structure as `HashMap(key, ListNode(value))` Vs `HashMap(key, ListNode(key, value))`** : When we remove a node, we need to remove the `(key, ListNode)` pair from hash map. To remove it, we need to get the key value from the node. That is why we need to use data structure as`HashMap(key, ListNode(key, value))`, not  `HashMap(key, ListNode(value))`. 

  4. We can implement get and set use the step 3 methods,

     ```java
     public class LRUCache {
         
       Map<Integer, ListNode> map;
         ListNode head;
       ListNode tail;
         int size;
       
         /*
         * @param capacity: An integer
         */public LRUCache(int capacity) {
             this.size = capacity;
             this.map = new HashMap<>();
             head = new ListNode(-1, -1);
             tail = new ListNode(-1, -1);
             head.next = tail;
             tail.pre = head;
             
         }
     
         /*
          * @param key: An integer
          * @return: An integer
          */
         public int get(int key) {
             if(!map.containsKey(key)) {
                 return -1;
             }
             ListNode node = map.get(key);
             moveToLast(node);
             return node.value;
         }
     
         /*
          * @param key: An integer
          * @param value: An integer
          * @return: nothing
          */
         public void set(int key, int value) {
             
             if(map.containsKey(key)) {
                 ListNode node = map.get(key);
                 node.value = value;
                 moveToLast(node);
             }else {
                 if(map.size() == this.size) {
                     removeFirst();
                 }
                 addLast(key, value);
             }
         }
         
         private void addLast(int key, int value) {
             ListNode newNode = new ListNode(key, value);
             map.put(key, newNode);
           addNode(newNode);
         }
       
         private void moveToLast(ListNode node) {
           removeNode(node);
             addNode(node);
         }
         
         private void removeFirst() {
            map.remove(head.next.key);
            removeNode(head.next);
            
         }
         
         private void addNode(ListNode newNode) {
             newNode.next = tail;
             tail.pre.next = newNode;
             newNode.pre = tail.pre;
             tail.pre = newNode;
         
         }
         
         private void removeNode(ListNode node) {
             
             node.pre.next = node.next;
             node.next.pre = node.pre;
             node.next = null;
             node.pre = null;
             
         }
     }
     
     class ListNode {
         int key;
         int value;
         ListNode next;
         ListNode pre;
         
         public ListNode(int key, int value) {
             this.key = key;
             this.value = value;
         }
      }
     ```


###### [124. Longest Consecutive Sequence](https://www.lintcode.com/problem/longest-consecutive-sequence/description)

Description,

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Example :

```
Input : [100, 4, 200, 1, 3, 2]
Output : 4
Explanation : The longest consecutive elements sequence is [1, 2, 3, 4]. Return its length:4
```

Your algorithm should run in O(*n*) complexity.

Analysis:

This question [Leetcode](https://leetcode.com/problems/longest-consecutive-sequence/solution/) offers detailed solution. The brute force way introduces a new way of thinking. We can increase each number until there is no bigger consecutive number. Then we can compare each number's consecutive streak and get the longest. After we came up with the brute force solution, we can optimize the solution by improving search time with hash set. As well, if a number belongs to a longer consecutive sequence and it is not the smallest number, we will skip it.  Thus our algorithm takes worst `O(2n)`  time.

There are other solutions as well. We can delete not used numbers from hash set to minimize the search range. Here we won't discuss other solutions in detail. One thing we need to pay attention is removing elements during iteration of hash set.

[Remove A Collection's Elements During Using Its Iterator](./CleanCodePractice.md)

 ![](E:\study\jiuzhang\Notes\LongestConsequtiveSequence.JPG)

```java
    public int longestConsecutive(int[] num) {
        
        Set<Integer> set = new HashSet<>();
        
        for(int n : num) {
            set.add(n);
        }
        
        int result = 0;
        for(int n : num) {
            
            if(!set.contains(n - 1)) {
                
                int current = n;
                int streak = 1;
                
                while(set.contains(current + 1)) {
                    
                    streak++;
                    current++;
                }
                
                if(streak > result) {
                    result = streak;
                }
            }
            
        }
        
        return result;
    }
```
