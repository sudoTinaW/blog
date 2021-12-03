---
published: true
layout: post
date: '2021-08-04 10:50:00 -0000'
categories: algo
---
Dynamic Programming (DP) is an algorithmic technique for solving an optimization problem by breaking it down into simpler subproblems and utilizing the fact that the optimal solution to the overall problem depends upon the optimal solution to its subproblems.

It has two characters. 

-  An original problem's solution uses the same subproblem multiple times. Subproblems are smaller versions of the original problem.
-  An original problem has optimal substructure property. Any problem has optimal substructure property if its overall optimal solution can be constructed from the optimal solutions of its subproblems.

It is often used to solve questions which are getting min/max, the number of ways, and existing or not.

#### Greedy vs DP vs DFS:

Greedy is based on some heuristic to pick a current optimal solution for a subproblem, and hopefully, the current optimal solution can lead to the final optimal solution. For example, we have 1, 2 , 5 three types of coins. When we want to find 6 dollars change and use less coins as possible, we can use a heuristic as the total is fixed, and the coin's amount bigger, the less coins needed. First we will find 5 and then we will find 1. 2 is exactly the minimal number of coins. However, if our coins values are 1, 3, 4. Based on our heuristic, we will choose 4, 1, and 1. 3 coins are actually not the optimal solution. If we use 3 and 3, we can find 6 with 2 coins.

DP scans all subproblems by steps, and picks the optimal solution at each step. Each step's optimal solution leads to the final optimal solution. Its advantage appears when there are overlapping problems.

DFS also scans all subproblems, and pick the optimal solution from all possible subproblems in one step.  If there are any overlapping problems, we will scan the overlapping problems multiple times.

We can think of most of questions as a graph. I will draw how each algorithm scan the graph. To display DP's advantage, we will draw a graph with overlapping nodes.

DFS 是穷举所有从起点到最深点的路径。如果一个子问题无法聚拢(For example, tree), DFS可以穷举所有答案， DP没有任何优势，就是DFS。

如果子问题可以聚拢(For examale, sum, max, min, exit or not, how many ways)，多个子问题可以合并就可以用DP。 因为如果子问题有聚拢后又发散的，就会出现重复子问题。

![img](GreedyVsDPVsDFS.png)

During an interview, if we make some heuristic and it can guide each step's optimal solution to make the final optimal solution, we will try to use greedy algorithm. Otherwise, we can try to use DP. If the question has no overlapping subproblem, we will use DFS.

#### BFS vs DP on Shortest Path Problem:

BFS is a modified version of Dijkstra with weight all equal to 1. Dijkstra is a greedy algorithm which can find optimal solution when every weight is positive. DP can always find the shortest path whatever a weight is positive or negative. However it is more complicated. Dijkstra is rarely asked during an interview. Therefore, if we find the weight all equal to 1, we shall try BFS first. Otherwise, we will use DP to find the shortest path.

#### Technique:

There are 3 steps to find solve a DP problem,

- First we need to find status and choices. Status is the variables that changes from original problem to subproblem. Choices are the values that lead to status changes.

- Define recursion functions. The function's return is often the original problem's solution. The function's input are status. We can suppose that we have get all subproblems' result but not the last step's. We need to find how we can find the last step's result by using the former result. The recursion function both work for DFS and DP. The difference is DP will use a DP table to save some middle result.

  如果reucrsion function 里，其中一些子问题不仅能组成（更大）下一层子问题的结果，还可以组成同层的答案，那么就说明存在重复子问题，我们应该考虑用DP。For example, If we find the recursion function is as  `f(n) = f(n - 1) + c `, it is a DFS. There is not an overlapping problem. If the recursion function is `f(n) = f(n - 1) + f(n - 2)`, we can use DP. `f(n - 2)` can compose `f(n)` , at the same time, `f(n - 2)` can compose `f(n - 1)`.

- Find the initialization value. Usually, we start from divergence side.

The difficulty of DP is how to define optimal subproblems. We will introduce some techniques to define subproblems. Almost every question can be resolved recursively, but not all questions can be resolved iteratively.

### Problems

### Matrix Type:

If the question offers a matrix as input, and it is not finding the shortest path or finding the shortest path but with weights not always 1, we shall think of using DP. Matrix can be consider as a graph, the node will be each cell, and its neighbours will be its surrounding cells, which also means a cell and its surrounding cells are connected. **For the matrix type DP, we shall think of how to transit the states from its neighbours to the cell.** 

个人总结了一个小技巧，Matrix Type question， 不像sub sequence type，一般**不会**根据不同的choice选择用**不同**元素的subproblem的答案来组成新的problem。**所有**用来组成新答案的元素的结果通常就是我们recursion function里要用的子问题的结果。所以Matrix Type的子问题比较好找。找到这些元素后，我们可以用例子，自己算一遍每一个位置的结果，并找到每个新元素与其前面用的元素的结果的关系。从两头推导， 更容易找到recursion function。

#### [LeetCode 62. Unique Paths](https://leetcode.com/problems/unique-paths/)

#### [LeetCode 63. Unique Paths II](https://leetcode.com/problems/unique-paths-ii/)

#### [LC 64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum/)

#### [LC 221. Maximal Square](https://leetcode.com/problems/maximal-square/)

#### [516  Paint House II](https://www.lintcode.com/problem/516)

### Sub Sequence Type:

