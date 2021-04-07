---
published: true
layout: post
date: '2021-2-09 14:50:00 -0000'
categories: algo
---
Prefix sum is a simple yet powerful technique that allows to perform fast calculation on the sum of elements in a given range(called contagious segments of array). It often resolves questions relating to array interval.

- What is prefix sum?

  `preSum[i]` is `num[0...i]` 's sum. Here is the example,

  ![preSum](/asset/PreSum.JPG)

- How to implement it?

  ```java
  int[] sum = new int[nums.length + 1];
  sum[0] = 0;
  for(int i = 0; i < nums.length; i++) {
      sum[i + 1] = sum[i] + nums[i];
  }
  ```

- How to use it?

  **The sum of range `[i, j]` is `sum[j + 1] - sum[i]`** For example using the above example, 

  - The sum of index range [0, 4] is `sum[5] = 10`

- The sum of index range [0, 6] is `sum[7]= 5` 

  - The sum of index range[2, 6] = sum of index range [0, 6] - sum of index range [0, 1] = `sum[7] - sum[2] = 5 - 9 = -4` 

- Common Techniques Used with Prefix Sum

  When we use prefix sum array, we only need `j > i` situation. When we calculate all different combinations of `sum[j] - sum[i]`, we don't need to consider the situation of `j <= i`. Therefore, we often loop through each `j` and only check `i` before `j`'s combinations.

- Time Complexity:

  To calculate prefix sum array time is `O(n)`, and to calculate sum of a range of array is `O(1)`

### Problems:

####  [138. Subarray Sum](https://www.lintcode.com/problem/subarray-sum/description)

Description:

Given an integer array, find a subarray where the sum of numbers is **zero**. Your code should return the index of the first number and the index of the last number.

Example 1:

```
Input:  [-3, 1, 2, -3, 4]
Output: [0, 2] or [1, 3].
Explanation: return anyone that the sum is 0.
```

There is at least one subarray that it's sum equals to zero.

Analysis:

The question is asking about subarray sum. We shall think of using prefix sum technique. After we get prefix sum, we can get sum of `range [i, j]` by `sum[j + 1] - sum[i]`, `i and j` are index of array `nums`. The question requires the sum shall be 0, which means we need to find all `[i, j]`, to make `sum[j + 1] - sum[i] = 0` , which also means, find all `[i. j]` to make  `sum[j + 1] == sum[i]`. 

Now we can translate the question to find two elements with the same value in the subarray. Here we can use hash table to quickly find a pair of duplicate elements. Obviously, we need to use the [second way](./HashTable.md) to manipulate hash table. 

```java
public List<Integer> subarraySum(int[] nums) {

    List<Integer> result = new ArrayList<>();

    if(nums == null || nums.length == 0) {
        return result;
    }

    // Build prefix sum array
    int[] sum = new int[nums.length + 1];
    sum[0] = 0;

    for(int i = 0; i < nums.length; i++) {
        sum[i + 1] = sum[i] + nums[i];
    }


    Map<Integer, Integer> map = new HashMap<>();

    for(int i = 0; i < sum.length; i++) {
        if(map.containsKey(sum[i])) {
            result.add(map.get(sum[i]));
            result.add(i - 1);
        }
        map.put(sum[i], i);
    }

    return result;
}
```

#### [838. Subarray Sum Equals K](https://www.lintcode.com/problem/838/)

Description:

Given an array of integers and an integer k, you need to find the total number of continuous subarrays whose sum equals to k.

Example1

```
Input: nums = [1,1,1] and k = 2
Output: 2
Explanation:
subarray [0,1] and [1,2]
```

Example2

```
Input: nums = [2,1,-1,1,2] and k = 3
Output: 4
Explanation:
subarray [0,1], [1,4], [0,3] and [3,4]
```

Analysis:

Since we need to get subarray sum, we shall try to use prefix sum first. After we get prefix sum array, we need to find all 2 elements `sum[i], sum[j]` to make `sum[j] - sum[i] == k`.  Here, `i and j` have order which is `j > i`. When we build and look up hash table, we usually loop through ` j `and find `i`. Therefore, we will loop each `j` to check whether `sum[i]`, which is `sum[j] - k`, exists or not.

