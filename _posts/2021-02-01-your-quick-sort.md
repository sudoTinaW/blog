---
published: true
layout: post
date: '2020-02-01 10:50:00 -0000'
categories: quick sort
---
Quick Sort is the sort algorithm widely used in Java and many other languages. Its average time complexity is the same as merge sort, but its space complexity is better than merge sort, which is in place. The main thought of partition is often asked in the interview questions.

##### Quick Sort:

- Time complexity: worst case is` O(n^2)`, and the best case is `O(nlogn)` when the partition is always by half. Average time complexity is `O(nlogn)`. The sort algorithm based on comparison best time complexity is `O(nlogn)`.

- Quick sort requires no extra space, it is the algorithm used in java

- The below template is one of the quick sort templates. 

  - Pivot must be an element in the array, otherwise it may lead to data range can not get smaller for every recursive call.

  - We have to make sure`i` and `j` will cross after every recursion call so that we can decrease the recursive call's data range. 

  - ###### 4 cases of the stop position

    - `i and j` next to each other:
      - move one step: it will transfer to `i and j` points to the same element.
      - move two steps:`i and j` will cross over as `{A[j],A[i]}`

    - `i and j` points to the same element:
      - move one step: `i and j` will cross over as `{A[j],A[i]}`
      - move two steps: it can only happens when `A[i] == A[j] == pivot`, and the final position will be `{A[j], pivot, A[i]}`

    As described above, this template can combine 4 situations into 2 conditions,`i` and `j` will only stop under 2 conditions, 

    - `{A[j],A[i]}`
    - `{A[j], pivot, A[i]}` 

  - From above, we can see quick sort can divide the input into 2 parts or 3 parts. Therefore, the base case of recursion is `start > end` and `start == end`.

  - When an element's value equals pivot, we will keep swapping. Such way, we can make all elements equals pivot eventually distributed on the left and right side of the place that `i` and `j` meet, which also means, we can make the partition more evenly distribute, so that the algorithm's time complexity can be closer to the best case scenario. After partition,  all elements in `[left, j]` are `<= pivot`, and all elements in `[i, right]`are `>= pivot`. The elements `== pivot` are evenly scattered on both left and right sides.

  - When `i` and `j` both equal to pivot, the template will keep swapping. The swapping itself doesn't have a meaning, but it will let `i` and `j` continue to move towards each other.

```java
    public void sortIntegers2(int[] A) {
        
        sort(A, 0, A.length - 1);
    }
    
    private void sort(int[] A, int start, int end) {
        
        if(start >= end) {
            return;
        }
        
        int i = start;
        int j = end;
        
        int pivot = A[start + (end - start) / 2];
        
        while(i <= j) {
            
            while(i <= j && A[i] < pivot) {
                i++;
            }
            
            while(i <= j && A[j] > pivot) {
                j--;
            }
            
            if(i <= j) {
                int temp = A[i];
                A[i] = A[j];
                A[j] = temp;
                i++;
                j--;
            }
        }
        
        sort(A, start, j);
        sort(A, i, end);
    }
```

##### Partition:

- Partition is an extension from Quick Sort. Quick Sort is also a type of two pointer algorithm. The pointers start from both ends of the array, and stop when two pointers meet each other.
- Partition requires data with same property staying together. Therefore, it usually requires data swapping, and the quick sort without recursion can just accomplish it. Comparing to partition's left and right pointer algorithm, other left and right pointer algorithm usually solves searching for a group of elements issue, and input usually contains a target value.
- Partition is usually using the quick sort template without recursion. Therefore,  most of time, the stop requirements don't need the left and right pointer crossing over, instead, if they can meet, the partition can stop.

### Problems:

###### [461. Kth Smallest Numbers in Unsorted Array](https://www.lintcode.com/problem/kth-smallest-numbers-in-unsorted-array/solution)

Description:

Find the kth smallest number in an unsorted integer array.

Example 1:

```
Input: [3, 4, 1, 2, 5], k = 3
Output: 3
```

Example 2:

```
Input: [1, 1, 1], k = 2
Output: 1
```

Analysis:

This question can sort the array and get the k element. It can also be resolved by heap in O(nlogk). Here we will only describe the **quick select** method. 

- Quick select is an extension from quick sort. Quick sort is sorting both sides of pivot until all elements are sorted. Quick select is only sorting the side which contains k position. 

- Like the quick sort's time complexity, quick select's time complexity depends on how average we can divide the input. The best time complexity, which is also average time complexity, is, 

$$
O(n + \frac{n}{2} + \frac{n}{4} + ... + 1) = O(n\frac{1 - \frac{1}{2} ^ n}{1 - \frac{1}{2}}) = 
\lim\limits_{n \to \infty}2n(1 - \frac{1}{2} ^ n) = O(2n) = O(n)
$$