Im mathematic, a [subsequence](https://en.wikipedia.org/wiki/Subsequence) is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements. For example, the sequence *{A, B, D}* is a subsequence of *{A, B, C, D, E, F}* obtained after removal of elements *E*, and *F* . If a question requires to find a value of some elements in a sequence and the elements' orders are fixed but not necessarily consecutive, we shall consider the question as DP sub sequence problem.

The sub sequence type's recursion function is harder to define. It is not like matrix type's question, the subproblem's elements are sometimes hard to find. Basically, depends on different choices, we will decide which elements' result will be used in the recursion function.

DP's definition usually is what the question asks among all subsequences ending or starting with `ith` element. Here, ending or starting with `ith`element doesn't mean to the `ith` element has to be presented.

To recursively enumerate all sub sequences, we have two different ways.

- One is **discontinuously** enumerating all elements. Starting from each element horizontally, we will enumerate all possible elements after the starting element in the next line. Such way, line by line, we will finish enumeration by reaching to the last element. We can use this way for all sub sequence questions.**This way is usually used for one dimensional DP functions, which means each element only has one state, used or not in the final result.**

  Following are some techniques to derived the recursion function from discontinuous decision tree,

  - Elements on the last level are all possible initial special case. 
  - We usually get the result from the bottom to the top.
  - To find the optimal sub problems, the elements which we have calculated from the lower level won't be calculated any more. 	
  - The recursion function shall cover all cases between every two level. If the lower level's index has no pattern to induced the upper level, we shall scan all possible indexes in the lower level.
  - The result shall be above all levels, which is "()" in the diagram

  The decision tree is as below,

  ![img](decisionTreeDiscontinue.png)



- Second is **continuously** enumerating all elements. For each element, we have 2 choices, present or not. Each level will only have one elements present or not. **This way is usually used for 2-dimensional DP functions.** Here we need to notice that not all one-dimensional DP functions can be derived from this method. If an element's DP result depends on its former **`<= 2`**DP results and the former 2 results between every 2 level are relatively fixed, we can use this way.

  Following are some techniques to derived the recursion function from continuous decision tree,

  - We often collect all DP results along bottom-up direction. The meaning is starting from the current level's element to the end result.

  - We can often find recursion function from the minimal structure of  two closet present elements. For example, here two closest elements can be `4` and `10.`

  - From a present element to non-present element, we will describe non-present element as `dp[i + 1]` and the present element as `dp[i]`.

  - From a non-present element to a present element, we need to find its closed upper present element, and update the upper present element `ith` result by calcuating the `ith` element and the non-present `j`result, such as `dp[i] = {nums[i] + dp[j], ...}`.

  - From a present to a present element, we need to update the upper present element `ith`result by calculating  the `ith`element and the present `(i + 1)th`element's result, such as `dp[i]= {nums[i] + dp[i + 1], ...}`

  - From a non-present element to a non-present element, we will use the closed to the upper number's result. Since the result can be passed on along the bottom-up direction among all non-present elements.

  - If there is a branch, we need to compare the result and choose one of them.

    The decision tree is as below,

    ![img](decisionTreeContinue.png)

#### [LC 198. House Robber](https://leetcode.com/problems/house-robber/)

Description:

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

Example 1:

```
Input: nums = [1,2,3,1]Output: 4Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).Total amount you can rob = 1 + 3 = 4.
```

Example 2:

```
Input: nums = [2,7,9,3,1]Output: 12Explanation: Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).Total amount you can rob = 2 + 9 + 1 = 12.
```



Constraints:

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 400`

Analysis:

First we will draw the discontinuous decision tree and try to find patterns. Here `dp[i]: starting from ith house till the last house, max money we can rob (Here starting from the ith house means the ith house can present or not present).`  

![img](houseRobDiscon.png)

Form the decision tree, we can see, if we save the results, we only need to compared the green highlight element's result to get the final result. If we loop the level from bottom to top, we can get  recursion function is `dp[i] = max(dp[i + 2], dp[i + 3]) + nums[i]`. The initial value is `dp[len - 1], dp[len - 2]`. Since all other cases are depends on 2 elements, only `dp[len - 3]` depends on 1 element, we need to specially handle`dp[len - 3]` as well. What is more, the final result will get from `max (dp[0] and dp[1])` at the end.

Second, we notice each element only uses maximum 2 elements from lower level, and the 2 elements index are relatively fixed. Therefore, we also can use the continuous decision tree.   `dp[i]` has the same definition as the first way. 

![img](hosuerobSubSequence.png)

Here, we find the minimal structure as the green highlight between two present numbers. For example, `1`'s  result ` dp[i]`will depends on `3's` result, `dp[i + 2] +nums[i] `, and the top most non-present result,  `dp(i + 1)`. Since the question is asking to get the max value. We can write our recursion function as 

`dp[i] = Math.max (dp[i + 2] + nums[i], dp[i + 1])`

Since the recursion function contains `i + 2` and `i + 1`. We need to consider the `(len - 1)th` and `(len - 2)th` as the initial value. The `(len - 1)th` result shall be `nums[len - 1]`. The  `(len - 2)th` result shall be  max of `(nums[len - 1], nums[len - 2])`. 

Here we can use a trick, we can set the DP array as length + 2. The default element's result will be all `0`. Such way, we can get the same initial value as above by following the recursion function. 
$$
\begin{align*}
dp[len - 1] 
&= max(dp[len - 1 + 2] + nums[len - 1], dp(len - 1 + 1)) \\
&= max(dp[len + 1] + nums[len - 1], dp[len])  \\
&= max(0 + nums[len - 1], 0) \\
&= nums[len - 1] \\
\\
dp[len - 2]
&= max(dp[len - 2 + 2] + nums[len - 2], dp(len - 2 + 1)) \\
&= max(dp[len] + nums[len - 2], dp[len - 1])  \\
&= max(0 + nums[len - 2], dp[len - 1]) \\
&= max(nums[len - 2], nums[len - 1]) 
\end{align*}
$$


Finally, we can find patterns between each results. This method is the easiest to understand, but it is harder to find the pattern between each result. Here `dp[i]: ending with ith house, max money we can rob (Here ending with ith house means the ith house can present or not present).`Depends on different choices, to rob or not, we find its subproblem's element might be `i - 1` and `i - 2`. To find what will be the relationship, we will draw the result table.

![img](houseRobbery.png)

From the picture, we can see `dp[i] = max{dp[i - 1], dp[i - 2] + nums[i]}`

#### [LC 213. House Robber II](https://leetcode.com/problems/house-robber-ii/)

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are **arranged in a circle.** That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have a security system connected, and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return *the maximum amount of money you can rob tonight **without alerting the police***.

 

Example 1:

```
Input: nums = [2,3,2]Output: 3Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2), because they are adjacent houses.
```

Example 2:

```
Input: nums = [1,2,3,1]Output: 4Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).Total amount you can rob = 1 + 3 = 4.
```

Example 3:

```
Input: nums = [0]Output: 0
```



Constraints:

- `1 <= nums.length <= 100`
- `0 <= nums[i] <= 1000`



Analysis:

This is a follow-up of [LC 198. House Robber](https://leetcode.com/problems/house-robber/). Since the houses are in a circle, which means the first house and last house can not be robbed at the same time. Therefore, we shall pick the max f containing the first house but not the last and the last house without first house as the final result.


#### [LC 300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/)

Description:

Given an integer array `nums`, return the length of the longest strictly increasing subsequence.

A **subsequence** is a sequence that can be derived from an array by deleting some or no elements without changing the order of the remaining elements. For example, `[3,6,2,7]` is a subsequence of the array `[0,3,1,6,2,2,7]`.

 

Example 1:

```
Input: nums = [10,9,2,5,3,7,101,18]Output: 4Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4.
```

Example 2:

```
Input: nums = [0,1,0,3,2,3]Output: 4
```

Example 3:

```
Input: nums = [7,7,7,7,7,7,7]Output: 1
```

 

Constraints:

- `1 <= nums.length <= 2500`
- `-104 <= nums[i] <= 104`



Analysis:

Depends on `dp[i]`different definition, we can resolve this question by 2 different DP functions .

First,`dp[i]: ending with ith element, longest increasing subsequence length.`  To find what elements will be used in the recursion function, we need to find what former elements's result can be used to combine with  `i` and form a new result. That is any element which can form an increasing sub sequence. To find what will be the relationship, we will draw the result table as following,

![img](lic.png)

We can find the recursion function as,

`dp[i] = Math.max(dp[j],...,dp[k]) + 1, ( nums[j] < nums[i], nums[k] < nums[i], 0 <= j < k < i)`


Second, `dp[i]: starting from ith element till the last element, longest increasing subsequence length.` This question can not draw continuous decision tree, because each new `ith` result can depend on its former more than 2 elements' results, and the elements between every two level are not fixed. Therefore, we will draw discontinuous decision tree as below,

![img](licDiscon.png)

The green highlight are the elements calculated. The rest are directly using the result. From the picture, we can see, each element's result will depends on its direct lower level's elements whose values are greater than it. The recursion function is 

`dp[i] = max{dp[j]}(j >= i + 1, and nums[j] > nums[i])`


#### [121. Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

You want to maximize your profit by choosing a **single day** to buy one stock and choosing a **different day in the future** to sell that stock.

Return *the maximum profit you can achieve from this transaction*. If you cannot achieve any profit, return `0`.

 

Example 1:

```
Input: prices = [7,1,5,3,6,4]Output: 5Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.Note that buying on day 2 and selling on day 1 is not allowed because you must buy before you sell.
```

Example 2:

```
Input: prices = [7,6,4,3,1]Output: 0Explanation: In this case, no transactions are done and the max profit = 0.
```

 

Constraints:

- `1 <= prices.length <= 105`
- `0 <= prices[i] <= 104`



Analysis:

First, we will draw the discontinuous decision tree,

![img](bestTimeToBuyAndSellStockI.png)

Here we notice that the diagram only has two 2 levels, there is no need to find a recursion pattern between levels to avoid repeated calculation. We can directly loop each level's all elements to find the result. 

Here, we can still have an optimization. Among the comparison of elements `(4), (6, 4), (3, 6, 4), (5, 3, 6, 4), (1, , 5, 3, 6, 4)`, if we can save the max value  of its left sibliing's result, we can use its former comparison result to avoid some redundant calculation. 


#### [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)

Description:

You are given an array `prices` where `prices[i]` is the price of a given stock on the `ith` day.

Find the maximum profit you can achieve. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

Example 1:

```
Input: prices = [7,1,5,3,6,4]Output: 7Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```

Example 2:

```
Input: prices = [1,2,3,4,5]Output: 4Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are engaging multiple transactions at the same time. You must sell before buying again.
```

Example 3:

```
Input: prices = [7,6,4,3,1]Output: 0Explanation: In this case, no transaction is done, i.e., max profit = 0.
```

Constraints:

- `1 <= prices.length <= 3 * 104`
- `0 <= prices[i] <= 104`

Analysis:

First we will draw discontinuous decision tree,

![img](bestTimeToBuyAndSellStockIIDiscon.png)

This question is a bit different. For each element, we have `+` and `-` two status. Every other 2 levels are using different sign elements to update results. This usually means, we need an extra dimension to save the DP result. Here, if we ignore the sign, we can see, the current level only uses its right lower level's elements. What is more, if we count elements with different signs as different elements, we can not use a lower level's result to derive its right above level's result by the discontinuous way. Therefore, we will change to use continuous enumeration way to analysis the recursion function.

![img](bestToBuyAndSellStockIICon.png)

From the diagram, we can see, the closet two numbers are only one level away. Therefore, `dp[i]` will only depends on `dp[i + 1]`. We will use `dp[i][0]` denotes the price is negative, which is the buying price. We will use `dp[i][1]` denotes the price is positive, which is the selling price. We can derive the two recursion functions as,

`dp[i][0] = Math.max(dp[i + 1][1] - prices[i], dp[i+ 1][0])`

`dp[i][1] = Math.max(dp[i + 1][0] + prices[i], dp[i + 1][1])`

 Here we notice that we only need the negative situation as the final result, since staring with positive situation is impossible. That is to say, `dp[0][0]` is the final result. 

This question's initial status is a bit special. The last step's negative status is not possible. If the last step can only be negative, we will choose 0 as the last step's result, which is not sell nor buy. Therefore, the initial status are`dp[n- 1][0] = 0` and `dp[n - 1][1] = prices[n - 1]`.


This question has another greedy solution, we will not discuss here.

#### [1911. Maximum Alternating Subsequence Sum](https://leetcode.com/problems/maximum-alternating-subsequence-sum/)

The **alternating sum** of a **0-indexed** array is defined as the **sum** of the elements at **even** indices **minus** the **sum** of the elements at **odd** indices.

- For example, the alternating sum of `[4,2,5,3]` is `(4 + 5) - (2 + 3) = 4`.

Given an array `nums`, return *the **maximum alternating sum** of any subsequence of* `nums` *(after **reindexing** the elements of the subsequence)*.



A **subsequence** of an array is a new array generated from the original array by deleting some elements (possibly none) without changing the remaining elements' relative order. For example, `[2,7,4]` is a subsequence of `[4,2,3,7,2,1,4]` (the underlined elements), while `[2,4,2]` is not.

 

Example 1:

```
Input: nums = [4,2,5,3]Output: 7Explanation: It is optimal to choose the subsequence [4,2,5] with alternating sum (4 + 5) - 2 = 7.
```

Example 2:

```
Input: nums = [5,6,7,8]Output: 8Explanation: It is optimal to choose the subsequence [8] with alternating sum 8.
```

Example 3:

```
Input: nums = [6,2,1,2,4,5]Output: 10Explanation: It is optimal to choose the subsequence [6,1,5] with alternating sum (6 + 5) - 1 = 10.
```

 

Constraints:

- `1 <= nums.length <= 105`
- `1 <= nums[i] <= 105`



Analysis:

This question is very similar to [122. Best Time to Buy and Sell Stock II](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/). We will draw the continuous diagram as below, 

![img](alternativeSum.png)

The recursion function will be 

`dp[i][0] = Math.max(dp[i][1] - nums[i], dp[i + 1][0])` 

`dp[i][1] = Math.max(dp[i][0] + nums[i], dp[i + 1][1])`

The final result will be `dp[0][1]`

The initial value will be `dp[n - 1][0] = 0` and `dp[n - 1][1] = nums[n - 1]` Here we need to notice that if the last element is negative, we will not add it up to the total sum. Its best value will be `0`.


###  Continuous Segment Type:

The continuous segment type questions usually offer a sequence.  We can split the sequence into several segments and every segment needs to satisfy some requirements. The number of segments can be fixed as k, or not fixed. To resolve this type of question, 

- This type of question, `dp[i]`usually means length of `i`'s ' result. To include all length, and DP index starts from `0`, we often define `dp.length` as `length + 1`
- We need to find what elements' results will be used in the recursion function. The elements are usually defined as any element before `i` which can form `i` length result. Usually those elements combine with one satisfied segement will form `I` length result .
- Calculate those element's results, and use them to find the pattern between the `ith` element's result and all other former element's result.
- Write the DP function.

#### [279. Perfect Squares](https://leetcode.com/problems/perfect-squares/)

Description: Given an integer `n`, return *the least number of perfect square numbers that sum to* `n`.

A **perfect square** is an integer that is the square of an integer; in other words, it is the product of some integer with itself. For example, `1`, `4`, `9`, and `16` are perfect squares while `3` and `11` are not.

 

Example 1:

```
Input: n = 12Output: 3Explanation: 12 = 4 + 4 + 4.
```

Example 2:

```
Input: n = 13Output: 2Explanation: 13 = 4 + 9.
```

 

Constraints:

- `1 <= n <= 104`

Analysis:

Here, we can think that we have n times`1`as an input sequence. All possible choices are length as perfect square which are less or equal to length. The elements that will be used in the recursion function are the `i` length - perfect square length choices. We will draw a result diagram to find the pattern.

![img](perfectSquare.png)

From the diagram, we can find DP function as

`dp[i] = Math.min(dp[i - j * j] + 1), (1 <= j <= i)`

To consider any perfect square number's result as  `1`, `dp[0]` shall be `0`.



#### [139. Word Break](https://leetcode.com/problems/word-break/)

Description:

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

 

Example 1:

```
Input: s = "leetcode", wordDict = ["leet","code"]
Output: true
Explanation: Return true because "leetcode" can be segmented as "leet code".
```

Example 2:

```
Input: s = "applepenapple", wordDict = ["apple","pen"]
Output: true
Explanation: Return true because "applepenapple" can be segmented as "apple pen apple".
Note that you are allowed to reuse a dictionary word.
```

Example 3:

```
Input: s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]
Output: false
```

 

Constraints:

- `1 <= s.length <= 300`
- `1 <= wordDict.length <= 1000`
- `1 <= wordDict[i].length <= 20`
- `s` and `wordDict[i]` consist of only lowercase English letters.
- All the strings of `wordDict` are **unique**.

Analysis:

First we need to find all elements used in the recursion function, the elements are all elements cutting off a possible word in the dictionary.

To find the relationship, we will draw a result diagram,

![img](wordbreak.png)

From the diagram, we can derive the DP function, `dp[i] = dp[i] || dp[i - 1], (1 <= j <= i)`


####  [132. Palindrome Partitioning II](https://leetcode.com/problems/palindrome-partitioning-ii/)

Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return *the minimum cuts needed* for a palindrome partitioning of `s`.

Example 1:

```
Input: s = "aab"Output: 1Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.
```

Example 2:

```
Input: s = "a"Output: 0
```

Example 3:

```
Input: s = "ab"Output: 1
```

 

Constraints:

- `1 <= s.length <= 2000`
- `s` consists of lower-case English letters only.

 

Analysis:

First we need to find all elements used in the recursion function, the elements are all elements cutting off a possible palindrome string.

To find the relationship, we will draw a result diagram,

![img](palindromePartitionII.png)

From the diagram, we can derive the DP function as 

`dp[i] = min(dp[j] + 1), ( 1 <= j <= i)`

`dp[0]`shall be `-1` so that any palindrome string follow the formula can be counted as `0`.


#### [437 Copy Books](https://www.lintcode.com/problem/437/)

Description

Given `n` books and the `i-th` book has `pages[i]` pages. There are `k` persons to copy these books.

These books list in a row and each person can claim a continous range of books. For example, one copier can copy the books from `i-th` to `j-th` continously, but he can not copy the 1st book, 2nd book and 4th book (without 3rd book).

They start copying books at the same time and they all cost 1 minute to copy 1 page of a book. What's the best strategy to assign books so that the slowest copier can finish at earliest time?

Return the shortest time that the slowest copier spends.

The sum of book pages is less than or equal to 2147483647

Example 1:

```
Input: pages = [3, 2, 4], k = 2Output: 5Explanation:     First person spends 5 minutes to copy book 1 and book 2.    Second person spends 4 minutes to copy book 3.
```

Example 2:

```
Input: pages = [3, 2, 4], k = 3Output: 4Explanation: Each person copies one of the books.
```



Analysis:

This question is a continous segment question with fixed segment size. Since we have a fixed segment size, we need to use 2D DP array to save result.  The difference of this question and above 1D questions is we will find results based on its all former results on the one row up, which is`dp[r - 1][0, ..., (i - 1)]`, not `dp[r][0, ..., (i - 1)]`. Because when we enumerate a new segment ending with `i`, the column segment will increase 1 as well.

I will post a diagram with only one calculation as an example,

![img](copyBooks.png)



To get the row 1 following our formula, we will create an extra 0 segment row. Its value shall not be picked in any situation. We can set them as the largest number.

The recursion function is `dp[r][i]=min(j=0,...,i){max{dp[r-1][j],pages[j]+...+pages[i-1]}}`

### Two Sub Sequences Type:

This type of question's input is usually two strings. Some sample questions are as following,

- Questions about whether two strings can match with each other by following some patterns.
- Questions about whether  one string includes the other string.
- Questions about whether one string can change to the other string by following some changing rules.
- Questions about the two strings' common subsequences
- ....

Those questions usually use 2D array to save DP result. `dp[i][j]`usually represents `str(0...i)`, and `str(0...j)`'s result. Those questions usually include empty string's situation. Therefore, the result array length is usually input string's `length + 1`. The best way to find the recursion function is to find from its 2D result table.

#### [LC 1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)

Description:

Given two strings `text1` and `text2`, return *the length of their longest **common subsequence**.* If there is no **common subsequence**, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

 

Example 1:

```
Input: text1 = "abcde", text2 = "ace" 
Output: 3  
Explanation: The longest common subsequence is "ace" and its length is 3.
```

Example 2:

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

Example 3:

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

 

Constraints:

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

Analysis:

This question has 2 strings as inputs, and want to get the **longest** length. We shall consider the question is DP Two Sub Sequences Type. `dp[i][j]`shall be `0 to i of string1 `  and  `0 to j of stirng2 ` longest common subsequence length. Since this question is asking whether the strings have common strings, the update situation might have relationship of whether the current character are equal. Now we will draw the diagram to find the relationship especially among `dp[i][j]` and `dp[i][j - 1], dp[i - 1][j], and dp[i - 1][j - 1]`.

![img](longestComSub.png)

From the result table, we can find 2 patterns

`when char1 == char2, dp[i][j] = Math.min(dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1]) + 1`

`when char1 != char2, dp[i][j] = Math.max(dp[i][j - 1], dp[i - 1][j])`

Sometimes, `dp[i][j - 1], dp[i - 1][j], dp[i - 1][j - 1]`have max / min relationship, we don't need all of them. It can simplify the DP function. For example, this question, `dp[i][j - 1], dp[i - 1][j] shall be >= dp[i - 1][j - 1]` because the longer the string, the longest common subsequence shall be longer or not changed. However, if we can not find those hinted relationships, we shall check all situations.

This question initial situations shall be `dp[i][0] = 0`, and` dp[0][j] = 0`.


#### [72. Edit Distance](https://leetcode.com/problems/edit-distance/)

Description,

Given two strings `word1` and `word2`, return *the minimum number of operations required to convert `word1` to `word2`*.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

 Example 1:

```
Input: word1 = "horse", word2 = "ros"Output: 3Explanation: horse -> rorse (replace 'h' with 'r')rorse -> rose (remove 'r')rose -> ros (remove 'e')
```

Example 2:

```
Input: word1 = "intention", word2 = "execution"Output: 5Explanation: intention -> inention (remove 't')inention -> enention (replace 'i' with 'e')enention -> exention (replace 'n' with 'x')exention -> exection (replace 'n' with 'c')exection -> execution (insert 'u')
```

Constraints:

- `0 <= word1.length, word2.length <= 500`
- `word1` and `word2` consist of lowercase English letters.

Analysis:

First we will draw the diagram, and find the pattern among `dp[i][j]` and `dp[i][j - 1], dp[i - 1][j], and dp[i - 1][j - 1]`. Since we calculate the distance of editing, the case of 2 characters from 2 strings the same might be a special case.

From the diagram, we can see,

`when 2 characters are the same,dp[i][j] = dp[i - 1][j - 1] `

`when 2 characters are different, dp[i][j] = Math.min(Math.min(dp[i - 1][j], dp[i][j - 1]), dp[i - 1][j - 1]) + 1 `

The initial value shall be `dp[0][0] = 0, dp[i][0] = dp[i - 1][0] + 1, dp[0][j] = dp[0][j - 1] + 1 `

![img](editDis.png)

#### [97. Interleaving String](https://leetcode.com/problems/interleaving-string/)

Description:

Given strings `s1`, `s2`, and `s3`, find whether `s3` is formed by an **interleaving** of `s1` and `s2`.

An **interleaving** of two strings `s` and `t` is a configuration where they are divided into **non-empty** substrings such that:

- `s = s1 + s2 + ... + sn`
- `t = t1 + t2 + ... + tm`
- `|n - m| <= 1`
- The **interleaving** is `s1 + t1 + s2 + t2 + s3 + t3 + ...` or `t1 + s1 + t2 + s2 + t3 + s3 + ...`

**Note:** `a + b` is the concatenation of strings `a` and `b`.

 

Example 1:

![img](https://assets.leetcode.com/uploads/2020/09/02/interleave.jpg)

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"Output: true
```

Example 2:

```
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"Output: false
```

Example 3:

```
Input: s1 = "", s2 = "", s3 = ""Output: true
```

 

Constraints:

- `0 <= s1.length, s2.length <= 100`
- `0 <= s3.length <= 200`
- `s1`, `s2`, and `s3` consist of lowercase English letters.

 Analysis:

This question is about 3 strings as input. However, it is still a two sub sequence type. We can think in this way, this question is using 2 strings' different subsequences to combine to a new string. If the 2 subsequences are fixed, we only need to check whether the new string is the 3rd input string. The 3rd string here serves as a constant. Therefore, we will still use two sub sequence type to resolve this question.

First we need to draw the result diagram to find the recursion function. Here are the diagram,

![img](interleavingString.png)

During the drawing, we notice when either s1 or s2's char equals to the third string's proper position's char, the result might be true. Also, any true result's surrounding `dp[i - 1][j]` or `dp[i][j - 1]` must be true. Therefore, we find the recursion function as,

 `when dp[i - 1][j] && s1.charAt(i - 1) == s3.charAt(i + j - 1), dp[i][j] = true`

`when dp[i][j - 1] && s2.charAt(j - 1) == s3.charAt(i + j - 1), dp[i][j] = true `

Other situation, `dp[i][j] = false`

The initial value `dp[0][0] = true`

`when s1.charAt(i - 1) == s3.charAt(i - 1) && dp[i - 1][0], dp[i][0] = true`

`when s2.charAt(j - 1) == s3.charAt(j - 1) && dp[0][j - 1], dp[0][j] = true`

### Knapsack Type:

Knapsack Problem is a typical DP question, and Its input is easily distinguished. The problem offers a sum, and a weight array whose element value can be a part of sum, as inputs. Sometimes, it offers another cost array whose element's index same as weight array. The Knapsack problem's subproblem is in 2D dimension. One dimension is sum from 0 to the total result. Another dimension is index of weight array. Since weight array and cost array own the same index, we can find its cost easily as well. 

The Knapsack problem's recursion function is short and easy to remember. Here we will draw the result diagram and find the recursion function first. Latter, we can remember the function and directly use it.

#### 01 Knapsack:

Here we will use an example to find the pattern. Suppose our sum is 10, the weight array is [2, 3, 5, 7], and the cost array is [2, 5, 2, 5]. The 2D array is one dimension is from 0 to 10, and another dimension is input two arrays' index. Following are the result table,

![img](knapsack.png)



From the table, we can find the recursion function as `dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + c[i]])`

#### Unbounded  Knapsack:

If every element in weight array can be used multiple times, the problem becomes complete Knapsack problem. We will use the same example and find its recursion function.

First we will draw the result table,

![img](knapsackII.png)



The recursion function will be as `dp[i][j] = max(dp[i - 1][j], dp[i][j - w[i]] + c[i]])`

Both knapsack problems' recursion function have meanings, the meaning explanation can be found from blog [Labuladong Blog](https://labuladong.gitbook.io/algo/mu-lu-ye-2/mu-lu-ye-2/bei-bao-wen-ti). Here we only need to remember the recursion function and how they are derived.

#### [92 Backpack](https://www.lintcode.com/problem/92/)

This question can be treated as an easy version of knapsack problem. `dp[i][j] will represent the max sum until i and j`. Since every item can be only used once, this is a 01 knapsack variation.We will directly use the 01 knapsack template.

Here we add an empty row of `0` to simplify the initialization. It is not necessary to be used all the time.

#### [125 Backpack II](https://www.lintcode.com/problem/125)

Description

There are `n` items and a backpack with size `m`. Given array `A` representing the size of each item and array `V` representing the value of each item.

What's the maximum value can you put into the backpack?

`A[i], V[i], n, m` are all integers.You can not split an item.The sum size of the items you want to put into backpack can not exceed `m`.Each item can only be picked up oncem <= 1000*m*<=1000\len(A),len(V)<=100*l**e**n*(*A*),*l**e**n*(*V*)<=100

Example

Example 1:

Input:

```
m = 10A = [2, 3, 5, 7]V = [1, 5, 2, 4]
```

Output:

```
9
```

Explanation:

Put A[1] and A[3] into backpack, getting the maximum value V[1] + V[3] = 9

Example 2:

Input:

```
m = 10A = [2, 3, 8]V = [2, 5, 8]
```

Output:

```
10
```

Explanation:

Put A[0] and A[2] into backpack, getting the maximum value V[0] + V[2] = 10

Analysis:

This is the 01 knapsack problem template. The thought has been described in the general description. Here we will only post the code.

#### [440 · Backpack III](https://www.lintcode.com/problem/440)

Description

Given `n` kinds of items, and each kind of item has an infinite number available. The `i-th` item has size `A[i]` and value `V[i]`.

Also given a backpack with size `m`. What is the maximum value you can put into the backpack?

You cannot divide item into small pieces.Total size of items you put into backpack can not exceed `m`.

Example 1:

```
Input: A = [2, 3, 5, 7], V = [1, 5, 2, 4], m = 10Output: 15Explanation: Put three item 1 (A[1] = 3, V[1] = 5) into backpack.
```

Example 2:

```
Input: A = [1, 2, 3], V = [1, 2, 3], m = 5Output: 5Explanation: Strategy is not unique. For example, put five item 0 (A[0] = 1, V[0] = 1) into backpack.
```

Analysis:

This is the unbounded knapsack problem's template. The thought has been described in the general description. Here we will only post the code.

#### [562 Backpack IV](https://www.lintcode.com/problem/563)

Description

Given n items with size `nums[i]` which an integer array and all positive numbers. An integer `target` denotes the size of a backpack. Find the number of possible fill the backpack.

```
Each item may only be used once
```

Example

Given candidate items `[1,2,3,3,7]` and target `7`,

```
A solution set is: [7][1, 3, 3]
```

return `2`

Analysis:

This question is a 01 knapsack variation. When we select the `ith`element, the total number of ways is  `dp[i][j] = dp[i - 1][j] + dp[i - 1][j - w[i]] `. When we are not selecting the `ith` element, the total number of ways are the former `dp[i - 1][j]`. This question's initialization is a bit different. There is one way to make sum as 0 which is not selecting any element. However, if there is no element, there is 0 way that make any weights except 0.

#### [416 Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/)

Description:

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

 

Example 1:

```
Input: nums = [1,5,11,5]Output: trueExplanation: The array can be partitioned as [1, 5, 5] and [11].
```

Example 2:

```
Input: nums = [1,2,3,5]Output: falseExplanation: The array cannot be partitioned into equal sum subsets.
```



Analysis:

This question is a variation of 01 knapsack problem. The question can be translated to whether there is a sum of elements from the input array equals to the `total sum / 2`. Therefore, 01 knapsack problem will save boolean value as DP array's content. The detailed relationship will be as the code.

There is a detail, if the sum is an odd number, we definitely can not find a partition.


####  [322. Coin Change](https://leetcode.com/problems/coin-change/)

Description:

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the fewest number of coins that you need to make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an infinite number of each kind of coin.

 

Example 1:

```
Input: coins = [1,2,5], amount = 11Output: 3Explanation: 11 = 5 + 5 + 1
```

Example 2:

```
Input: coins = [2], amount = 3Output: -1
```

Example 3:

```
Input: coins = [1], amount = 0Output: 0
```

Example 4:

```
Input: coins = [1], amount = 1Output: 1
```

Example 5:

```
Input: coins = [1], amount = 2Output: 2
```

 

Constraints:

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 231 - 1`
- `0 <= amount <= 104`