This question needs to find all possible solutions. If there are more than 2 duplicated elements in the sum array, we need to collect all possible duplicated composed answers. If we manipulate hash table as  [second way](./HashTable.md), we can only find duplicated elements twice maximum (`i and j`). Therefore, we need to remember each number's frequency and update the final result by its frequency(duplicated times). 

```java
public int subarraySumEqualsK(int[] nums, int k) {

    int[] sum = new int[nums.length + 1];

    sum[0] = 0;

    for(int i = 0; i < nums.length; i++) {
        sum[i + 1] = sum[i] + nums[i];
    }

    int result = 0;
    Map<Integer, Integer> map = new HashMap<>();

    for(int s : sum) {

        if(map.containsKey(s - k)) {
            result += map.get(s - k);
        }

        if(map.containsKey(s)) {
            map.put(s, map.get(s) + 1);
        }else {
            map.put(s, 1);
        }


    }

    return result;
}
```

#### [41. Maximum Subarray(leetcode)](https://leetcode.com/problems/maximum-subarray/)

Description:

Given an array of integers, find a contiguous subarray which has the largest sum.

Example1:

```
Input: [−2,2,−3,4,−1,2,1,−5,3]
Output: 6
Explanation: the contiguous subarray [4,−1,2,1] has the largest sum = 6.
```

Example2:

```
Input: [1,2,3,4]
Output: 10
Explanation: the contiguous subarray [1,2,3,4] has the largest sum = 10.
```

Can you do it in time complexity O(n)?

The subarray should contain at least one number.

Analysis:

This question is asking about contiguous subarray sum. We shall think of using prefix sum. After we get the prefix sum, we need to find the greatest diff of  `sum[j] - sum[i] (j > i)`. To find the minimum value of `sum[j] - sum[i]`, we can compare all diff of `sum[j] - min (the minimum value of sum[i] before j)`, and get the maximum diff.

```java
public int maxSubArray(int[] nums) {

    int[] sum = new int[nums.length + 1];

    sum[0] = 0;

    for(int i = 0; i < nums.length; i++) {
        sum[i + 1] = sum[i] + nums[i];
    }

    int maxDiff = Integer.MIN_VALUE;
    int min = sum[0];

    for(int i = 1; i < sum.length; i++) {

        if(sum[i] - min > maxDiff) {
            maxDiff = sum[i] - min;
        }

        if(sum[i] < min) {
            min = sum[i];
        }
    }

    return maxDiff;
}
```

#### [139. Subarray Sum Closest](https://www.lintcode.com/problem/subarray-sum-closest/description)

Description:

Given an integer array, find a subarray with sum closest to zero. Return the indexes of the first number and last number.

Example:

```
Input: 
[-3,1,1,-3,5] 
Output: 
[0,2]
Explanation: [0,2], [1,3], [1,1], [2,2], [0,4]
```

`O(nlogn)` time

It is guaranteed that the sum of any numbers is in `[-2^{31},2^{31}-1][−231,231−1]`.

Analysis:

This question includes following techniques,

- Prefix sum

  This question is asking about subarray sum, we often think of prefix sum.

- Encapsulating prefix sum's index and value into a Pair object

  After we get the prefix sum, we will sort the prefix sum array. Since we need prefix sum original index to compose the final result, we will encapsulate      the prefix sum and its index into a Pair object.

- Customize Comparator to compare Pair object

  Since the Pair object is not comparable, we need to create a customized Comparator to compare Pair object. Our Customized Comparator will implement `Comparator<Pair>` interface.

