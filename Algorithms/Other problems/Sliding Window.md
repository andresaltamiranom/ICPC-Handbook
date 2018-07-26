# Sliding Window

## Problem Description

There are several variants of Sliding Window problems. But all of them have similar basic idea: ‘Slide’ a sub-array (that we call a ‘window’, which can have static or dynamic length) in linear fashion from left to right over the original array of n elements in order to compute something. Some of the variants are:

1. Find the smallest sub-array size (smallest window length) so that the sum of the subarray is greater than or equal to a certain constant S in O(n). Examples:  For array A1 = {5, 1, 3, [5, 10], 7, 4, 9, 2, 8} and S = 15, the answer is 2 as highlighted. For array A2 = {1, 2, [3, 4, 5]} and S = 11, the answer is 3 as highlighted.

2. Find the smallest sub-array size (smallest window length) so that the elements inside the sub-array contains all integers in range [1..K]. Examples: For array A = {1, [2, 3, 7, 1, 12, 9, 11, 9, 6, 3, 7, 5, 4], 5, 3, 1, 10, 3, 3} and K = 4, the answer is 13 as highlighted. For the same array A = {[1, 2, 3], 7, 1, 12, 9, 11, 9, 6, 3, 7, 5, 4, 5, 3, 1, 10, 3, 3} and K = 3, the answer is 3 as highlighted.

3. Find the maximum sum of a certain sub-array with (static) size K. Examples: For array A1 = {10, [50, 30, 20], 5, 1} and K = 3, the answer is 100 by summing the highlighted sub-array. For array A2 = {49, 70, 48, [61, 60], 60} and K = 2, the answer is 121 by summing the highlighted sub-array.

4. Find the minimum of each possible sub-arrays with (static) size K. Example: For array A = {0, 5, 5, 3, 10, 0, 4}, n = 7, and K = 3, there are n−K +1 = 7−3+1 = 5 possible sub-arrays with size K = 3, i.e. {0, 5, 5}, {5, 5, 3}, {5, 3, 10}, {3, 10, 0}, and {10, 0, 4}. The minimum of each sub-array is 0, 3, 3, 0, 0, respectively.

## Solution(s)

We ignore the discussion of naïve solutions for these Sliding Window variants and go straight to the O(n) solutions to save space. The four solutions below run in O(n) as what we do is to ‘slide’ a window over the original array of n elements — some with clever tricks.

For variant number 1, we maintain a window that keeps growing (append the current element to the back—the right side—of the window) and add the value of the current element to a running sum or keeps shrinking (remove the front — the left side — of the window) as long as the running sum is ≥ S. We keep the smallest window length throughout the process and report the answer.

For variant number 2, we maintain a window that keeps growing if range [1..K] is not yet covered by the elements of the current window or keeps shrinking otherwise. We keep the smallest window length throughout the process and report the answer. The check whether range [1..K] is covered or not can be simplified using a kind of frequency counting. When all integers in [1..K] has non zero frequency, we said that range [1..K] is covered. Growing the window increases a frequency of a certain integer that may cause range [1..K] to be fully covered (it has no ‘hole’) whereas shrinking the window decreases a frequency of the removed integer and if the frequency of that integer drops to 0, the previously covered range [1..K] is now no longer covered (it has a ‘hole’).

For variant number 3, we insert the first K integers into the window, compute its sum, and declare the sum as the current maximum. Then we slide the window to the right by adding one element to the right side of the window and removing one element from the left side of the window — thereby maintaining window length to K. We add the sum by the value of the added element minus the value of the removed element and compare with the current maximum sum to see if this sum is the new maximum sum. We repeat this window-sliding process n − K times and report the maximum sum found.

Variant number 4 is quite challenging especially if n is large. To get O(n) solution, we need to use a _deque_ (double-ended queue) data structure to model the window. This is because deque supports efficient — O(1) — insertion and deletion from front and back of the queue. This time, we maintain that the window (that is, the deque) is sorted in ascending order, that is, the front most element of the deque has the minimum value. However, this changes the ordering of elements in the array. To keep track of whether an element is currently still inside the current window or not, we need to remember the index of each element too. The detailed actions are best explained with the C++ code below. This sorted window can shrink from both sides (back and front) and can grow from back, thus necessitating the usage of deque data structure.

```cpp

void SlidingWindow(int A[], int n, int K) {
	// ii---or pair<int, int>---represents the pair (A[i], i)
	deque<ii> window; // we maintain ‘window’ to be sorted in ascending order
	for (int i = 0; i < n; i++) { // this is O(n)
		while (!window.empty() && window.back().first >= A[i])
			window.pop_back(); // this to keep ‘window’ always sorted
		
		window.push_back(ii(A[i], i));
		
		// use the second field to see if this is part of the current window
		while (window.front().second <= i - K) // lazy deletion
			window.pop_front();

		if (i + 1 >= K) // from the first window of length K onwards
			printf("%d\n", window.front().first); // the answer for this window
} }

```