Analysis:

This question is an unbounded knapsack problem. Since this question is getting the min value, we need to initialize the impossible situation as MAX. During the calculation, we shall avoid MAX + 1 situation to avoid overflowing to MIN.

#### [518. Coin Change 2](https://leetcode.com/problems/coin-change-2/)

Description:

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return *the number of combinations that make up that amount*. If that amount of money cannot be made up by any combination of the coins, return `0`.

You may assume that you have an infinite number of each kind of coin.

The answer is **guaranteed** to fit into a signed **32-bit** integer.

 

Example 1:

```
Input: amount = 5, coins = [1,2,5]Output: 4Explanation: there are four ways to make up the amount:5=55=2+2+15=2+1+1+15=1+1+1+1+1
```

Example 2:

```
Input: amount = 3, coins = [2]Output: 0Explanation: the amount of 3 cannot be made up just with coins of 2.
```

Example 3:

```
Input: amount = 10, coins = [10]Output: 1
```

 

Constraints:

- `1 <= coins.length <= 300`
- `1 <= coins[i] <= 5000`
- All the values of `coins` are **unique**.
- `0 <= amount <= 5000`

Analysis:

This is also an unbounded knapsack problem. This questions is asking for how many ways. If we pick the item, the total ways is its `dp[i - 1][j] + dp[i][j - coins[i - 1]]`.

