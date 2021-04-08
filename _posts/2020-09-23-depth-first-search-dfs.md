---
published: true
layout: post
date: '2020-09-23 14:50:00 -0000'
categories: algo
---
Depth First Search is traversing through a multiple-branch decision tree. The question usually requires **all** solutions. Since DFS time complexity is O(2^n) or O(n!), tree's depth can not be very big. DFS is often used to resolve Combination and Permutation problems.

### Basic Knowledge:
DFS problem has a template. As long as you can draw the decision tree, you can resolve the issue by the template. Here is the template, modified from [labuladong](https://labuladong.gitbook.io/algo/suan-fa-si-wei-xi-lie/hui-su-suan-fa-xiang-jie-xiu-ding-ban) 

```
result = []
def dfs(choice, choice list):
    
    if satisfied the requirment:
        result.add(choice)
        return

    for choice in choice list:
        make the choice
        dfs(choice, choice list)
        cancle the choice
```

The main issue that DFS can resolve is to enumerate all combinations or permutations of some data. The combination and permutation decision tree look a bit different. As well, their way of keeping track of choice list are different too.

 Combination:

![img](/asset/DFSCom.jpg)

Permutation:

![img](/asset/Perm.jpg)

Combination decision tree has more nodes on the left side than on the right side. Permutation decision tree are equally distributed on the left and right side.

Combination DFS template: 

*Most of base cases are explicitly shown as in what situation the program will exit, that is, when the program doesn't need to call recursion. However, this template did not show the exiting situation, instead, it shows in what situation program needs to call the recursion.*   

**Combination uses start index to exclude choices before the index you are choosing**

```
result = []
def dfs(nums, startIndex, subset, result):
		
	result.add(subset)
    
    for(int i = startIndex; i < nums.length; i++)
    	subset.add(nums[i]);
    	dfs(nums, i++, subset, result);
    	subset.removeLast();

```

Permutation DFS template:

**Permutation uses a visited array to exclude choices has been chosen before.**

```
result = []
def dfs(nums, boolean[] visited, subset, result):
    if satisfied the requirment:
        result.add(subset);
        return

    for(int i = 0; i < nums.length; i++) 
    	if(visited[i]) {
    		continue;
    	}
    	subset.add(nums[i]);
    	visited[i] = true;
    	dfs(nums, visited, subset, result);
    	subset.removeLast();
    	visited[i] = false;
    
```

If there are duplicates in permutation or combination, we only choose the duplicates once, that is to say, if we met the duplicate number second time, we won't choose the duplicate number, neither call further recursion. The combination or permutation duplicates will exclude some choices from the original template. The decision trees for duplicate numbers are as followings,

Combination with duplicates(After sorting, the same element appears more than once): 

![img](/asset/comb2.jpg)

Permutation with duplicates:

![img](/asset/perm2.jpg)



### Problems:

#### [17. Subsets](https://www.lintcode.com/problem/subsets/solution)

Description:

Given a set of distinct integers, return all possible subsets.

Example :

```
Input: [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```

Analysis:

This is the template of Combination.

```java
    public List<List<Integer>> subsets(int[] nums) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        if(nums == null || nums.length == 0) {
            return result;
        }
        Arrays.sort(nums);
        List<Integer> subset = new ArrayList<>();
        
        dfs(nums, 0, subset, result);
        return result;
    }
    
    private void dfs(int[] nums, int start, List<Integer> subset, List<List<Integer>> result) {

        result.add(new ArrayList<Integer>(subset));
        
        for(int i = start; i < nums.length; i++) {
            subset.add(nums[i]);
            dfs(nums, i + 1, subset, result);
            subset.remove(subset.size() - 1);
        }
    }
```

#### [18. Subsets II](https://www.lintcode.com/problem/subsets-ii/description) 

Description:

Given a collection of integers that might contain duplicates, *nums*, return all possible subsets (the power set).

Example:

```
Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
```

Analysis:

This question only needs to add one more condition to exclude the duplicate choices.

```java
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        if(nums == null) {
            return result;        
        }
        
        if(nums.length == 0) {
            result.add(new ArrayList<>());
            return result;
        }
        
        Arrays.sort(nums);
        dfs(nums, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void dfs(int[] nums, int start, List<Integer> subset, List<List<Integer>> result) {
        
        result.add(new ArrayList<Integer>(subset));
        
        for(int i = start; i < nums.length; i++) {
            
            if(i != start && nums[i] == nums[i - 1]) {
                continue;
            }
            
            subset.add(nums[i]);
            dfs(nums, i + 1, subset, result);
            subset.remove(subset.size() - 1);
        }
    }
```

####  [135. Combination Sum](https://www.lintcode.com/problem/combination-sum/description)

Descriptions:

Given a set of candidate numbers `candidates` and a target number `target`. Find all unique combinations in `candidates` where the numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

Example 1:

```
Input: candidates = [2, 3, 6, 7], target = 7
Output: [[7], [2, 2, 3]]
```

Example 2:

```
Input: candidates = [1], target = 3
Output: [[1, 1, 1]]
```

Notice:

1. All numbers (including `target`) will be positive integers.
2. Numbers in a combination `a1, a2, … , ak` must be in non-descending order. (ie, `a1 ≤ a2 ≤ … ≤ ak`)
3. Different combinations can be in any order.
4. The solution set must not contain duplicate combinations.

Analysis:

This is a combination problem because all results don't have an order difference unlike a permutation problem requires. For example, the result [2, 2, 3] and [3, 2, 2] are the same result. The order doesn't matter.

Then we can draw the decision tree to see which choices we need to collect.

![img](/asset/CombinationSum.jpg)



From the decision tree, we can see that recursion quit condition is sum > target. Also, the start index will be the current index, not the next. This question also requires removing duplicates. Therefore, we will use the combination with duplicates template.

*This question is modifying template by changing the start and terminate condition*

```java
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        
        if(candidates == null || candidates.length == 0) {
            return null;
        }
        
        List<List<Integer>> result = new ArrayList<>();
        
        Arrays.sort(candidates);
        dfs(candidates, target, 0, new ArrayList<Integer>(), result, 0);
        return result;
    }
    
    private void dfs(int[] candidates, int target, int start, List<Integer> subset, List<List<Integer>> result, int sum) {
        
        if(sum > target) {
            return;
        }
        
        if(sum == target) {
            result.add(new ArrayList<Integer>(subset));
        }
        
        for(int i = start; i < candidates.length; i++) {
            
            if(i != start && candidates[i] == candidates[i - 1]) {
                continue;
            }
            
            subset.add(candidates[i]);
            dfs(candidates, target, i, subset, result, sum + candidates[i]);
            subset.remove(subset.size() - 1);
        }
    }
```

#### [90. k Sum II](https://www.lintcode.com/problem/k-sum-ii/description)

Descriptions:

Given n unique positive integers, number k (1<=k<=n) and target.

Find all possible k integers where their sum is target.

Example 1:

```
Input: [1,2,3,4], k = 2, target = 5
Output:  [[1,4],[2,3]]
```

Example 2:

```
Input: [1,3,4,6], k = 3, target = 8
Output:  [[1,3,4]]	
```

Analysis:

First the result order doesn't matter. For example, [1, 4] and [4, 1] only counts as 1 solution. It means we will use combination template. We will draw the decision tree first to see which choices we need. From the tree, we can see the recursion quit condition is k == 0. Along the recursion, we need to collect the sum of each branch. When sum == target, we will collect the result.

*This question is modifying template by adding a terminate condition.*

![img](/asset/kSumII.jpg)

```java
    public List<List<Integer>> kSumII(int[] A, int k, int target) {
        List<List<Integer>> result = new ArrayList<>();
        
        if(A == null || A.length == 0) {
            return result;
        }
        
        dfs(A, k, target, 0, 0, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int[] A, int k, int target, int start, int sum, List<Integer> comb, List<List<Integer>> result) {
        
        if(k == 0) {
            if(sum == target) {
                result.add(new ArrayList<Integer>(comb));
            }
            return;
        }
        
        
        for(int i = start; i < A.length; i++) {
            comb.add(A[i]);
            dfs(A, k - 1, target, i + 1, sum + A[i], comb, result);
            comb.remove(comb.size() - 1);
        }

    }
```

#### [136. Palindrome Partitioning](https://www.lintcode.com/problem/palindrome-partitioning/description)

Descriptions:

Given a string `s`. Partition `s` such that every substring in the partition is a palindrome.

Return all possible palindrome partitioning of `s`.

Example:

```
Input: "aab"
Output: [["aa", "b"], ["a", "a", "b"]]
Explanation: There are 2 ways to split "aab".
    1. Split "aab" into "aa" and "b", both palindrome.
    2. Split "aab" into "a", "a", and "b", all palindrome.
```

Notice:

1. Different partition can be in any order.
2. Each substring must be a continuous segment of `s`.

Analysis:

This question's the length of each element in the result set is different, which means this is a combination problem.

The common way  to partition a string is, for each cut from 0 to length, enumerate the cut positions, and take the left side of cut as a partition. For example string "aab" partition's decision tree is as following,

![img](/asset/palindromePartition.jpg)

From the picture, horizontally, we will enumerate each position for a new cut, and vertically, we will enumerate number of cuts. each node is the substring between start and loop index. If the all substrings are palindrome vertically, we can add all substrings into the result. If there is any substring which is not a palindrome, we will not call its recursion any further. 

*This question is modifying the combination by adding a pruning condition and a terminate condition*

```java
    public List<List<String>> partition(String s) {
        
        List<List<String>> result = new ArrayList<>();
        
        if(s == null || s.length() == 0) {
            return result;    
        }
        
        dfs(s, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void dfs(String s, int start, List<String> comb, List<List<String>> result) {
        
        if(start == s.length()) {
            result.add(new ArrayList<String>(comb));
        }
        
        for(int i = start; i < s.length(); i++) {
            String temp = s.substring(start, i + 1);
            
            if(!isPalindrome(temp)) {
                continue;
            }
            
            comb.add(temp);
            dfs(s, i + 1, comb, result);
            comb.remove(comb.size() - 1);
            
        }
        
    }
    
    private boolean isPalindrome(String s) {
        
        int start = 0;
        int end = s.length() - 1;
        
        while(start <= end) {
            if(s.charAt(start) != s.charAt(end)) {
                return false;
            }
            start++;
            end--;
        }
        
        return true;
    }
```

#### [680. Split String](https://www.lintcode.com/problem/split-string/description)

Description:

Give a string, you can choose to split the string after one character or two adjacent characters, and make the string to be composed of only one character or two characters. Output all possible results.

Example1

```
Input: "123"
Output: [["1","2","3"],["12","3"],["1","23"]]
```

Example2

```
Input: "12345"
Output: [["1","23","45"],["12","3","45"],["12","34","5"],["1","2","3","45"],["1","2","34","5"],["1","23","4","5"],["12","3","4","5"],["1","2","3","4","5"]]
```

Analysis:  This is a combination variation. Instead of enumerate all possible n numbers in one node's all siblings, it only needs to enumerate first 2 siblings. Therefore, the index `i` will not be only  between `start` and `string.length()`, but also in the range of `start` and `start + 2`.  Here is the decision tree. 

![img](/asset/splitString.jpg)

*This problem modifies the combination template by narrowing the index `i` range, which is shorting the ending position.*

```java
    public List<List<String>> splitString(String s) {
        List<List<String>> result = new ArrayList<>();
        
        if(s == null) {
            return result;
        }
        
        if(s.length() == 0) {
            result.add(new ArrayList<String>());
            return result;
        }
        
        dfs(s, 0, new ArrayList<>(), result);
        return result;
    }
    
    private void dfs(String s, int start, List<String> comb, List<List<String>> result) {
            
        if(start == s.length()) {
            result.add(new ArrayList<>(comb));
            return;
        }
        
        for(int i = start; i < start + 2; i++) {
            if(i < s.length()) {
                comb.add(s.substring(start, i + 1));
                dfs(s, i + 1, comb, result);
                comb.remove(comb.size() - 1);
            }

        }         

    }
```



#### [15. Permutations](https://www.lintcode.com/problem/permutations/description)

Descriptions:

Given a list of numbers, return all possible permutations.

Example:

```
Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

Notice:

You can assume that there is no duplicate numbers in the list.

Analysis:

DFS permutation template.

```java
    public List<List<Integer>> permute(int[] nums) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        if(nums == null) {
            return result;
        }
        
        if(nums.length == 0) {
            result.add(new ArrayList<>());
            return result;
        }
        boolean[] visited = new boolean[nums.length];
        
        dfs(nums, visited, new ArrayList<Integer>(), result);
        return result;
    }
    
    private void dfs(int[] nums, boolean[] visited, List<Integer> perm, List<List<Integer>> result) {
        
        if(perm.size() == nums.length) {
            result.add(new ArrayList<Integer>(perm));
        }
        
        for(int i = 0; i < nums.length; i++) {
            
            if(visited[i] == true) {
                continue;
            }
            
            perm.add(nums[i]);
            visited[i] = true;
            dfs(nums, visited, perm, result);
            perm.remove(perm.size() - 1);
            visited[i] = false;
        }
    }
