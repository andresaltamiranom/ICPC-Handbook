# Sorting in Linear Time

## Problem Description

Given an (unsorted) array of n elements, can we sort them in O(n) time?

## Theoretical Limit

In general cases, the lower bound of generic-comparison-based-sorting algorithm is Ω(n * log(n)). However, if there is a
special property about the n elements, we can have a faster, linear, O(n) sorting algorithm by not doing comparison between elements. We will see two examples below.

## Solution(s)

### Counting Sort

If the array A contains n integers with a small range [L..R] (e.g. ‘human age’ of [1..99] years), we can use the Counting Sort algorithm. For the explanation below, assume that array A is {2, 5, 2, 2, 3, 3}. The idea of Counting Sort is as follows:

1. Prepare a ‘frequency array’ f with size k = R-L+1 and initialize f with zeroes. On the example array above, we have L=2, R=5, and k=4.

2. We do one pass through array A and update the frequency of each integer that we see, i.e. for each i ∈ [0..n-1], we do f[A[i]-L]++. On the example array above, we have f[0] = 3, f[1] = 2, f[2] = 0, f[3] = 1.

3. Once we know the frequency of each integers in that small range, we compute the prefix sums of each i, i.e. f[i] = [f-1] + f[i] ∀i ∈ [1..k-1]. Now, f[i] contains the number of elements less than or equal to i. On the example array above, we have f[0] = 3, f[1] = 5, f[2] = 5, f[3] = 6.

4. Next, go backwards from i = n-1 down to i=0. We place A[i] at index f[A[i]-L]-1 as it is the correct location for A[i]. We decrement f[A[i]-L] by one so that the next copy of A[i] - if any — will be placed right before the current A[i]. On the example array above, we first put A[5] = 3 in index f[A[5]-2]-1 = f[1]-1 = 5-1 = 4 and decrement f[1] to 4. Next, we put A[4] = 3 — the same value as A[5] = 3 — now in index f[A[4]-2]-1 = f[1]-1 = 4-1 = 3 and decrement f[1] to 3. Then, we put A[3] = 2 in index f[A[3]-2]-1 = 2 and decrement f[0] to 2. We repeat the next three steps until we obtain a sorted array: {2, 2, 2, 3, 3, 5}.

The time complexity of Counting Sort is O(n + k). When k = O(n), this algorithm theoretically runs in linear time by not doing comparison of the integers. However, in programming contest environment, usually k cannot be too large in order to avoid Memory Limit Exceeded. For example, Counting Sort will have problem sorting this array A with n = 3 that contains {1, 1000000000, 2} as it has a large k.

### Radix Sort

If the array A contains n non-negative integers with relatively wide range [L..R] but it has a relatively small number of digits, we can use the Radix Sort algorithm.

The idea of Radix Sort is simple. First, we make all integers have d digits — where d is the largest number of digits in the largest integer in A — by appending zeroes if necessary. Then, Radix Sort will sort these numbers digit by digit, starting with the least significant digit to the most significant digit. It uses another stable sort algorithm as a sub-routine to sort the digits, such as the O(n + k) Counting Sort shown above. For example:

![Radix Sort](https://i.imgur.com/Ju6AvnO.png)

For an array of n d-digits integers, we will do O(d) passes of Counting Sorts which have time complexity of O(n + k) each. Therefore, the time complexity of Radix Sort is O(d * (n + k)). If we use Radix Sort for sorting n 32-bit signed integers (≈ d = 10 digits) and k = 10. This Radix Sort algorithm runs in O(10 * (n + 10)). It can still be considered as running in linear time but it has a high constant factor.

Considering the hassle of writing the complex Radix Sort routine compared to calling the standard O(n * log(n)) C++ STL sort (or Java Collections.sort), this Radix Sort algorithm is rarely used in programming contests.
