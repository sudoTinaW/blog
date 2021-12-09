---
published: true
layout: post
date: '2021-01-28 10:50:00 -0000'
categories: algo
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

#### Hash Table Basic Operation:

#### [129. Rehashing](https://www.lintcode.com/problem/rehashing/description)

#### Hash Table Application:

Hash table is often used to look-up elements in `O(1)` time. It is a data structure and often used with other algorithms. Depends on different situations, there are two ways to save the elements into the hash table. **First is saving all elements into the hash table all together and looking up the table. Second is looking up already saved elements from hash table before saving a new element.** Since elements in hash table don't have order or duplicates, we can only use the first way when the element's order or duplicate doesn't matter. Otherwise, we will use the second way. Most of times, if a question can use the first way, it can also use the second way. However, if a question can use the second way, it cannot usually use the first way. **Therefore, we will use the second way as much as possible**

#### [1369 Most Common Word](https://www.lintcode.com/problem/1369/)


#### [644. Strobogrammatic Number](https://www.lintcode.com/problem/strobogrammatic-number/solution)

#### [487. Name Deduplication](https://www.lintcode.com/problem/name-deduplication/description)

#### [171. Anagrams](lintcode.com/problem/anagrams/description)

#### [56. Two Sum](https://www.lintcode.com/problem/two-sum/description)

#### [134. LRU Cache](https://www.lintcode.com/problem/lru-cache/description)


#### [124. Longest Consecutive Sequence](https://www.lintcode.com/problem/longest-consecutive-sequence/description)
