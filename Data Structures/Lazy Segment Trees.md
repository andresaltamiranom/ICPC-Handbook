# Lazy Segment Trees

Using _lazy propagation_, we can build a segment tree that supports both range updates and range queries in O(log(n)) time. The idea is to perform updates and queries from top to bottom and perform updates lazily so that they are propagated
down the tree **only when it is necessary**. In a lazy segment tree, nodes contain two types of information. Like in an ordinary segment tree, each node contains the sum or some other value related to the corresponding subarray. In addition, the node may contain information related to lazy updates, which has not been propagated to its children.

There are two types of range updates: each array value in the range is either increased by some value or assigned some value. Both operations can be implemented using similar ideas, and it is even possible to construct a tree that supports both operations at the same time.

**Example**

Let us consider an example where our goal is to construct a segment tree that supports two operations:

1) Increasing each value in [a, b] by a constant

2) Calculating the sum of values in [a, b]

We will construct a tree where each node has two values: _s_ and _z_.
_S_ denotes the sum of values in the range, and _z_ denotes the value of a lazy update, which means that all values in the range should be increased by _z_.

In the following tree, _z_ is 0 in all nodes, so there are no ongoing lazy updates.

![lazy segment tree](https://i.imgur.com/aYJsNte.png)

When the elements in [a, b] are increased by a value _u_, we walk from the root towards the leaves and modify the nodes of the tree as follows:

- If the range [x, y] of a node is completely inside [a, b], we increase the _z_ value of the node by _u_ and stop.

- If [x, y] only partially belongs to [a, b], we increase the _s_ value of the node by _h_\*_u_, where _h_ is the size of the intersection of [a, b] and [x, y], and continue our walk recursively in the tree.

For example, the following picture shows the tree after increasing the elements in [a, b] by 2:

![lazy segment tree](https://i.imgur.com/K7CHGz0.png)

We also calculate the sum of elements in a range [a, b] by walking in the tree from top to bottom. If the range [x, y] of a node completely belongs to [a, b], we add the _s_ value of the node to the sum. Otherwise, we continue the search recursively downwards in the tree.

Both in updates and queries, the value of a lazy update is always propagated to the children of the node before processing the node. The idea is that updates will be propagated downwards only when it is necessary, which guarantees that the operations are always efficient.

The following picture shows how the tree changes when we calculate the value of sum(a, b). The rectangle shows the nodes whose values change, because a lazy update is propagated downwards.

![lazy segment tree](https://i.imgur.com/d0N6YB7.png)

Note that sometimes it is needed to combine lazy updates. This happens when a node that already has a lazy update is assigned another lazy update. When calculating sums, it is easy to combine lazy updates, because the combination of updates _z1_ and _z2_ corresponds to an update _z1 + z2_.

Following is an implementation of a lazy segment tree.

```cpp

struct LazySegmentTree {
	vi t, d; int n, h;
	LazySegmentTree(vi &values) {
		n = values.size();
		h = sizeof(int) * 8 - __builtin_clz(n);
		t.assign(n<<1, 0), d.assign(n, 0);
		for(int i = 0; i < N; i++) t[i+N] = values[i];
		build(i+N, n<<1);
	}
	void calc(int p, int k) {
		if (d[p] == 0) t[p] = t[p<<1] + t[p<<1|1];
		else t[p] = d[p] * k;
	}
	void apply(int p, int value, int k) {
		t[p] = value * k;
		if (p < n) d[p] = value;
	}
	void push(int l, int r) {
		int s = h, k = 1 << (h-1);
		for (l += n, r += n-1; s > 0; --s, k >>= 1)
			for (int i = l >> s; i <= r >> s; ++i) if (d[i]) {
				apply(i<<1, d[i], k);
				apply(i<<1|1, d[i], k);
				d[i] = 0;
			}
	}
	void build(int l, int r) {
		int k = 2;
		for (l += n, r += n-1; l; k <<= 1) {
  			l >>= 1, r >>= 1;
  			for (int i = r; i >= l; --i) calc(i, k);
    	}
	}
	void modify(int l, int r, int value) {
		if (value == 0) return;
		push(l, l + 1); push(r - 1, r);
		int l0 = l, r0 = r, k = 1;
		for (l += n, r += n; l < r; l >>= 1, r >>= 1, k <<= 1) {
			if (l&1) apply(l++, value, k);
			if (r&1) apply(--r, value, k);
		}
		build(l0, l0 + 1);
		build(r0 - 1, r0);
	}
	int query(int l, int r) {
		push(l, l + 1); push(r - 1, r);
		int res = 0;
		for (l += n, r += n; l < r; l >>= 1, r >>= 1) {
			if (l&1) res += t[l++];
			if (r&1) res += t[--r];
		}
		return res;
	}
};
```