#### [89 · k Sum](https://www.lintcode.com/problem/89)

Description:

Given `n` distinct positive integers, integer `k` (k \leq n)(*k*≤*n*) and a number `target`.

Find `k` numbers where sum is target. Calculate how many solutions there are?

Example 1:

Input:

```
A = [1,2,3,4]k = 2target = 5
```

Output:

```
2
```

Explanation:

1 + 4 = 2 + 3 = 5
Example 2:

Input:

```
A = [1,2,3,4,5]k = 3target = 6
```

Output:

```
1
```

Explanation:

There is only one method. 1 + 2 + 3 = 6

Analysis:

This question is a 01 knapsack problem with one more dimension which is k. When we don't pick the item, to get the r sum, its value shall be its former `dp[i - 1][j][r]`. If we pick the item, the value shall be ` dp[i][j][r] = dp[i - 1][j][r] + dp[i - 1][j - A[i - 1]][r - 1]`.

### Interval Type:

Those questions need to make choice from both ends of input, or to delete elements from middle. Those questions are easier to resolved by DFS + memorization, because their DP results are traversed along diagonal direction. As matrix property, diagonal index's sum are always be the same, which is the length of input. If we need to traverse along diagonal, we will use this property. Since the recursion function is hard to find for those questions already, we will not use diagonal traverse way, but DFS + memorization.



