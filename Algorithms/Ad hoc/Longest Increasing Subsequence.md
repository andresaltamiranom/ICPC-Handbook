# Longest Increasing Subsequence

In this problem we are looking to find the longest subsequence in a given array of integers such that all elements of the subsequence are sorted in strictly ascending order.

For example, in the following array:

![LIS](https://i.imgur.com/jNfhUgo.png)

the longest increasing subsequence contains 4 elements:

![LIS](https://i.imgur.com/6KiMnsE.png)

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
