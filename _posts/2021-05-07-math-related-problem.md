---
published: false
layout: post
date: '2021-05-07 14:50:00 -0000'
categories: Math
---
Pure math related problems are rarely asked during an interview. That is because an interview is usually testing an interviewee's problem solving skills. Most math problems are based on some theorems, and those theorems are hard to infer in a short time. Therefore, it is hard to show problem solving skills by resolving a pure math problem. In this article, we will focus on introducing some common used theorems to inspire sparks. 

Original thought are inspired by [labuladong](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/shu-xue-yun-suan-ji-qiao)

#### Bit Operation:

Most often used bit operations are `or`, `and`, `xor`. There are many tricky and interesting skills. Here we  will only focus on two of them.

- `n &(n - 1)` : This operation can remove the last bit of `1` in `n`.
- `xor` operation : `a ^ a = 0` `a ^ 0 = a`

The prove will not be shown here, but you can use examples to infer theorems.

#### Factorial Calculation:

Factorial problems usually are not resolved by actual factorial calculation because factorial calculation's time complexity is very big. We often need to find patterns to get result without a complete factorial computation .

### Problems:

#### Bit Operation:

###### [1332. Number of 1 Bits](https://www.lintcode.com/problem/1332/)

Description:

Write a function that takes an unsigned integer and returns the number of ’1' bits the corresponding binary number has (also known as the Hamming weight).

Example 1:

```
Input：n = 11
Output：3
Explanation：11 in binary is 1011, so return 3
```

Example 2:

```
Input：n = 7
Output：3
Explanation：7 in binary is 111, so return 3
```

Analysis:

Here we will use `n & (n - 1)` operation to remove 1 in the `n` until it becomes 0, which is also known as no more 1s in n.

```java
public int hammingWeight(int n) {
    int total = 0;
    while(n != 0) {
        total++;
        n = n & (n - 1);
    }

    return total;
}
```

###### [142. O(1) Check Power of 2](https://www.lintcode.com/problem/142/)

Description:

Using O(*1*) time to check whether an integer *n* is a power of `2`.

```
Example 1:
	Input: 4
	Output: true


Example 2:
	Input:  5
	Output: false
```

Analysis:

First we need to observe any power of 2 number only has one `1` in the binary format, such as following,

`1 is 1`

`2 is 10` 

`4 is 100`

`8 is 1000`

If a number removes `1` once and becomes 0, we can tell make sure the number is power of 2. 

```java
public boolean checkPowerOf2(int n) {

    if(n <= 0) {
        return false;
    }

    return (n & (n - 1)) == 0;
}
```

###### [82. Single Number](https://www.lintcode.com/problem/82/)

Description:

Given `2 * n + 1` numbers, every numbers occurs twice except one, find it.

Example 1:

```
Input：[1,1,2,2,3,4,4]
Output：3
Explanation:
Only 3 appears once
```

Example 2:

```
Input：[0,0,1]
Output：1
Explanation:
Only 1 appears once
```

Analysis:

This question is easy if we can use `xor` operation. First we need to know, `a ^ (b ^ c) <=> (a ^ b) ^ c`. Secondly, we know that if an element appears twice, its `a ^ a = 0`. All pairs with the same value's `xor` result will be `0`. The single number `xor` with `0` will be itself, which is `a ^ 0 = a`. If we can `xor` all numbers, the final result will be the single number.

```java
public int singleNumber(int[] A) {

    int result = 0;

    for(int a : A) {
        result = result ^ a;
    }

    return result;
}
```

#### Factorial Calculation:

###### [(LeetCode) 172 Factorial Trailing Zeroes](https://leetcode.com/problems/factorial-trailing-zeroes/)

Description:

Given an integer `n`, return *the number of trailing zeroes in `n!`*.

Follow up: Could you write a solution that works in logarithmic time complexity?

 

Example 1:

```
Input: n = 3Output: 0Explanation: 3! = 6, no trailing zero.
```