- From the quick sort's template we have already known that`i and j` will stop in 2 conditions, `{A[j],A[i]}` and`{A[j], pivot, A[i]}` .  

  -  `{A[j],A[i]}` : if the k is in `[start, A[j]]`, or `[A[i], end]` we can continue to call recursion.
  -  `{A[j], pivot, A[i]}` : if the k is in `[start, A[j]]`, or `[A[i], end]` we can continue to call recursion, and if `k == pivot`, we can directly return.

  Therefore, we have 3 conditions, `k <= j` , `k == pivot`, and `k >= i`. When `k == pivot`, we find the k smallest. When `k <= j` and  `k >= i`, we will continue to call recursion.

- The base case of recursion will be `start == end`.

  -  If we have 3 elements left, `{A[j], pivot, A[i]}` . It will either directly return, or become `A[j]` or `A[i]` , in such situation, the return condition will be when `start == end`.
  -  If we have 2 elements left, `{A[j],A[i]}`. The k will either `A[j]` or `A[i]` , in such situation, the return condition will be when `start == end` as well.

  ```java
      public int kthSmallest(int k, int[] nums) {
          
          if(nums == null || nums.length == 0 || k <= 0 || k > nums.length) {
              return Integer.MIN_VALUE;
          }
          
          
          return quickSelect(k - 1, nums, 0 , nums.length - 1);
          
      }
      
      private int quickSelect(int k, int[] nums, int start, int end) {
          if(start == end) {
              return nums[start];
          }
          
          int i = start;
          int j = end;
          
          int pivot = start + (end - start) / 2;
          
          while(i <= j) {
              
              while(i <= j && nums[i] < pivot) {
                  i++;
              }
              while(i <= j && nums[j] > pivot) {
                  j--;
              }
              
              if(i <= j) {
                  int temp = nums[i];
                  nums[i] = nums[j];
                  nums[j] = temp;
                  i++;
                  j--;
              } 
          }
          
          if(k > j && k < i) {
              return nums[k];
          }
          
          if(k <= j) {
              return quickSelect(k, nums, start, j);
          }else {
              return quickSelect(k, nums, i, end);
          }
          
      }
  ```

###### [31. Partition Array](https://www.lintcode.com/problem/partition-array/description)

Description:

Given an array `nums` of integers and an `int k`, partition the array (i.e. move the elements in `nums`) such that:

- All elements < *k* are moved to the *left*
- All elements >= *k* are moved to the *right*

Return the partitioning index, i.e. the first index` i nums[i] >= k`.

Example:

```
Input:
[3,2,2,1],2
Output:1
Explanation:
the real array is[1,2,2,3].So return 1
```

Challenge:

Can you partition the array in-place and in O(n)?

Notice:

You should do really partition in array `nums` instead of just counting the numbers of integers smaller than k.

If all elements in `nums` are smaller than *k*, then return `nums.length`

Analysis:

- k is not required to be one element in the input because we don't need to call recursion in this question. If the input size can not decrease, we will return the length of input as question required.

- The question requires the first index making `nums[i] >= k`. Therefore, all elements equal to pivot shall be on the right side of `i, and j` meeting place. We will only let `i` pointer stops when an element equals to the pivot. 

- This question doesn't need recursion, the two pointer can stop at cross over or stop before cross over. To consider whether two pointer needs to stop at cross over or stop before cross over, we shall think of when the two pointer are pointing to the same element, the 3 conditions  `< pivot`, `== pivot`, and `> pivot`, which an element can be. When `element < pivot `, and we don't let the two pointer cross over, we shall return `i + 1`. When `element >= pivot`, and we don't let the two pointer cross over, we shall return `i`.  Meanwhile, when `element < pivot` and `element >= pivot`, and we let the two element crossed, we can both return `i`. Therefore, we will let the two pointer stops at the cross over.

  ```java
  public int partitionArray(int[] nums, int k) {
      if(nums == null || nums.length == 0) {
          return 0;
      }
  
      int i = 0;
      int j = nums.length - 1;
  
      while(i <= j) {
  
          while(i <= j && nums[i] < k) {
              i++;
          }
          while(i <= j && nums[j] >= k) {
              j--;
          }
  
          if(i <= j) {
              int temp = nums[i];
              nums[i] = nums[j];
              nums[j] = temp;
              i++;
              j--;
          }
      }
  
      return i;
  
  
  }
  ```

  

###### [373. Partition Array by Odd and Even](https://www.lintcode.com/problem/partition-array-by-odd-and-even/description)

Description:

Partition an integers array into odd number first and even number second.

Example :