```

#### [16. Permutations II](https://www.lintcode.com/problem/permutations-ii/description)

Description:

Given a list of numbers with duplicate number in it. Find all **unique** permutations.

Example:

```
Input: [1,2,2]
Output:
[
  [1,2,2],
  [2,1,2],
  [2,2,1]
]
```

Analysis:

DFS permutation template +  skipping the duplicate sibling branch horizontally.

```java
    public List<List<Integer>> permuteUnique(int[] nums) {
        
        List<List<Integer>> result = new ArrayList<>();
        
        if(nums == null) {
            return result;    
        }
        
        if(nums.length == 0) {
            result.add(new ArrayList<Integer>());
            return result;
        }
        
        Arrays.sort(nums);
        boolean[] visited = new boolean[nums.length];
        dfs(nums, visited, new ArrayList<Integer>(), result);
        
        return result;
    }
    
    private void dfs(int[] nums, boolean[] visited, List<Integer> perm, List<List<Integer>> result) {
        
        if(perm.size() == nums.length) {
            result.add(new ArrayList<Integer>(perm));
        }
        
        for(int i = 0; i < nums.length; i++) {
            
            if(visited[i]) {
                continue;
            }
            
            if(i != 0 && nums[i] == nums[i - 1] && visited[i - 1] == false) {
                continue;
            }
            
            visited[i] = true;
            perm.add(nums[i]);
            dfs(nums, visited, perm, result);
            visited[i] = false;
            perm.remove(perm.size() - 1);
        }
    }
