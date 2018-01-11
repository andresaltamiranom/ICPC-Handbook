# Longest Increasing Subsequence

In this problem we are looking to find the length of the longest subsequence in a given array of integers such that all elements of the subsequence are sorted in strictly ascending order.

For example, the length of the LIS for [15, 27, 14, 38, 26, 55, 46, 65, 85]  is 6 since the longest increasing subsequence is [15, 27, 38, 55, 65, 85].

### longestIncreasingSubsequence

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

**Input:** A list of numbers (in this case, we pass it as a _vector_ of integers.)

**Output:** A list of numbers (in this case, we return a _vector_ of integers, the _LIS_.)

// TODO Explanation