- Find any two elements' difference closet to 0. To find two elements' difference closet to a value in a sorted array, we can think of using left and right pointer and sliding window techniques. It is similar to question [610. Two Sum - Difference equals to target](./TwoPointer.md). Here we will find the closet difference of any two qualified left and right pointer to 0. 

  Since this question is looking for any two elements' difference closet to **0**, we can translate the question to find the minimum difference of any two elements from the prefix sum array. After sort, we can find the minimum diff can only exit between two closed elements. Therefore, we can simplify this question to find the minimum diff of any two elements next to each other.

  [The Difference of any Two Elements in a Sorted Array Variation Pattern](#the-difference-of-any-two-elements-in-a-sorted-array-variation-pattern)

  Here we will post generalized and simplified way to find any two elements' difference closet to 0.

  The generalized Way:

  ```java
  public class Solution {
      /*
       * @param nums: A list of integers
       * @return: A list of integers includes the index of the first number and the index of the last number
       */
      public int[] subarraySumClosest(int[] nums) {
          
          int[] result = new int[2];
          
          result[0] = -1;
          result[1] = -1;
          
          int[] sum = new int[nums.length + 1];
          sum[0] = 0;
          for(int i = 0; i < nums.length; i++) {
              sum[i + 1] = sum[i] + nums[i];
          }
          
          Pair[] pairs = new Pair[sum.length];
          for(int i = 0; i < sum.length; i++) {
              pairs[i] = new Pair(sum[i], i);
          }
          
          Arrays.sort(pairs, new MyComparator());
  
          int left = 0;
          int right = 1;
          
          int start = 0;
          int end = 1;
          
          int minDiff = pairs[right].val- pairs[left].val;
          
          while(right < pairs.length) {
              if(left == right) {
                  right++;
                  continue;
              }
              
              if(pairs[right].val - pairs[left].val == 0) {
                  result[0] = Math.min(pairs[right].index, pairs[left].index);
                  result[1] = Math.max(pairs[right].index, pairs[left].index) - 1;
                  return result;
              }
              if(pairs[right].val - pairs[left].val > 0) {
                  if(minDiff > pairs[right].val - pairs[left].val) {
                      minDiff = pairs[right].val - pairs[left].val;
                      start = left;
                      end = right;
                      
                  }
                  left++;
              }else {
                  right++;
              }
              
          }
          
          result[0] = Math.min(pairs[start].index, pairs[end].index);
          result[1] = Math.max(pairs[start].index, pairs[end].index) - 1;
  
          
          return result;
          
          
      }
  }
  
  class Pair {
      int val;
      int index;
      
      Pair(int val, int index) {
          this.val = val;
          this.index = index;
      }
      
  }
  
  class MyComparator implements Comparator<Pair> {
      
      public int compare(Pair a, Pair b) {
          return a.val - b.val;
          
      }
  }
  ```

  The simplified way:

  ```java
  public class Solution {
      /*
       * @param nums: A list of integers
       * @return: A list of integers includes the index of the first number and the index of the last number
       */
      public int[] subarraySumClosest(int[] nums) {
          
          int[] result = new int[2];
          
          result[0] = -1;
          result[1] = -1;
          
          int[] sum = new int[nums.length + 1];
          sum[0] = 0;
          for(int i = 0; i < nums.length; i++) {
              sum[i + 1] = sum[i] + nums[i];
          }
          
          Pair[] pairs = new Pair[sum.length];
          for(int i = 0; i < sum.length; i++) {
              pairs[i] = new Pair(sum[i], i);
          }
          
          Arrays.sort(pairs, new MyComparator());
          
          int start = 0;
          int end = 1;
          
          int minDiff = pairs[1].val - pairs[0].val;
          
          for(int i = 1; i < sum.length; i++) {
              
              if(minDiff > pairs[i].val - pairs[i - 1].val) {
                  minDiff =  pairs[i].val - pairs[i - 1].val;
                  start = i - 1;
                  end = i;
              }
          }
          
          result[0] = Math.min(pairs[start].index, pairs[end].index);
          result[1] = Math.max(pairs[start].index, pairs[end].index) - 1;
  
          
          return result;
          
          
      }
  }
  
  class Pair {
      int val;
      int index;
      
      Pair(int val, int index) {
          this.val = val;
          this.index = index;
      }
      
  }
  
  class MyComparator implements Comparator<Pair> {
      
      public int compare(Pair a, Pair b) {
          return a.val - b.val;
          
      }
  }
  ```