```

#### [33. N-Queens](https://www.lintcode.com/problem/n-queens/description)

Descriptions:

The n-queens puzzle is the problem of placing n queens on an `n×n` chessboard such that no two queens attack each other(Any two queens can't be in the same row, column, diagonal line).

Given an integer `n`, return all distinct solutions to the n-queens puzzle.

Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'` each indicate a queen and an empty space respectively.

Example:

```
Input:4
Output:
[
  // Solution 1
  [".Q..",
   "...Q",
   "Q...",
   "..Q."
  ],
  // Solution 2
  ["..Q.",
   "Q...",
   "...Q",
   ".Q.."
  ]
]
```

Analysis: ***Transform a 2D choices into 1D permutation problem. We can think the special character's(Queen's) row number is the position. The special character's(Queen's) column number is the choice that each position can have.*** The question is pruning some selections from the decision tree. 

Pruning Condition:

1. Row: We can only put one queue in each row, therefore, in each position we can put only 1 col number on it. 
2. Column: Since we can not put the same queue in the same column, the former chosen column number can not appear again in the rest of the positions. 
3. Main Diagonal: For 2 points(x1, y1) and  (x2, y2) their subtraction can not be the same, that is to say, x1 - y1 != x2 - y2. To make sure no points are in the same main diagonal, we will save all x - y results using hash set. If there is any duplicate, the permutation can stop recursive build directly. 
4. Minor Diagonal: For 2 points(x1, y1) and  (x2, y2) their sum can not be the same, that is to say, x1 + y1 != x2 + y2. To make sure no points are in the same sub diagonal, we will save all x + y results using hash set. If there is any duplicate, the permutation can stop recursive build directly. 

