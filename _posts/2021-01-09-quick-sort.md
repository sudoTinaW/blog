---
published: true
layout: post
date: '2021-01-09 10:50:00 -0000'
categories: algo
---
Quick Sort is the sort algorithm widely used in Java and many other languages. Its average time complexity is the same as merge sort, but its space complexity is better than merge sort, which is in place. The main thought of partition is often asked in the interview questions.

##### Quick Sort:

- Time complexity: worst case is` O(n^2)`, and the best case is `O(nlogn)` when the partition is always by half. Average time complexity is `O(nlogn)`. The sort algorithm based on comparison best time complexity is `O(nlogn)`.

- Quick sort requires no extra space, it is the algorithm used in java

- The below template is one of the quick sort templates. 

  - Pivot must be an element in the array, otherwise data range might not get smaller for every recursive call.

  - We have to make sure`i` and `j` will cross after every recursive call so that we can decrease the recursive call's data range. 

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

  - When an element's value equals pivot, we will keep swapping. Such way, we can make all elements equal to the pivot eventually distributed on the left and right side of the place where `i` and `j` meet. Also, we can make the partition more evenly distributed, so that the algorithm's time complexity can be closer to the best case scenario. After partition,  all elements in `[left, j]` are `<= pivot`, and all elements in `[i, right]`are `>= pivot`. The elements `== pivot` are evenly scattered on both left and right sides.

  - When `i` and `j` both equal to the pivot, the template will keep swapping. The swapping itself doesn't have a meaning, but it will let `i` and `j` continue to move towards each other.

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
- Partition requires data with same property staying together. Therefore, it usually requires data swapping, and the quick sort without recursion can just accomplish it. Comparing to partition's left and right pointer algorithm, other left and right pointer algorithms usually solve searching for a group of elements issue and other left and right pointer algorithms' input usually have target values.
- Partition is usually using the quick sort template without recursion. Therefore,  most of time, the stop requirements don't need the left and right pointer cross-over, instead, if they can meet, the partition can stop.

### Problems:

#### [461. Kth Smallest Numbers in Unsorted Array](https://www.lintcode.com/problem/kth-smallest-numbers-in-unsorted-array/solution)

#### [31. Partition Array](https://www.lintcode.com/problem/partition-array/description)


#### [373. Partition Array by Odd and Even](https://www.lintcode.com/problem/partition-array-by-odd-and-even/description)

#### [49. Sort Letters by Case](https://www.lintcode.com/problem/sort-letters-by-case/description)

#### [148. Sort Colors](https://www.lintcode.com/problem/sort-colors/description)

#### [143. Sort Colors II](https://www.lintcode.com/problem/sort-colors-ii/description)

####  [144. Interleaving Positive and Negative Numbers](https://www.lintcode.com/problem/interleaving-positive-and-negative-numbers/description)

#### [96. Partition List](./LinkedList.md)

#### [98. Sort List](./LinkedList.md)