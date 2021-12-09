---
published: true
layout: post
date: '2020-10-20 14:50:00 -0000'
categories: algo
---
Sliding Window often applies on finding consecutive elements. When we need to find subarray or substring, we shall try sliding window technique first. Its has 2 difficulties, one is how to prove a question can use this technique. The other is how to implement it.

- How to prove a question can use sliding window technique:
  Sliding window is actually a greedy algorithm. To proof the current best choice can make the final best choice is the key thought of deciding whether we can use this technique for a question. Here we will offer the detailed proof, and offer a conclusion to help you quickly decide whether we can use this algorithm to solve the problem.

  - The left pointer moves when a window **qualifies** the question's requirement to get the **min** window.

    When the right pointer moves to a new position,

    If the window has qualified the requirement, we shall shrink the window to find the min. There is no need to enlarge the window by moving the left pointer backward.

    If the window disqualified the requirement, any point before the left pointer can not build a min valid window with the current right pointer. That is because any point before the left pointer must be able to build a valid window with a point before the current right pointer, and with the current right pointer the window can only get bigger, which can not be min window any more.

  - The left pointer moves when a window **disqualifies** the question's requirement to get the **max** window.

    When the right pointer moves to a new position,

    If the window still qualifies the requirement, any point before the left pointer can not make a valid window with the current right pointer. Thus the left pointer won't need to go backward.

    If the window disqualifies the requirement, we shall shrink the window to make it qualified instead of enlarging the window by moving the left pointer backwards.

  - The left pointer moves when a window **qualifies** the question's requirement to get a **fixed** window.

    When the right pointer moves to a new position,

    If the window has not reached to the fixed size, the left pointer will stay at the beginning, which is not possible to move backward.

    if the window has reached to the fixed size, the left pointer can only go one forward to maintain the window size, which is not possible to move backwards either.

- How to implement:

  We have a template as following,

  - Right and left pointer in this template will both stop at the final result + 1 position. For the right pointer, since substring function requires[ ) boundary, the final result + 1 is what we need. For the left pointer, if we we need to collect its result inside while loop we need to collect it before the left pointer is updated.

  - If the left pointer can only move one step forward at most for every new right pointer, which is a fixed size window situation, we can change the inner `while` loop to `if `condition.

  - The left pointer moves when a window **qualifies** the question's requirement to get the **min/ fixed** window. Therefore, the inner loop condition shall be when window qualifies the question's requirement.

  - The left pointer moves when a window **disqualifies** the question's requirement to get the **max** window, the inner loop condition shall be when window disqualifies the question's requirement. Therefore, the inner loop condition shall be when window disqualifies the question's requirement.

  - The result will always be updated when the window qualifies the question's requirement. When the left pointer moves when a window qualifies the question's requirement, the result will be updated inside the left pointer's move condition. When the left pointer moves when a window disqualifies the question's requirement, the result will be updated outside the left pointer's while loop whenever a right pointer moves and left and updates to a new valid window.

    

  ```java
  /* 滑动窗口算法框架 */
  void slidingWindow(string s, string t) {
      int left = 0, right = 0;
      
      //统计合格的字符个数， 用以判断一个window是否包含所有要求字符
      int valid = 0; 
      while (right < s.size()) {
          // c 是将移入窗口的字符
          char c = s[right];
          // 右移窗口
          right++;
          // 进行窗口内数据的一系列更新
          ...
  
          /*** debug 输出的位置 ***/
          /********************/
  
          // 判断左侧窗口是否要收缩
          while (window needs shrink) {
              //collect result
              ...
              // d 是将移出窗口的字符
              char d = s[left];
              // 左移窗口
              left++;
              // 进行窗口内数据的一系列更新
              ...
        }
      }
  }
  ```

  

### Problems

#### [32. Minimum Window Substring](https://www.lintcode.com/problem/minimum-window-substring/solution)

#### [604. Window Sum](https://www.lintcode.com/problem/window-sum/solution)

####  [1169. Permutation in String](https://www.lintcode.com/problem/permutation-in-string/description)

#### [647. Find All Anagrams in a String](https://www.lintcode.com/problem/find-all-anagrams-in-a-string/description)

#### [384. Longest Substring Without Repeating Characters](https://www.lintcode.com/problem/longest-substring-without-repeating-characters/description)

#### [386. Longest Substring with At Most K Distinct Characters](https://www.lintcode.com/problem/longest-substring-with-at-most-k-distinct-characters/solution)

#### [1859 · Minimum Amplitude](https://www.lintcode.com/problem/1859/)
