# Longest Increasing Subsequence

In this problem we are looking to find the longest subsequence in a given array of integers such that all elements of the subsequence are sorted in strictly ascending order.

For example, in the following array:

![LIS](https://i.imgur.com/jNfhUgo.png)

the longest increasing subsequence contains 4 elements:

![LIS](https://i.imgur.com/6KiMnsE.png)

A naïve Complete Search that enumerates all possible subsequences to find the longest increasing one is too slow as there are O(2^n) possible subsequences. There's also a DP implementation that can solve this problem in O(n^2) instead. However, there is a solution that runs in O(n * log(n)) by maintaining an array that is **always sorted** and therefore amenable to binary search.

Let array L be an array such that L(i) represents the smallest ending value of all length-i LISs found so far. Though this definition is slightly complicated, it is easy to see that it is always ordered —- L(i-1) will always be smaller than L(i) as the second-to-last element of any LIS (of length-i) is smaller than its last element. As such, we can binary search array L
to determine the longest possible subsequence we can create by appending the current element A[i] -— simply find the index of the last element in L that is less than A[i].

Following is an implementation for LIS that runs in O(n * log(n)) time:

```cpp

vi longestIncreasingSubsequence(vi v) {
	vii best;
	vi parent(v.size(), -1);
	FOR(i, 0, v.size()) {
		ii item = ii(v[i], i);
		vii::iterator it = upper_bound(best.begin(), best.end(), item);
		if (it == best.end()) {
			parent[i] = (best.size() == 0 ? -1 : best.back().second);
			best.pb(item);
		} else {
			parent[i] = parent[it->second];
			*it = item;
		}
	}
	vi lis;
	for(int i=best.back().second; i >= 0; i=parent[i])
		lis.pb(v[i]);
	reverse(lis.begin(), lis.end());
	return lis;
}

```
