---
published: true
layout: post
date: '2020-06-11 14:50:00 -0000'
categories: algo
---
Binary Search main thought is decreasing the searching range by half for every loop. Its time complexity is O(logn).

1. What type of questions can be resolved by Binary Search?

   1. Given a **sorted** array and a target, to find any, last, first position of target in the array. 

    2. During a search, if you can leave/cut half of the input every time.

2. Time Complexity : O(logn)


        T(N) = T(n / 2) + O(1)
             = T(n / 4) + O(1) + O(1)
             = T(n / 8) + O(1) + O(1) + O(1)
             ...
             = T(1) + logn * O(1)
             = O(log n)

3. Iterative Template:

```java
public int binSearch(int[] nums, int target) {
	//start and end are both valid indexes
	int start = 0;
    int end = nums.length() - 1;
	// start and end will stop when they are next to each other. 
    //By doing this, it can avoid only 2 elements left, and the range (start ~ mid or mid ~ end)
    // doesn't decrease anymore issue. 
    while(start + 1 < end) {
    	int mid = start + (end - start) / 2;
        if(nums[mid] == target) {
        ...
        }else if(nums[mid] < target) {
        	start = mid;
        }else {
        	end = mid;
        }
    }
    // At the end, we have 2 elements left, start and end. 
    // Here we will seperately check which one is the answer
    if(nums[start] == target) {
    	return start;
    }
    if(nums[end] == target) {
    	return end;
    }
    
}
```

4. Recursion Template:	

```java
    int binSearch(int[] nums, int target, int start, int end) {
       if(start > end) {
        	return -1;
        }
        
        int mid = start + (end - start) / 2;
        
        if(nums[mid] == target) {
        	return mid;
        }
        
        if(nums[mid] < target) {
        	reutrn binSearch(nums, target, mid + 1, end);
        }
        
        return binSearch(nums, start, mid - 1, target);
    
    }
```


Iterative Vs Recursion

In practice, recursion is not recommended. It can easily cause stack overflow. The recursion way will only be used when the iterative way is too complicated to implement.

### Problems

#### [457 Classical Binary Search](https://www.lintcode.com/problem/classical-binary-search/description)

#### [14. First Position of Target](https://www.lintcode.com/problem/first-position-of-target/description)

#### [61. Search for a Range](https://www.lintcode.com/problem/search-for-a-range/description)

#### [60 Search Insert Postion](https://www.lintcode.com/problem/search-insert-position/description)

#### [28 Search a 2D Matrix](https://www.lintcode.com/problem/search-a-2d-matrix/description)

#### [38. Search a 2D Matrix II](https://www.lintcode.com/problem/search-a-2d-matrix-ii/description)

#### [74 First Bad Version](https://www.lintcode.com/problem/first-bad-version/description)

#### [162 Find Peak Element(Leetcode)](https://leetcode.com/problems/find-peak-element/) 

#### [585 Maximum Number in Moutain Sequence](https://www.lintcode.com/problem/maximum-number-in-mountain-sequence/description)

#### [159. Find Minimum in Rotated Sorted Array](https://www.lintcode.com/problem/find-minimum-in-rotated-sorted-array/description)

#### [62 Search in Rotated Sorted Array](https://www.lintcode.com/problem/search-in-rotated-sorted-array/description)

#### [63. Search in Rotated Sorted Array II](https://www.lintcode.com/problem/search-in-rotated-sorted-array-ii/description)

#### [**65. Median of 2 Sorted Arrays**](https://www.lintcode.com/problem/median-of-two-sorted-arrays/description)

#### [600 Smallest Rectangle Enclosing Black Pixels](https://www.lintcode.com/problem/smallest-rectangle-enclosing-black-pixels/description)

#### [460 Find K Closest Elements](https://www.lintcode.com/problem/find-k-closest-elements/description)