Here are the decision tree without pruning condition 3 and 4.

![img](/asset/NQueens.jpg)

*This question modifies permutation template by only selecting some paths.*

```java
    public List<List<String>> solveNQueens(int n) {
        
        List<List<String>> result = new ArrayList<>();
        if(n <= 0) {
            return result;
        }
        
        List<List<Integer>> perms = new ArrayList<>();
        boolean[] visited = new boolean[n];
        Set<Integer> mainDiagonal = new HashSet<>();
        Set<Integer> subDiagonal = new HashSet<>();
        
        dfs(n, visited, mainDiagonal, subDiagonal, new ArrayList<Integer>(), perms);
        
        draw(perms, result);
        return result;
    }
    
    private void dfs(int n, boolean[] visited, Set<Integer> mainDiagonal, Set<Integer> subDiagonal, List<Integer> perm, List<List<Integer>> perms) {
        
        if(perm.size() == n) {
            perms.add(new ArrayList<Integer>(perm));
        }
        
        for(int i = 0; i < n; i++) {
            
            int col = perm.size();
            if(visited[i] || mainDiagonal.contains(i - col) || subDiagonal.contains(i + col)) {
                continue;
            }
            
            mainDiagonal.add(i - col);
            subDiagonal.add(i + col);
            visited[i] = true;
            perm.add(i);
            
            dfs(n, visited, mainDiagonal, subDiagonal, perm, perms);
            
            mainDiagonal.remove(i - col);
            subDiagonal.remove(i + col);
            visited[i] = false;
            perm.remove(perm.size() - 1);
        }
    }
    
    
    private void draw(List<List<Integer>> perms, List<List<String>> result) {
        
        for(List<Integer> pl : perms) {
            List<String> permString = new ArrayList<>();
            for(int i : pl) {
                StringBuilder sb = new StringBuilder();
                
                for(int j = 0; j < pl.size(); j++) {
                    if(j == i) {
                        sb.append("Q");
                    }else {
                        sb.append(".");
                    }
                }
                
                permString.add(sb.toString());
            }
            result.add(permString);
        }
    }
```

#### [34. N-Queens II](https://www.lintcode.com/problem/n-queens-ii/description)