#### [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)

Description:

Given a string `s`, find *the longest palindromic **subsequence**'s length in* `s`.

A **subsequence** is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.

 

Example 1:

```
Input: s = "bbbab"
Output: 4
Explanation: One possible longest palindromic subsequence is "bbbb".
```

Example 2:

```
Input: s = "cbbd"
Output: 2
Explanation: One possible longest palindromic subsequence is "bb".
```

 

Constraints:

- `1 <= s.length <= 1000`
- `s` consists only of lowercase English letters.

Analysis:

First we need to find elements' result relationship by drawing result table. Here, the row represents starting index, and the column represents ending index.

![img](longestPalindromicSub.png)

From the picture we can find each element's result has following relationship,

When `str[i] == str[j]`, `dp[i][j] = dp[i - 1][j + 1] + 2` 

When `str[i] != str[j]`, `dp[i][j] = max(dp[i - 1][j], dp[i][j + 1])`



Since the recursion function is growing diagonally, to avoid more difficult diagonal traverse,  we will directly use DFS + memorization to solve this problem. To use DFS, we will need to draw the decision tree.

```
                  str[0...n]
    头尾元素相等 /          \ 头尾元素不相等
         str[1...n-1]    str[0...n-1] and str[1...n]
        /         \             ...
  str[2...n-2]   str[1...n-2] and str[2...n-1]

```

