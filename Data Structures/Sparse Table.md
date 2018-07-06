# Sparse Table

The Segment Tree data structure can be used to solve the Range Minimum Query (RMQ) problem: the problem of finding the index that has the minimum element within a range [i..j] of the underlying array A. It takes O(n) pre-processing time to build the Segment Tree, and once the Segment Tree is ready, each RMQ is just O(log(n)). With Segment Tree, we can deal with the _dynamic version_ of this RMQ problem, i.e. when the underlying array is updated, we usually only need O(log(n)) to update the corresponding Segment Tree structure.

However, some problems involving RMQ never change the underlying array A after the first query. This is called the _static_ RMQ problem. Although Segment Tree obviously can be used to deal with the static RMQ problem, this static version has an alternative DP solution with O(n * log(n)) pre-processing time and O(1) per RMQ. One such example is the Lowest Common Ancestor (LCA) problem.

The key idea of the DP solution is to split A into sub arrays of length 2^j for each non-negative integer j such that 2^j ≤ n. We will keep an array SpT of size n * log(n) where SpT[i][j] stores the index of the minimum value in the sub array starting at index i and having length 2^j. This array SpT will be sparse as not all of its cells have values (hence the name 'Sparse Table'). We use an abbreviation SpT to differentiate this data structure from Segment Tree (ST).

To build up the SpT array, we use a technique similar to the one used in many Divide and Conquer algorithms such as merge sort. We know that in an array of length 1, the single element is the smallest one. This is our base case. To find out the index of the smallest element in an array of size 2^j, we can compare the values at the indices of the smallest elements in the two distinct sub arrays of size 2^(j-1) and take the index of the smallest element of the two. It takes O(n * log(n)) time to build up the SpT array like this.

It is simple to understand how we would process a query if the length of the range were a power of 2. Since this is exactly the information SpT stores, we would just return the corresponding entry in the array. However, in order to compute the result of a query with arbitrary start and end indices, we have to fetch the entry for two smaller sub arrays within this range and take the minimum of the two. Note that these two sub arrays might have to overlap, the point is that we want to cover the entire range with two sub arrays and nothing outside of it. This is always possible even if the length of the sub arrays have to be a power of 2. First, we find the length of the query range, which is j-i+1. Then, we apply log2 on it and round down the result, i.e. k = floor(log2(j-i+1)). This way, 2^k ≤ (j-i+1). The following figure shows what the two sub arrays might look like. As there is a potentially overlapping sub-problems, this part of the solution is classified as Dynamic Programming.

![RMQ](https://i.imgur.com/Uspu74O.jpg)

An example implementation of Sparse Table to solve the static RMQ problem is shown below.

```cpp

/* Sparse Table
O(N*log(N)) construction
O(1) queries
Answers RMQ
*/
struct SparseTable {
	vi A; vvi SpT;
	int log2(int n) { int i=0; while(n >>= 1) i++; return i; }
	SparseTable(vi arr) { //O(NlogN)
		int N = arr.size();
		A.assign(N, 0);
		SpT.assign(N, vi(log2(N)+1));
		int i, j;
		for(i=0; i<N; i++)
			SpT[i][0] = i, A[i] = arr[i];

		for(j=1; 1<<j <= N; j++)
			for(i=0; i + (1<<j) - 1 < N; i++)
				if(A[SpT[i][j - 1]] < A[SpT[i + (1 << (j - 1))][j - 1]])
					SpT[i][j] = SpT[i][j - 1];
				else
					SpT[i][j] = SpT[i + (1 << (j - 1))][j - 1];
	}
	// returns the index of the minimum value
	int query(int i, int j) {
		if(i > j) swap(i, j);
		int k = log2(j-i+1);
		if(A[SpT[i][k]] < A[SpT[j-(1 << k)+1][k]])
			return SpT[i][k];
		return SpT[j-(1 << k)+1][k];
	}
};
```
