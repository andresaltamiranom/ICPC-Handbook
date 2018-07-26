# Selection Problem

## Problem Description

Selection problem is the problem of finding the k-th smallest element of an array of n elements. Another name for selection problem is order statistics. Thus the minimum (smallest) element is the 1-st order statistic, the maximum (largest) element is the n-th order statistic, and the median element is the n / 2 order statistic (there are 2 medians if n is even).

## Solution(s)

### Special Cases: k = 1 and k = n

Searching the minimum (k = 1) or maximum (k = n) element of an arbitrary array can be done in Ω(n − 1) comparisons: We set the first element to be the temporary answer, and then we compare this temporary answer with the other n − 1 elements one by one and keep the smaller (or larger, depending on the requirement) one. Finally, we report the answer. Ω(n − 1) comparisons is the lower bound, i.e. We cannot do better than this. While this problem is easy for k = 1 or k = n, finding the other order statistics—the general form of selection problem—is more difficult.

### O(n^2) algorithm, static data

A naïve algorithm to find the k-th smallest element is to this: Find the smallest element, ‘discard’ it (e.g. by setting it to a ‘dummy large value’), and repeat this process k times. When k is near 1 (or when k is near n), this O(k * n) algorithm can still be treated as running in O(n), i.e. we treat k as a ‘small constant’. However, the worst case scenario is when we have to find the median (k = n / 2 ) element where this algorithm runs in O(n / 2 × n) = O(n^2).

### O(n * log(n)) algorithm, static data

A better algorithm is to sort (that is, pre-process) the array first in O(n * log(n)). Once the array is sorted, we can find the k-th smallest element in O(1) by simply returning the content of index k - 1 (0-based indexing) of the sorted array. The main part of this algorithm is the sorting phase. Assuming that we use a good O(n * log(n)) sorting algorithm, this algorithm
runs in O(n * log(n)) overall.

### Expected O(n) algorithm, static data

An even better algorithm for the selection problem is to apply Divide and Conquer paradigm. The key idea of this algorithm is to use the O(n) Partition algorithm (the randomized version) from Quick Sort as its sub-routine.

A randomized partition algorithm: **RandomizedPartition(A, l, r)** is an algorithm to partition a given range [l..r] of the array A around a (random) pivot. Pivot A[p] is one of the elements of A where p is in [l..r]. After partition, all elements ≤ A[p] are placed before the pivot and all elements > A[p] are placed after the pivot. The final index of the pivot q is returned. This randomized partition algorithm can be done in O(n).

After performing q = **RandomizedPartition(A, 0, n - 1)**, all elements ≤ A[q] will be placed before the pivot and therefore A[q] is now in it’s correct order statistic, which is q + 1. Then, there are only 3 possibilities:

1. q + 1 = k, A[q] is the desired answer. We return this value and stop.

2. q + 1 > k, the desired answer is inside the left partition, e.g. in A[0..q-1].

3. q + 1 < k, the desired answer is inside the right partition, e.g. in A[q+1..n-1].

This process can be repeated recursively on smaller range of search space until we find the required answer. A snippet of C++ code that implements this algorithm is shown below.

```cpp

int RandomizedSelect(int A[], int l, int r, int k) {
	if (l == r) return A[l];
	int q = RandomizedPartition(A, l, r);
	if (q + 1 == k) return A[q];
	else if (q + 1 > k) return RandomizedSelect(A, l, q - 1, k);
	else return RandomizedSelect(A, q + 1, r, k);
}

```

This **RandomizedSelect** algorithm runs in expected O(n) time and is very unlikely to run in its worst case O(n^2) as it uses randomized pivot at each step. The full analysis involves probability and expected values.

A simplified (but not rigorous) analysis is to assume **RandomizedSelect** divides the array into two at each step and n is a power of two. Therefore it runs **RandomizedPartition** in O(n) for the first round, in O(n/2) in the second round, in O(n/4) in the third round and finally O(1) in the 1 + log2(n) round. The cost of **RandomizedSelect** is mainly determined by the
cost of **RandomizedPartition** as all other steps of **RandomizedSelect** are O(1). Therefore the overall cost is O(n + n/2 + n/4 + ... + n/n) = O(n * (1/1 + 1/2 + 1/4 + ... + 1/n) ≤ O(2 * n) = O(n).

### Library solution for the expected O(n) algorithm, static data

C++ STL has function **nth_element** in the _algorithm_ library. This **nth_element** implements the expected O(n) algorithm as shown above. However as of 24 May 2013, we are not aware of a Java equivalent for this function.

### O(n * log(n)) pre-processing, O(log(n)) algorithm, dynamic data

All solutions presented earlier assume that the given array is static — unchanged for each query of the k-th smallest element. However, if the content of the array is frequently modified, i.e. a new element is added, an existing element is removed, or the value of an existing element is changed, the solutions outlined above become inefficient.

When the underlying data is dynamic, we need to use a balanced Binary Search Tree. First, we insert all n elements into a balanced BST in O(n * log(n)) time. We also augment (add information) about the size of each sub-tree rooted at each vertex. This way, we can find the k-th smallest element in O(log(n)) time by comparing k with q — the size of the left sub-tree of the root:

1. If q + 1 = k, then the root is the desired answer. We return this value and stop.

2. If q + 1 > k, the desired answer is inside the left sub-tree of the root.

3. If q + 1 < k, the desired answer is inside the right sub-tree of the root and we are now searching for the (k−q−1)-th smallest element in this right sub-tree. This adjustment of k is needed to ensure correctness.

This process — which is similar with the expected O(n) algorithm for static selection problem — can be repeated recursively until we find the required answer. As checking the size of a sub-tree can be done in O(1) if we have properly augment the BST, this overall algorithm runs at worst in O(log(n)) time, from root to the deepest leaf of a balanced BST.

However, as we need to augment a balanced BST, this algorithm cannot use built-in C++ STL <map>/<set> (or Java TreeMap/TreeSet) as these library code cannot be augmented. Therefore, we need to write our own balanced BST routine (e.g. AVL tree — see figure below — or Red Black Tree, etc — all of them take some time to code) and therefore such selection
problem on dynamic data can be quite painful to solve.

![Selection Problem](https://i.imgur.com/VR0pUgl.png)