The decision tree is actually traversing from top left conner in the result table to the diagonal line of the diagram. Therefore,`i` and `j` update are reversed as our recursion function. From the decision tree, we can see, there are only 3 possibilities on each row. Therefore, we will call recursion function for 3 times.

#### [312. Burst Balloons](https://leetcode.com/problems/burst-balloons/)

Description:

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return *the maximum coins you can collect by bursting the balloons wisely*.

 

Example 1:

```
Input: nums = [3,1,5,8]
Output: 167
Explanation:
nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
coins =  3*1*5    +   3*5*8   +  1*3*8  + 1*8*1 = 167
```

Example 2:

```
Input: nums = [1,5]
Output: 10
```

 

Constraints:

- `n == nums.length`
- `1 <= n <= 500`
- `0 <= nums[i] <= 100`



Analysis:

This question's recursion function is hard to find. For all different possible interval, `[i, j]`. We need to try to burst all elements in the interval and get the max value of all possible burst situation. The DP function is as below, 

```
dp[i][j] = Math.max(nums[i - 1] * nums[k] * nums[j + 1] + dp[i][k - 1] + dp[k + 1][j]), i <= k <= j

```

Since it uses all its former diagonal result line, it is hard to find the recursion even we draw the diagram. Here, we will remember the resolving way.

Since it is an interval question, we will use DFS + memorization.  Here is the decision tree,

![img](burstBullom.png)


There are 2 special cases which are start or end are on the edges. Here we use `getNum` function to handle this situation. If start or end is on the edge, we will set its interval value as `1`.


This article is inspired from the following articles,

[Divide and Conquer Vs Dynamic Programming](https://itnext.io/dynamic-programming-vs-divide-and-conquer-2fea680becbe)

[常用的最短路算法](https://seineo.github.io/%E5%9B%BE%E8%AE%BA%EF%BC%9A%E5%B8%B8%E7%94%A8%E7%9A%84%E6%9C%80%E7%9F%AD%E8%B7%AF%E7%AE%97%E6%B3%95%E8%AF%A6%E8%A7%A3.html)

[P.yh Blog](https://juejin.cn/post/6844903985711677447)

[Labuladong Blog](https://labuladong.gitbook.io/algo/mu-lu-ye-2)