Example 2:

```
Input: n = 5Output: 1Explanation: 5! = 120, one trailing zero.
```

Example 3:

```
Input: n = 0Output: 0
```

Constraints:

- `0 <= n <= 104`

Analysis:

The trailing zeros are generated by computation of `2 * 5`.  The number of trailing zeroes are decided by how many pairs of 2 and 5 factor can be derived from a number. 

For example,

`11 ! = 1 * 2 * 3 * (2 * 2) * 5 * (2 * 3) * 7 * (2 * 2 * 2) * 9 * (2 * 5) * 11`

Here we can see factor `5` appears at least once every 5 numbers and factor`2` appears at least once every 2 numbers. Therefore, how many pairs of factors `2 and 5  ` there are, are determined by how many factor `5` there are in a number's factorial.

Here we need to find the patterns of how many factor` 5 `a number's factorial has.



```
5 = 1 * 5, 5! has 1 5-factor10 = 2 * 5,10! has 2 5-factor15 = 3 * 5,15 has 3 5-factor...25 = 5 * 5,   25! has 5 5-factor and 1 (5 * 5)-factor, total is 5 + 1 = 6s 5 factor.125 = (5 * 5 )* 5,  125! has (5 * 5) 5-factor, 5 (5 * 5)-factor, 1(5 * 5 * 5)-factor, total is 25 + 5 + 1 = 31s  5 factor.625 = ((5 * 5) * 5 )* 5,  625! has (5 * 5 * 5) 5-factor, 25 (5 * 5)-factor, 5 (5 * 5 * 5)-factor, 1 (5 * 5 * 5 * 5)-factor 125 + 25 +   5 + 1 = 156s 5 factor.
```

Therefore, the number of factor 5 is the sum of quotients of the number continuously divided by 5.  

```java
public int trailingZeroes(int n) {    int total = 0;    while(n / 5 != 0) {        total += n / 5;        n = n / 5;    }    return total;}
```



###### [(Leetcode)793 Preimage Size of Factorial Zeroes Function](https://leetcode.com/problems/preimage-size-of-factorial-zeroes-function/)

Description:

Let `f(x)` be the number of zeroes at the end of `x!`. (Recall that `x! = 1 * 2 * 3 * ... * x`, and by convention, `0! = 1`.)

For example, `f(3) = 0` because 3! = 6 has no zeroes at the end, while `f(11) = 2` because 11! = 39916800 has 2 zeroes at the end. Given `K`, find how many non-negative integers `x` have the property that `f(x) = K`.

```
Example 1:Input: K = 0Output: 5Explanation: 0!, 1!, 2!, 3!, and 4! end with K = 0 zeroes.Example 2:Input: K = 5Output: 0Explanation: There is no x such that x! ends in K = 5 zeroes.
```

Note:

- `K` will be an integer in the range `[0, 10^9]`.

Analysis:

This question is an extension of [(LeetCode) 172 Factorial Trailing Zeroes](#(LeetCode) 172 Factorial Trailing Zeroes). From the following picture we can tell,

![](/Users/tina/Documents/jiuzhang/Notes/preimageSizeOfFactorialZeroesFunction.png)

Every 5 number will have the same number of tailing zeroes. For example, number 0 to 4 have 0 tailing zeroes, number 5 to 9 have 1 tailing zero, and number 25 to 29 have 6 tailing zeroes. That is to say, the number of integers x having the property that `f(x) = K` is either be 0 (K non-exist like 5) or 5 (K exist like 4 or 6). Now the quesiton can transferred to whether we can find a number, whose factorial trailing zeroes equal to K. If such K exist, our function returns 5. If such K does not exist, our function returns 0. If we enumrate all possible numbers from 0 to ` 5 * (K + 1)` as imput of [(LeetCode) 172 Factorial Trailing Zeroes](#(LeetCode) 172 Factorial Trailing Zeroes),  we can tell whether there is a number's factorial trailing zeroes equal to K. Since the result of all number's factorial trailing zeroes are increasing (in order) as number is growing. We can use binary search to find a number, whose factorial trailing zeroes equal to K.

`K` will be an integer in the range `[0, 10^9]`. Our search range shall be `[0, 5 * (10 ^ 9 + 1)]`, we can maxmize it to `[0, 10 ^ 10]`. The integer 's range will only be around ` 2 * 10 ^ 9`. Integer type is not enough for the search range's max boundary. We will use data type `long`.



```java
class Solution {    public int preimageSizeFZF(int K) {                long start = 0;        long end = Long.MAX_VALUE;                while(start + 1 < end) {                        long mid = start + (end - start) / 2;            long numOfZeroes = trailingZeroes(mid);                        if(numOfZeroes == K) {                return 5;            }            if(numOfZeroes > K ) {                end = mid;            }else if(numOfZeroes < K) {                start = mid;            }        }                if(trailingZeroes(start) == K || trailingZeroes(end) == K) {            return 5;        }         return 0;    }        private long trailingZeroes(long num) {                long result = 0;                while(num != 0) {            num /= 5;            result += num;                    }                return result;    }}
```

#### [1324  Count Primes](https://www.lintcode.com/problem/1324/)

Description:

Count the number of prime numbers less than a non-negative number, n.

Example

Example 1：

```
Input: n = 2Output: 0
```

Example 2：

```
Input: n = 4Output: 2Explanation：2, 3 are prime number
```

Analysis:

First the easiest way to solve this question is to check whether all numbers from 0 to n are prime . To find whether a number is a prime, we need to test whether this number can be exactly divided by any number from 2 to `this number - 1`. The time complexity will be `O(n)`.

If we only judge whether a number is a prime or not, we can let the number mod all numbers in range `[2, number)`, and any number in range `[2, number)` will be divided once. However, this question need to count the number of all prime numbers, any number in range [2, number] will be divided n times. As we know any two number in range `[2, number)` can only compose one product. If we can save those product and use it in some way, we can decrease the time complexity.

Therefore, we can calculate all possible composites of any two numbers in range `[2, number)`. The rest numbers in range `[2, number)`and not composites are primes.

Here we can optimize even more. For example, when we find a number 16 is a composite, we can get it by `2 * 8`, and also by `8 *2`. If we try to calculate the product of any two numbers in range `[2, number)`, there must be a duplication as above. Thus, if rule out that the first number must be smaller or equal to the second number, we can avoid more duplicated calculations. That is our first operator's range shall be in `[2, squrt(number)]`. What is more, our second operator's range shall be [first operator, `number / first operator`].

Time complexity  for this question is `O(n * loglogn)`. Proof is complexed, we only need to know the time complexity is between `O(n) and O(logn)`

```java
public int countPrimes(int n) {  boolean[] isComposite = new boolean[n];  for(int i = 2; i * i < n; i++) {    if(!isComposite[i]) {      for(int j = i; j * i < n; j++) {        isComposite[j * i] = true;      }    }  }  int count = 0;  for(int i = 2; i < n; i++) {    if(!isComposite[i]) {      count++;    }  }  return count;}
```

#### [1275 Super Pow](https://www.lintcode.com/problem/super-pow/)

Description:

Your task is to calculate a^b mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.

The length of b is in range [1, 1100]

Example1:

```
Input:a = 2b = [3]Output:8
```

Example2:

```
Input:a = 2b = [1,0]Ouput:1024
```

Analysis:

The easiest way to think of this question is to calculate the power from array b and calculate `(a ^ power) mod 1337`. If `a` is very big, for example `Integer_MAX_VALUE`, its power will be overflow.  If array b's length is long (max 32 length for integer, 64 for long, but b's max length cab be 1100), we can not calculate `(a ^ power)`then get its module of 1337. 

We shall do the module operation before the power operation. We can translate the power operation to multiple multiplication operation. Then we can use modulo property`(a * b) % k = ((a % k) * (b % k)) % k` to make each operation is between `[0 , 1337)`. [Common-used Modulo Multiplication Property](.CleanCodePractice.md)

Find the minimal pattern as below,

![img](/Users/tina/Documents/CodeStudy/Notes/superPow.png)

`result = ((result ^ 10) * (a ^ b[i])) % 1337 `

```java
public int superPow(int a, int[] b) {  int result = 1;  for(int i = 0; i < b.length; i++) {    result = ((modPower(result, 10) % 1337) * (modPower(a, b[i]) % 1337)) % 1337;  }  return result;}private int modPower(int base, int power) {  int result = 1;  for(int i = 0; i < power; i++) {    result = ((result % 1337) * (base % 1337)) % 1337;  }  return result;}
```



#### [196  Missing Number](https://www.lintcode.com/problem/196/)

Example 1:

```
Input:[0,1,3]Output:2
```

Example 2:

```
Input:[1,2,3]Output:0
```

Challenge

Do it in-place with O(1) extra memory and O(n) time.

Analysis:

This question we can use 2 ways to solve the question in place and in O(n) time.

First we can use `xor`. We can let all numbers `xor` with all numbers in range `[0, n]`. There will be `n - 1` pair becomes `0`. The only left number will be the signal number which is the missing number.

```java
public int findMissing(int[] nums) {  int n = nums.length;  int missing = 0;  for (int i = 0; i < n ; i++) {    missing ^= nums[i] ^ i;  }  return missing ^ n;}
```

Second we can sum up all numbers in array, and sum all numbers in [0, n]. The difference between the two sums will be the missing number. This way, we need multiplication and addition first, and then do a subtraction. There is a chance when we do multiplication and addition, the result will be overflow though the final result with subtraction will be in the range. It is better to to subtraction before multiplication or addition to avoid overflow. 

```java
public int findMissing(int[] nums) {  int result = 0;  for(int i = 0; i < nums.length; i++) {    result += i - nums[i];  }  result += nums.length;  return result;}
```

#### [1112  Set Mismatch](https://www.lintcode.com/problem/1112/)

Description

The set `S` originally contains numbers from 1 to `n`. But unfortunately, due to the data error, one of the numbers in the set got duplicated to another number in the set, which results in repetition of one number and loss of another number.

Given an array `nums` representing the data status of this set after the error. Your task is to firstly find the number occurs twice and then find the number that is missing. Return them in the form of an array.

1.The given array size will in the range [2, 10000].
2.The given array's numbers won't have any order.

Example 1:

```
Input: nums = [1,2,2,4]Output: [2,3]Explanation:2 is the number occurs twice, 3 is the missing number.
```

Example 2:

```
Input: nums = [1,3,3,4]Output: [3,2]Explanation:3 is the number occurs twice, 2 is the missing number.
```

Analysis:

If we can use extra space, this question will be very easy. We can use a hash table to remember whether we have visited a number. The hash table can be an array. The index will the number in the array. The content of array can be 1 and 0. When we add elements into the hash array, if its value has been marked, it means this number is duplicate. After we marked all numbers, we can check the missing number from the hash array with is number is not set.

```java
public int[] findErrorNums(int[] nums) {  int[] result = new int[2];  int[] hash = new int[nums.length + 1];  for(int i = 0; i < nums.length; i++) {    if(hash[nums[i]] < 0) {      result[0] = nums[i];      continue;    }    hash[nums[i]] = -nums[i];  }  for(int i = 1; i < hash.length; i++) {    if(hash[i] == 0) {      result[1] = i;      break;    }  }  return result;}
```



If the question can not use extra space, we can mark the original content with negative if it is visited. If a content's corresponding index's content is already marked as negative, we know the content's corresponding index's content is the duplicate. After we marked all numbers, we can find the only positive number's index, the index + 1 will be the missing number.

```java
public int[] findErrorNums(int[] nums) {  int[] result = new int[2];  for(int i = 0; i < nums.length; i++) {    int index = Math.abs(nums[i]) - 1;    if(nums[index] < 0) {      result[0] = Math.abs(nums[i]);    }else {      nums[index] *= -1;    }  }  for(int i = 0; i < nums.length; i++) {    if(nums[i] > 0) {      result[1] = i + 1;      break;    }  }  return result;}
```

#### [(LeetCode) 382 Linked List Random Node](https://leetcode.com/problems/linked-list-random-node/)

Description:

Given a singly linked list, return a random node's value from the linked list. Each node must have the **same probability** of being chosen.

Example 1:

![img](https://assets.leetcode.com/uploads/2021/03/16/getrand-linked-list.jpg)

```
Input["Solution", "getRandom", "getRandom", "getRandom", "getRandom", "getRandom"][[[1, 2, 3]], [], [], [], [], []]Output[null, 1, 3, 2, 2, 3]ExplanationSolution solution = new Solution([1, 2, 3]);solution.getRandom(); // return 1solution.getRandom(); // return 3solution.getRandom(); // return 2solution.getRandom(); // return 2solution.getRandom(); // return 3// getRandom() should return either 1, 2, or 3 randomly. Each element should have equal probability of returning.
```

Constraints:

- The number of nodes in the linked list will be in the range `[1, 104]`.
- `-104 <= Node.val <= 104`
- At most `104` calls will be made to `getRandom`.

 

Follow up:

- What if the linked list is extremely large and its length is unknown to you?
- Could you solve this efficiently without using extra space?

Analysis:

- We can traverse all nodes and get the total length of the list. Then can generate a random number from total length, and traverse again to find that random length's node. This method is not suitable for the list is extremely large and its length is unknown.

- There is a better solution using reservoir sampling. Reservoir sampling is every time when we have a new element joining, we will roll a dice with (former element size + 1), which is also known as generate a random number from 0 to (former elements size + 1). When we add nth elements, every element will have 1/n probability of being chosen.

  Before explaining why this method works, we will first show a similar example. Say we have 5 people (A, B, C, D, E) to win a prize from 5 tickets. Everyone have a (1/5) probability to win. First we let A to chose one ticket, and show the ticket. If A win the prize, the rest of people's win opportunity will be 0. If A doesn't get the prize, B will have 1/4 chance to win. If B doesn't win, C will have 1/3 chance to win. If C doesn't win, D will have 1/2 to win. If D doesn't win, E will win the prize. Such way, each people has different probability to win. The order matters. However, if we let A chose a ticket, and not showing the result. Then we let B choose a ticket. Now A and B will have the same chance to win a ticket. After we have all people chosen ticket, every one will have same probability to win the ticket which is (1/5). Now order doesn't matter.

  Reservoir sampling is similar to the second way to win the price. Every time, we have a new people join, we will reselect a random number, which also means the former people can win or can not win any more, that is also to say, the former people's result is unknown. This way can always make sure all people that we select so far will have the equal opportunity to win. When our sample reached to the total size, every element will have `1/totalSize` chance to win.

  We have another optimization, whenever we have a new element, we will remember its random chosen result. If the new node is chosen,  the random chosen result will be the new node. If the new node is not chosen, the former random chosen result will be the current random chosen result. We will show the process and probability of total size of 5 as below picture,

  From the picture, we can see, every time when a new people join, every former element's probability is changing.

![img](ReserviorSampling.png)



```java
class Solution {    /** @param head The linked list's head.        Note that the head is guaranteed to be not null, so it contains at least one node. */        private ListNode head;        public Solution(ListNode head) {        this.head = head;    }        /** Returns a random node's value. */    public int getRandom() {        Random rand = new Random();        ListNode cur = head;        int result = cur.val;        int count = 1;               while(cur != null) {            if(rand.nextInt(count) == 0) {                result = cur.val;            }            count++;            cur = cur.next;        }                return result;    }}
```