Description: Follow up for [33. N-Queens](#33. N-Queens) problem. Now, instead outputting board configurations, return the total number of distinct solutions.

Example:

```
Input: n=4
Output: 2
Explanation:
1:
0 0 1 0
1 0 0 0
0 0 0 1
0 1 0 0
2:
0 1 0 0 
0 0 0 1
1 0 0 0
0 0 1 0
```

Analysis:

The main DFS function is the same as  [33. N-Queens](#33. N-Queens). The difference is, instead of drawing the result list out, we only need to return the permutation list's size.

```java
    public int totalNQueens(int n) {
        
        if(n <= 0) {
            return 0;
        }
        
        List<List<Integer>> perms = new ArrayList<>();
        boolean[] visited = new boolean[n];
        Set<Integer> mainDiagonal = new HashSet<>();
        Set<Integer> subDiagonal = new HashSet<>();
        
        dfs(n, visited, mainDiagonal, subDiagonal, new ArrayList<>(), perms);
        
        return perms.size();
        
    }
    
    private void dfs(int n, boolean[] visited, Set<Integer> mainDiagonal, Set<Integer> subDiagonal, List<Integer> perm, List<List<Integer>> perms) {
        
        if(perm.size() == n) {
            perms.add(new ArrayList<Integer>(perm));
        }
        
        for(int i = 0; i < n; i++) {
            
            int col = perm.size();
            if(visited[i] || mainDiagonal.contains(i - col) || subDiagonal.contains(col + i)) {
                continue;
            }
            
            perm.add(i);
            visited[i] = true;
            mainDiagonal.add(i - col);
            subDiagonal.add(i + col);
            
            dfs(n, visited, mainDiagonal, subDiagonal, perm, perms);
            perm.remove(perm.size() - 1);
            visited[i] = false;
            mainDiagonal.remove(i - col);
            subDiagonal.remove(i + col);
            
        }
    }
```

#### [816. Traveling Salesman Problem](https://www.lintcode.com/problem/traveling-salesman-problem/description)

Description:

Give `n` cities(labeled from `1` to `n`), and the undirected road's `cost` among the cities as a three-tuple `[A, B, c]`(i.e there is a road between city `A` and city `B` and the cost is `c`). We need to find the smallest cost to travel all the cities starting from 1.

Example 1

```plain
Input: 
n = 3
tuple = [[1,2,1],[2,3,2],[1,3,3]]
Output: 3
Explanation: The shortest path is 1->2->3
```

Notice:

1.A city can only be passed once.
2.You can assume that you can reach `all` the rest cities.

Analysis: 

- This question is a classical CS question. The question is pruning a permutation of 1 to n. Every two level has a cost. If there is not a route between 2 cities, the cost will be infinity. After comparing all paths, we can get the smallest cost. There is an extra pruning condition. If one path's partial cost is already bigger than paths' smallest cost calculated before, we don't need to calculate the total cost of this path any more. 

  Another key point of this question is to transform the input to a graph matrix. The input data format (start city, end city, cost) is not convenient to access the cost between 2 cities. 2 cities can be thought as nodes in a graph. The cost can be considered as weight in a graph. **We can save a graph into a matrix. The row index will be start city. The column index will be end city. The data in 2D array is the cost between 2 cities.** 

  From the decision tree, we can see, the permutation will start with 1 element and the first element can only be 1. Vertically, the permutation position will start from 2nd position which depth = 1. Here we can see horizontally and vertically all starts from 2nd position. Therefore, our index `i` will start from 2nd value which is 1.

![img](/asset/travellingSalesman.jpg)

*This problem modifies the permutation template by narrowing the index `i` range, which is starting the position from 2nd position.*

```java
public class Solution {
    /**
     * @param n: an integer,denote the number of cities
     * @param roads: a list of three-tuples,denote the road between cities
     * @return: return the minimum cost to travel all cities
     */
     
    
    private int minCost;
    
    public int minCost(int n, int[][] roads) {
        

        if(n <= 0 || roads.length == 0 || roads[0].length == 0) {
            return 0;
        }
        
        minCost = Integer.MAX_VALUE;
        
        int[][] graph = buildGraph(n, roads);
        

        
        boolean[] visited = new boolean[n];
        
        visited[0] = true; 
        dfs(n, graph, visited, 0, 0, 1);
        return minCost;
    }
    
    private void dfs(int n, int[][] graph, boolean[] visited, int cost, int startCity, int depth) {
        
        if(cost >= minCost) {
            return;
        }
        
        if(depth == n) {
            
            minCost = Math.min(minCost, cost);
            return;
        }
        
        for(int i = 0; i < n; i++) {
            
            if(visited[i] == true || graph[startCity][i] == Integer.MAX_VALUE) {
                continue;
            }
            
            visited[i] = true;
            dfs(n, graph, visited, cost + graph[startCity][i], i, depth + 1);
            visited[i] = false;
        }
        

    }
    
    private int[][] buildGraph(int n, int[][] roads) {
        
        int[][] graph = new int[n][n];
        
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < n; j++) {
                graph[i][j] = Integer.MAX_VALUE;
            }
        }
        
        for(int[] tuple : roads) {
            int a = tuple[0] - 1;
            int b = tuple[1] - 1;
            
            graph[a][b] = Math.min(graph[a][b], tuple[2]);
            graph[b][a] = Math.min(graph[b][a], tuple[2]);
        }
        
        return graph;
    }
}
```



- This question has another way of thought. The data structure will be more complicated, but decision tree will become smaller. As the decision tree below, for each node, we only need to collect its neighbors' cost. We can build a map (start city, (destination city, cost)). For each city, we need to loop all its neighbor cities. For each node in the for loop, its next level's loop collection is different.

  With this method, **graph will be saved as a map (city: neighbors).**

![img](/asset/TravelingSalesman.jpg)



```java
public class Solution {    
    private int minCost;
    
    public int minCost(int n, int[][] roads) {
        
        if(n <= 0 || roads == null || roads.length == 0 || roads[0].length == 0) {
            return 0;
        }
        minCost = Integer.MAX_VALUE;
        Map<Integer, List<Pair>> map = build(roads);
        boolean[] visited = new boolean[n + 1];
        visited[1] = true;
        List<Pair> connections = map.get(1);
        List<Integer> perm = new ArrayList<>();
        perm.add(1);
        
        dfs(n, map, connections, visited, 0, perm);
        
        return minCost;
        
    }
    

    private void dfs(int n, Map<Integer, List<Pair>> map, List<Pair> connections, boolean[] visited, int cost, List<Integer> perm) {
        
        if(perm.size() == n) {
            minCost = Math.min(minCost, cost);
        }
        
        
        for(Pair p : connections) {
            
            // System.out.println(p.city);
            if(visited[p.city] == true) {
                continue;
            }
            
            connections = map.get(p.city);
            //  System.out.println(Arrays.toString(connections.toArray()));
            perm.add(p.city);
            visited[p.city] = true;
            dfs(n, map, connections, visited, cost + p.cost, perm);
            perm.remove(perm.size() - 1);
            visited[p.city] = false;
    
        }
        
        
    }
    
    //build a map(starting city, list: all its connected cities)
    
    private Map<Integer, List<Pair>> build (int[][] roads) {
        
        Map<Integer, List<Pair>> map = new HashMap<>();
        
        for(int[] r : roads) {
            
            List<Pair> list0 = map.getOrDefault(r[0], new ArrayList<Pair>());
            list0.add(new Pair(r[1], r[2]));
            map.put(r[0], list0);
            
            List<Pair> list1 = map.getOrDefault(r[1], new ArrayList<Pair>());
            list1.add(new Pair(r[0], r[2]));
            map.put(r[1], list1);
        }
        
        return map;
    }
}

class Pair {
    int city;
    int cost;
    
    Pair(int city, int cost) {
        this.city = city;
        this.cost = cost;
    }
    
    public String toString() {
        return city + " ";
    }
}
```

#### [427. Generate Parentheses](https://www.lintcode.com/problem/generate-parentheses/description)

Descriptions:

Given n, and there are n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

Example 1:

```
Input: 3
Output: ["((()))", "(()())", "(())()", "()(())", "()()()"]
```

Example 2:

```
Input: 2
Output: ["()()", "(())"]
```

Analysis:

This question is to get all permutations of n pairs of '(' and ')'. The duplicate which means continuous '(' or ')' will only select the 1st element to enumerate. So the input shall be in order, open brackets shall be together and closed brackets shall be together. This question has another condition, which is, **To make sure a parentheses are valid, any time except the last time, open brackets must be >= close brackets.**

*There are 2 types of parentheses problem. One is generating all, which will use DFS. The other is validating parentheses, which will use stack to resolve.* 

Here is the decision tree, '(' marks as 0, and ')' marks as 1. 

 ![img](/asset/generateParenthesis.jpg)



*This problem modifies the permutation with duplicates template by selecting all paths whose left brackets >= right brackets all the time.*

```java
public List<String> generateParenthesis(int n) {
    
    List<String> result = new ArrayList<>();
    if(n <= 0) {
        return result;
    }
    
    StringBuilder sb = new StringBuilder();
    for(int i = 0; i < n; i++) {
        sb.append('(');
    }
    for(int i = 0; i < n; i++) {
        sb.append(')');
    }
    
    String parentheses = sb.toString();
    
    boolean[] visited = new boolean[n + n];
    StringBuilder perm = new StringBuilder();
    
    dfs(parentheses, visited, perm, result, 0, 0);
    return result;
    
}

private void dfs(String parentheses, boolean[] visited, StringBuilder perm, List<String> result, int right, int left) {
    
    if(right > left) {
        return;
    } 
    
    if(perm.length() == parentheses.length()) {
        result.add(perm.toString());
    }
    
    for(int i = 0; i < parentheses.length(); i++) {
        
        if(visited[i] == true) {
            continue;
        }
        
        if(i != 0 && parentheses.charAt(i) == parentheses.charAt(i - 1) && !visited[i - 1]) {
            continue;
        }
        
        char c = parentheses.charAt(i);
        visited[i] = true;
        if(c == '(') {
            perm.append('(');
            dfs(parentheses, visited, perm, result, right, left + 1);
            perm.deleteCharAt(perm.length() - 1);
        }else {
            perm.append(')');
            dfs(parentheses, visited, perm, result, right + 1, left);
             perm.deleteCharAt(perm.length() - 1);
        }
        visited[i] = false;
    }
}
```

#### [815. Course Schedule IV](https://www.lintcode.com/problem/course-schedule-iv/description)

There are a total of `n` courses you have to take, labeled from `0` to `n - 1`.
Some courses may have prerequisites, for example to take course `0` you have to first take course `1`, which is expressed as a pair: `[0,1]`

Given the total number of courses and a list of prerequisite pairs, return the number of different ways you can get all the lessons

Example 1

```plain
Input:
n = 2
prerequisites = [[1,0]]
Output: 1
Explantion:
You must have class in order 0->1.
```

Analysis:

The problem is asking to get 0 to n - 1 permutations with condition that all prerequisite courses are taken before the courses.  There are two ways that we can filter the non-workable conditions. 

Here is its decision tree.

![img](/asset/coursescheduleIV.jpg)

*This problem modifies permutation template by selecting all paths satisfying pre-requisite course orders.*

1. Every time when we add a new element into a set along one path vertically, we can check whether the set contains this new element's all prerequisite-course list . The checking list is getting longer and longer. Each element in the set will be checked multiple times. All elements in the set will be checked in average O(n ^ 2) (n + n - 1 + n - 2 + ... + 1)time, and each check will take O(n) time in worst case. Therefore the checking time of one path will be O(n ^ 3).

   *[Set often used functions](https://www.tinawang.ca/algo/2021/04/06/clean-code-practice.html)*

   *[For each parameter can not be null](https://www.tinawang.ca/algo/2021/04/06/clean-code-practice.html)*

   ```java
   public class Solution {
       /**
        * @param n: an integer, denote the number of courses
        * @param p: a list of prerequisite pairs
        * @return: return an integer,denote the number of topologicalsort
        */
        
       private int total;
       
       public int topologicalSortNumber(int n, int[][] p) {
           
           total = 0;
           
           if(p == null || p.length == 0) {
               return factorialUsingForLoop(n);
           }
           
           Map<Integer, List<Integer>> map = buildMap(p);
           
           boolean[] visited = new boolean[n];
           
           dfs(n, map, visited, new HashSet<Integer>());
           return total;
       }
       
       
       private void dfs(int n, Map<Integer, List<Integer>> map, boolean[] visited, Set<Integer> perm) {
           
           if(perm.size() == n) {
               total++;
           }
           
           
           for(int i = 0; i < n; i++) {
               
               if(visited[i]) {
                   continue;
               }
               
               if(map.get(i) != null && !perm.containsAll(map.get(i))) {
                   continue;
               }
   
               
               perm.add(i);
               visited[i] = true;
               dfs(n, map, visited, perm);
               perm.remove(i);
               visited[i] = false;
               
           }
       }
       
       private Map<Integer, List<Integer>> buildMap(int[][] p) {
           
           Map<Integer, List<Integer>> map = new HashMap<>();
           
           for(int[] pair : p) {
               
               List<Integer> pre = map.getOrDefault(pair[0], new ArrayList<>());
               pre.add(pair[1]);
               map.putIfAbsent(pair[0], pre);
               
           }
           
           return map;
       }
       
       public int factorialUsingForLoop(int n) {
           int fact = 1;
           for (int i = 2; i <= n; i++) {
               fact = fact * i;
           }
           return fact;
       }
   
   }
   ```

   

2. When we add an element, we can also remove this element from all prerequisite  sets that contain this element as a prerequisite course. When any course with 0 prerequisite course, we can add it onto the list. This is the topological sort, we need to remember every node's indegree (how many prerequisite courses not taken yet). This way, each element will be checked O(n) time, each check only takes O(1) time, and there are n elements. Therefore, it will take O(n ^ 2) time to check one path. Faster than the former one.

```java
public class Solution {
    /**
     * @param n: an integer, denote the number of courses
     * @param p: a list of prerequisite pairs
     * @return: return an integer,denote the number of topologicalsort
     */
     
    private int total;
    
    public int topologicalSortNumber(int n, int[][] p) {
        
        total = 0;

        boolean[] visited = new boolean[n];
        boolean[][] graph = new boolean[n][n];
        int[] inDegree = new int[n];
        
        build(n, p, graph, inDegree);
        
        dfs(n, graph, visited, inDegree, 0);
        return total;
    }
    
    
    private void dfs(int n, boolean[][] graph, boolean[] visited, int[] inDegree, int depth) {
        
        if(depth == n) {
            total++;
        }
        
        for(int i = 0; i < n; i++) {
            
            if(visited[i] || inDegree[i] != 0) {
                continue;
            }
            
            
            visited[i] = true;
            for(int j = 0; j < n; j++) {
                if(graph[i][j]) {
                    inDegree[j]--;
                }
            }
            dfs(n, graph, visited, inDegree, depth + 1);

            for(int j = 0; j < n; j++) {
                if(graph[i][j]) {
                    inDegree[j]++;
                }
            }
            visited[i] = false;
            
        }
    }
    
    private void build(int n, int[][] p, boolean[][] graph, int[] inDegree) {
        
        for(int[] a : p) {
            int x = a[1];
            int y = a[0];
            graph[x][y] = true;
            inDegree[a[0]]++;
        }
    }
    
    
}
```

*This question we use 2 ways to save a graph. Firstly, we save the graph by map(key: key's direct neighbors). Secondly, we use matrix to save any two courses' relationship.*

#### [795. 4-Way Unique Paths](https://www.lintcode.com/problem/4-way-unique-paths/description)

Analysis:

This question is also a permutation problem. The unique part is siblings in the horizontal level are not enumeration from 1 to n. It is the parent's node's 4 directions. You can not get siblings horizontally from each other. You can only get them from parent value. Here are the decision tree.

![img](/asset/4-unique-path.jpg)



The path we collect is the path from (0, 0) to (1, 2).

 For 2D permutation, the visited array will be a 2D array.  

In the solution, there is an interesting part. The variable `nx` and `ny`. Usually, we can change a variable inside `dfs` signature as an anonymous variable or before the recursion we change it, and after the recursion, we will revert it. Here,we can not update them as an anonymous variable in `dfs` signature because we need to use it before call `dfs` function. Neither, we don't need to revert the `nx` and `ny` after recursion because we did not use them before we change them. 

*This problem modifies permutation template by enumerating each node's siblings differently from 0 to n but 4 directions.*

```java
public class Solution {
    /**
     * @param m: the row
     * @param n: the column
     * @return: the possible unique paths
     */
     
     
    int total;
    
    public int uniquePaths(int m, int n) {
        if(m <= 0 || n <= 0) {
            return 0;
        }
        
        total = 0;
        
        boolean[][] visited = new boolean[m][n];
        
        visited[0][0] = true;
        
        dfs(m, n, 0, 0, visited);
        
        return total;
        
    }
    
    private void dfs(int m, int n, int x, int y, boolean[][] visited) {
        
        
        if(x == m - 1 && y == n - 1) {
            total++;
            return;
        }
        
        
        int[] dx = {0, 0, 1, -1};
        int[] dy = {1, -1, 0, 0};
        
        for(int i = 0; i < 4; i++) {
            
            int nx = x + dx[i];
            int ny = y + dy[i];


            
            if(nx < 0 || nx >= m || ny < 0 || ny >= n) {
                continue;
            }
            
            if(visited[nx][ny]) {
                continue;
            }
            
            visited[nx][ny] = true;
            dfs(m, n, nx, ny, visited);
            visited[nx][ny] = false;

            
        }
    }
}
```

#### [425. Letter Combinations of a Phone Number](https://www.lintcode.com/problem/letter-combinations-of-a-phone-number/solution)

Description:

Given a digit string excluded `0` and `1`, return all possible letter combinations that the number could represent.

A mapping of digit to letters (just like on the telephone buttons) is given below.

|   1    | 2 ABC | 3 DEF  |
| :----: | :---: | :----: |
| 4 GHI  | 5 JKL | 6 MNO  |
| 7 PQRS | 8 TUV | 9 WXYZ |

Example 1:

```
Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"]
Explanation: 
'2' could be 'a', 'b' or 'c'
'3' could be 'd', 'e' or 'f'
```

Although the answer above is in lexicographical order, your answer could be in any order you want.

Analysis:

The decision tree is here.

![img](/asset/letterComofaPhoneNum.jpg)

From the tree, we can see each new level of siblings are not repeated from last level. Horizontally, you only need to loop through 1 digit's all letters. Vertically, you need to move the position. This permutation won't need check whether a number is visited or not because each level's enumeration are in order, and you can not reverse the order.

*This problem modifies the permutation template by only enumerating each position by order already, which means, siblings in different level can not exchanged between each other .*

```java
    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        
        if(digits == null || digits.length() == 0) {
            return result;
        }
        
        Map<Character, List<String>> map = build();
        int digitLength = digits.length();
        StringBuilder sb = new StringBuilder();

        dfs(digits.toCharArray(), map, sb, result, 0);

        
        return result;
        
    }
    
    private void dfs(char[] digits, Map<Character, List<String>> map, StringBuilder sb, List<String> result, int index) {
        
        if(sb.length() == digits.length) {
             result.add(sb.toString());
             return;
        }
        
        List<String> cur = map.get(digits[index]);
       
        for(int i = 0; i < cur.size(); i++) {
            String s = cur.get(i);
            
            sb.append(s);
            dfs(digits, map, sb, result, index + 1);
            sb.deleteCharAt(sb.length() - 1);

        }

    }
    
    private Map<Character, List<String>> build() {
        
        Map<Character, List<String>> map = new HashMap<>();
        
        List<String> l2 = new ArrayList<>(Arrays.asList("a", "b", "c"));
        List<String> l3 = new ArrayList<>(Arrays.asList("d", "e", "f"));
        List<String> l4 = new ArrayList<>(Arrays.asList("g", "h", "i"));
        List<String> l5 = new ArrayList<>(Arrays.asList("j", "k", "l"));
        List<String> l6 = new ArrayList<>(Arrays.asList("m", "n", "o"));
        List<String> l7 = new ArrayList<>(Arrays.asList("p", "q", "r", "s"));
        List<String> l8 = new ArrayList<>(Arrays.asList("t", "u", "v"));
        List<String> l9 = new ArrayList<>(Arrays.asList("w", "x", "y", "z"));
        
        map.put('2', l2);
        map.put('3', l3);
        map.put('4', l4);
        map.put('5', l5);
        map.put('6', l6);
        map.put('7', l7);
        map.put('8', l8);
        map.put('9', l9);
        
        return map;
    }
```
<!--
#### [37. Sudoku Solver](https://leetcode.com/problems/sudoku-solver/)

Description:

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

Empty cells are indicated by the character `'.'`.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
A sudoku puzzle...

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
...and its solution numbers marked in red.

Note:

- The given board contain only digits `1-9` and the character `'.'`.
- You may assume that the given Sudoku puzzle will have a single unique solution.
- The given board size is always `9x9`.

Analysis:

This is a permutation problem. The decision tree is as below. However, the horizontal and vertical enumeration are different. Therefore, we don't need a visit array to remember the visited point.  Horizontally, each blank can be 1 to 9. Vertically, we need to enumerate all blanks. We will use recursion to visit a 2D array. When column reaches the board[0].length - 1, instead of returning the function to cease the recursion as 1D array, we will call a recursion on next row and  restart the column to 0. When the row reaches the last row as well, we will return a solution.

Another point to this question is how to exam the 3 X 3 sub boxes of the grid. 

First row / 3 and column / 3 can get the point 's row and column start in one of sub boxes. If we serialize the 9 units from 0 to 8. Any point's row index will serialized total / 3, and its column index will be serialized total % 3. In such way, you get 9 elements' index in one sub box. By adding the start point, we can get any sub box's index.

![img](/asset/sudokuSolver.jpg)

```java
class Solution {
    
    private boolean hasFound;
    public void solveSudoku(char[][] board) {
        if(board == null || board.length == 0) {
            return;
        } 
        
        dfs(board, 0, 0);
    }
    
    

    
    private void dfs(char[][] board, int x, int y) {
        
        if(y == board[0].length) {
            dfs(board, x + 1, 0);
            return;
        }
        
        if(x == board.length) {
            hasFound = true;
            return;
        }
        
        if(board[x][y] != '.') {
            dfs(board, x, y + 1);
        }
        

        for(char i = '1'; i <= '9'; i++){
            
            if(!isValid(board, x, y, i)) {
                continue;
            }
            

            board[x][y] = i;
            dfs(board, x, y + 1);
            if(hasFound) {
                return;
            }

            board[x][y] = '.';

        }
    }
    
    
    private boolean isValid(char[][] board, int x, int y, char target) {
        
        for(int i = 0; i < 9; i++) {
            if(board[i][y] == target) {
                return false;
            }
            
            if(board[x][i] == target) {
                return false;
            }
            
            if(board[(x / 3) * 3 + i / 3][(y / 3) * 3 + i % 3] == target) {
                return false;
            }
        }
        
        return true;
    }

}
```
 -->
Summery:

To solve DFS problems, 

- We shall draw the decision tree. the tree's horizontal enumeration will be one position's all possibilities. The vertical enumeration will be one solution. 
- We shall tell the question is a combination or a permutation problem. If a result's member changes its order and the result will count as another solution, it is a permutation problem. Otherwise, it is a combination problem. 
- The enumeration varies in different ways. It is not necessary always from 0 to n. It can be 4 directions of a point, neighbors of a node, and so on. The loop inside dfs will help to enumerate all possibilities. 
- The recursion can control the vertically loop by call another dfs or return. The for loop can control horizontally loop by conditionally call dfs recursion.

