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

#### [838. Subarray Sum Equals K](https://www.lintcode.com/problem/838/)

#### [41. Maximum Subarray(leetcode)](https://leetcode.com/problems/maximum-subarray/)

#### [139. Subarray Sum Closest](https://www.lintcode.com/problem/subarray-sum-closest/description)

