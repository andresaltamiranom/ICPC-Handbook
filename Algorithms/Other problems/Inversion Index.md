# Inversion Index

## Problem Description

The Inversion index problem is defined as follows: Given a list of numbers, count the minimum number of ‘bubble sort’ swaps (swap between pair of consecutive items) that are needed to make the list sorted in (usually ascending) order.

For example, if the content of the list is {3, 2, 1, 4}, we need 3 ‘bubble sort’ swaps to make this list sorted in ascending order, i.e. swap (3, 2) to get {2, 3, 1, 4}, swap (3, 1) to get {2, 1, 3, 4}, and finally swap (2, 1) to get {1, 2, 3, 4}.

## Solution(s)

#### O(n^2) Solution

The most obvious solution is to count how many swaps are needed during the actual running of the O(n^2) bubble sort algorithm.

#### O(nlog(n)) solution

The better O(nlog(n)) Divide and Conquer solution for this inversion index problem is to modify merge sort. During the merge process of merge sort, if the front of the right sorted sublist is taken first rather than the front of the left sorted sublist, we say that an ‘inversion occurs’. Add inversion index counter by the size of the current left sublist. When merge sort is completed, report the value of this counter. As we only add O(1) steps to merge sort, this solution has the same time complexity as merge sort, i.e. O(nlog(n)).

On the example above, we initially have: {3, 2, 1, 4}. Merge sort will split this into sublist {3, 2} and {1, 4}. The left sublist will cause one inversion as we have to swap 3 and 2 to get {2, 3}. The right sublist {1, 4} will not cause any inversion as it is already sorted. Now, we merge {2, 3} with {1, 4}. The first number to be taken is 1 from the front of the right sublist. We have two more inversions because the left sublist has two members: {2, 3} that have to be swapped with 1. There is no more inversion after this. Therefore, there are a total of 3 inversions for this example.