```plain
Input: [1,4,2,3,5,6]
Output: [1,3,5,4,2,6]
```

Do it in-place.

Notice:

The answer is not unique.

Analysis:

It is a partition question. We can use quick sort template without recursion. Change the condition in while and if statement to whether an element is an odd number or even number. This question doesn't need recursion, the two pointer can stop at cross over or stop before cross over. When they are pointing to the same element, no matter when element is`< pivot`, `== pivot`, and `> pivot,` there is no difference between stop before cross over or stop after cross over. Therefore, we can stop before cross over.

```java
    public void partitionArray(int[] nums) {
        if(nums == null || nums.length == 0) {
            return;
        }
        
        int left = 0;
        int right = nums.length - 1;
        
        while(left < right) {
            
            while(left < right && nums[left] % 2 == 1) {
                left++;
            }
            while(left < right && nums[right] % 2 == 0) {
                right--;
            }
            
            if(left < right) {
                int temp = nums[left];
                nums[left] = nums[right];
                nums[right] = temp;
                left++;
                right--;
            }
        }
    }
```

###### [49. Sort Letters by Case](https://www.lintcode.com/problem/sort-letters-by-case/description)

Description:

Given a string which contains only letters. Sort it by lower case first and upper case second.

Example

```
Example 1:
	Input:  "abAcD"
	Output:  "acbAD"

Example 2:
	Input: "ABC"
	Output:  "ABC"
	
```

Challenge:

Do it in one-pass and in-place.

Notice:

It's *NOT* necessary to keep the original order of lower-case letters and upper case letters.

Analysis:

It is the exact same thought as [373. Partition Array by Odd and Even](#373. Partition Array by Odd and Even).

```java
public void sortLetters(char[] chars) {
    if(chars == null || chars.length == 0) {
        return;
    }

    int left = 0;
    int right = chars.length - 1;

    while(left < right) {

        while(left < right && Character.isLowerCase(chars[left])) {
            left++;
        }

        while(left < right && Character.isUpperCase(chars[right])) {
            right--;
        }

        if(left < right) {
            char temp = chars[left];
            chars[left] = chars[right];
            chars[right] = temp;
            left++;
            right--;
        }
    }
}
```



###### [148. Sort Colors](https://www.lintcode.com/problem/sort-colors/description)

Description:

Given an array with *n* objects colored *red*, *white* or *blue*, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.

Here, we will use the integers `0`, `1`, and `2` to represent the color red, white, and blue respectively.

Example 1

```
Input : [1, 0, 1, 2]
Output : [0, 1, 1, 2]
Explanation : sort it in-place
```

A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.

Could you come up with an one-pass algorithm using only constant space?

You are not suppose to use the library's sort function for this problem.
You should do it in-place (sort numbers in the original array).

Analysis:

If we use quick sort partition once, we can only sort the input into `<= pivot`, and `> pivot` or `< pivot`, and `>= pivot`. If we want to partition into 3 parts in one pass. We have to split the `< and == pivot` situation at the same time. Therefore, we can combine fast and slow pointer with quick sort two pointer partition. Here, we need to check whether fast pointer needs to swap with slow pointer whenever fast pointer moves. What is more, in quick sort,  left (fast) pointer can move in both `<= pivot` or after swap.  Since it is not necessary to move both pointers after swap, we can skip move left pointer to get a cleaner code.

```java
    public void sortColors(int[] nums) {
       
       if(nums == null || nums.length == 0) {
           return;
       }
       
       int slow = 0;
       int leftFast = 0;
       int right = nums.length - 1;
       
       while(leftFast <= right) {
           while(leftFast <= right && nums[leftFast] <= 1) {
               if(nums[slow] > nums[leftFast]) {
                   swap(slow, leftFast, nums);
               }
               leftFast++;
               if(nums[slow] == 0) {
                   slow++;
               }
             
           }
           
           while(leftFast <= right && nums[right] > 1) {
               right--;
           }
           
           if(leftFast <= right) {
               swap(leftFast, right, nums);
               right--;
           }
       }
    }
    
    private void swap(int a, int b, int[] nums) {
      int temp = nums[a];
      nums[a] = nums[b];
      nums[b] = temp;
    }
```



###### [143. Sort Colors II](https://www.lintcode.com/problem/sort-colors-ii/description)

Description:

Given an array of *n* objects with *k* different colors (numbered from 1 to k), sort them so that objects of the same color are adjacent, with the colors in the order 1, 2, ... k.

Example1:

```
Input: 
[3,2,2,1,4] 
4
Output: 
[1,2,2,3,4]
```

Example2:

```
Input: 
[2,1,1,2,2] 
2
Output: 
[1,1,2,2,2]
```

Challenge:

A rather straight forward solution is a two-pass algorithm using counting sort. That will cost O(k) extra memory. Can you do it without using extra memory?

Notice:

1. You are not suppose to use the library's sort function for this problem.
2. `k` <= `n`

Analysis:

This question can be resolved by 4 difference ways,

- quick sort: Time `O(nlogn)`, Space `O(logn)`
- counting sort: Time `O(n)`, Space `O(k)`
- k times partition, one color per partition: Time `O(nk)`, Space` O(1)`
- Divide&Conquer on k colors: Time `O(nlogk)` Space `O(logk)` (when k is way smaller than n, this is the best solution)

Here we will only describe the Divide & Conquer solution. If k is way smaller than n and we can skip sort the same color elements, we can save a lot of time. 

Similar to merge sort, first we shall divide the input into 2 parts, the color `<= k/2` and the color `> k/2`. The difference between merge sort and this question is that merge sort divides the input naturally by the mid index, and takes most time on merge step, but this question needs partition the array into `<= k/2` and `> k` part, and merge can be done naturally. 

It looks more like quick sort. The difference between the quick sort and this question is, we shall let pivot scatter only on one side instead of evenly distribute on both sides. That is because we want to stop when one color stays together. If we let the pivot evenly distribute on both sides, the stop will be when there is only one element left. What is more, in the quick sort, we want the pivot  evenly distribute on both sides because we want to divide the input more evenly so that the time complexity can approach to `O(nlogn)`. However, if k is way smaller than n, even the worst case `O(nk)` will be better than `O(nlogn)`. Therefore, we will let pivot scatter on one side rather than on both sides. Therefore the range for one recursion will be `[startColor, pivot]` and `[pivot + 1, endColor]`.



```java
public void sortColors2(int[] colors, int k) {
    sort(colors, 1, k, 0, colors.length - 1);
}

private void sort(int[] colors, int startColor,  int endColor, int start, int end) {
    if(startColor >= endColor) {
        return;
    }



    int left = start;
    int right = end;
    int pivot = (startColor + endColor) / 2;

    while(left <= right) {

        while(left <= right && colors[left] <= pivot) {
            left++;
        }

        while(left <= right && colors[right] > pivot) {
            right--;
        }

        if(left <= right) {
            int temp = colors[left];
            colors[left] = colors[right];
            colors[right] = temp;
            left++;
            right--;
        }
    }

    sort(colors, startColor, pivot, start, right);
    sort(colors, pivot + 1, endColor, left, end);
}
```







######  [144. Interleaving Positive and Negative Numbers](https://www.lintcode.com/problem/interleaving-positive-and-negative-numbers/description)

Description: 

Given an array with positive and negative integers. Re-range it to interleaving with positive and negative integers. The number of positive and negative integers are equal or one different.

Example 1:

```
Input : [-1, -2, -3, 4, 5, 6]
Outout : [-1, 5, -2, 4, -3, 6]
Explanation :  any other reasonable answer.
```

Notice:

Do it in-place and without extra memory.

You are not necessary to keep the original order of positive integers or negative integers.

Analysis:

This question can be resolved by two ways. The first way is using partition first, the second way is using 2 pointers on 2 arrays,

- We can partition the input by positive and negative integers first, and then re-arrange elements with positive or negative number interleaved. 
- We can let one pointer only check odd position, and one pointer only check even position. If numbers in odd and even don't match the sign, we will let them swap. This way is more like  we have 2 imaginary arrays, if their elements are not qualified, we will let them swap. As long as we finish one array, the loop can be stop.

One more thing needs to be consider, if positive integer is one more than negative integer, we shall place positive number first. Otherwise, if negative integer is one more than positive integer, we shall place negative integer first. If the number of positive and negative integers are equal, either positive or negative integer can be placed as first.

We will only posts the second way,

```java
public void rerange(int[] A) {

    if(A == null || A.length == 0) {
        return;
    }


    int pos = 0;
    int neg = 0;

    for(int a : A) {
        if(a > 0) {
            pos++;
        }else {
            neg++;
        }
    }


    int postStart = 1;
    int negStart = 1;

    if(pos > neg) {
        postStart = 0;
    }else {
        negStart = 0;
    }


    int i = postStart;
    int j = negStart;

    while(i < A.length && j < A.length) {

        while(i < A.length && (i % 2 == postStart && A[i] > 0)) {
            i += 2;
        }

        while(j < A.length && (j % 2 == negStart && A[j] < 0)) {
            j += 2;
        }

        if(i < A.length && j < A.length) {
            int temp = A[i];
            A[i] = A[j];
            A[j] = temp;

            i += 2;
            j += 2;
        }

    }

}
```

#####
