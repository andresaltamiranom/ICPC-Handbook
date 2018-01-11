# Maximum Subarray Sum

Given an array of integers (positive, negative, or zero), find the subarray which sums up to the maximum possible value.

The first function _maximumSubarray()_ assumes a normal array and _maxCircularSum()_ assumes a circular array.

### maximumSubarray

```cpp

int maximumSubarray(int numbers[], int N) {
	int maxSoFar = numbers[0], maxEndingHere = numbers[0];
	FOR(i, 1, N) {
		if (maxEndingHere < 0) maxEndingHere = numbers[i];
		else maxEndingHere += numbers[i];
		maxSoFar = max(maxEndingHere, );
	}
	return maxSoFar;
}

```

**Input:** An array of integers and the size of the array.

**Output:** The maximum subarray sum.

We can find the maximum subarray sum using Kadane's algorithm. Kadane's algorithm looks for all positive, contiguous segments of the array (_maxEndingHere_ is used for this). We don't stop when we find a negative number. We stop when the current sum turns negative. Then we keep track of the maximum sum segment among all positive segments (_maxSoFar_ is used for this). Each time we find a positive sum, we compare it with _maxSoFar_ and update _maxSoFar_ if _maxEndingHere_ is greater than _maxSoFar_.

Once we've looped through the whole array, we just return the value stored at _maxSoFar_.

### maxCircular

```cpp

int maxCircularSum (int a[], int n) {
   int max_kadane = maximumSubarray(a, n);
   int max_wrap  =  0;
   FOR(i, 0, n) {
        max_wrap += a[i];
        a[i] = -a[i];
   }
   max_wrap = max_wrap + maximumSubarray(a, n);
   return (max_wrap > max_kadane) ? max_wrap : max_kadane;
}

```

**Input:** An array of integers and the size of the array.

**Output:** The maximum subarray sum of the circular array.

There can be two possibilities with circular arrays:

1) The elements that contribute to the maximum sum are arranged such that no wrapping is there. In this case, Kadane's algorithm will give us the answer. (In our function, the condition in the last line will go to _max\_kadane_)

2) The elements which contribute to the maximum sum are arranged such that wrapping is there (we have to go around from the end to the start). In this case, we change wrapping to non-wrapping. In other words, we modify the array so that the maximum subarray sum is not circular. We can do this because wrapping of contributing elements implies non wrapping of non contributing elements. If we invert the sign of every element in the array, then the non wrapping of non contributing elements will turn into our contributing elements, and they will not wrap around the array. If we run Kadane's algorithm in this new array, we will find the sum of all non contributing elements. We then substract this from the total sum in the original array. (We keep the sum of the original array in _max\_wrap_ and we do the substraction by adding the result of Kadane's algorithm (since we will be calling it on the sign-inverted array)). Finally we return the maximum between the original Kadane and _max\_wrap_.
