# Segment Trees

A _segment tree_ is a data structure that allows two types of operations:

1) Process a query within a range.

2) Update a value in the list that makes up the tree.

Segment trees support queries that involve sums, minimums, maximums, and other associative operations. These operations run on O(log(n)) time.

To construct a segment tree, we assume that the size of the list that makes it up is a power of two and we use zero-based indexing, since it is more convenient to build a segment tree from a list with those characteristics. If the size of our list is not a power of two, we can add dummy values at the end to make its size a power of two.

**Example**

For the next array, its corresponding segment tree of sums is shown.

![alt text](https://i.imgur.com/jJgV7Cd.png)

In this tree, each node corresponds to the sum of its left and right children. The main advantage of placing our values in this data structure is that it allows queries over a range of these values to finish in a shorter time. If we ran a query of the sums of the values from the index 2 to index 7, we would have to check 6 places if we ran it over the array.

![alt text](https://i.imgur.com/ceLmjDc.png)

Instead, if we use a segment tree, we only have to check the nodes that cover this range.

![alt text](https://i.imgur.com/rryaoBb.png)

This reduces query times from O(n) to O(log(n)).

Following is an implementation of a segment tree using a vector of integers.

```cpp

struct SegmentTree {
	vi t; int N;
	SegmentTree(vi &values) {
		N = values.size();
		t.assign(N<<1, 0);
		for(int i = 0; i < N; i++) t[i+N] = values[i];
		for(int i = N-1; i; --i) t[i] = combine(t[i<<1], t[i<<1|1]);
	}
	int combine(int a, int b) { return a+b; }
	void set(int index, int value) {
		t[index+N] = value;
		for(int i = (index+N)>>1; i; i >>= 1) t[i] = combine(t[i<<1], t[i<<1|1]);
	}
	int query(int from, int to) {
		int ansL = 0, ansR = 0;
		for(int l = N+from, r = N+to; l<r; l >>= 1, r >>= 1) {
			if (l&1) ansL = combine(ansL, t[l++]);
			if (r&1) ansR = combine(ansR, t[--r]);
		}
		return combine(ansL, ansR);
	}
};
```

Following is an explanation for each of the functions.

### SegmentTree

```cpp
SegmentTree(vi &values) {
	N = values.size();
	t.assign(N<<1, 0);
	for(int i = 0; i < N; i++) t[i+N] = values[i];
	for(int i = N-1; i; --i) t[i] = combine(t[i<<1], t[i<<1|1]);
}
```
**Input:** List of integers.

**Output:** None.

Segment tree's constructor. Initializes the value of _N_ with the size of the list, then reserves double of this value (_N_<<1) for the vector _t_, and assigns a value of 0 to each space.

### combine

```cpp
int combine(int a, int b) { return a+b; }
```
**Input:** Two integers.

**Output:** The sum of the two integers received as parameters.

The idea is that this functions receives the value of two nodes whose value each represents the value of a range in the list that make up the tree.

### set

```cpp
void set(int index, int value) {
	t[index+N] = value;
	for(int i = (index+N)>>1; i; i >>= 1) t[i] = combine(t[i<<1], t[i<<1|1]);
}
```
**Input:** Two integers, one representing the index and the other the value to store in this index.

**Output:** None.

Sets the value at _index_ + _N_ to _value_. The right half of _t_ stores the real values in the list (the leaves), while the left half of _t_ stores the upper nodes, this is why we add _N_ to the index updated. After updating the actual value, all of its ancestors are updated to reflect the new value. (t[i<<1] and t[i<<1|1] represent the left and right children of a node, respectively.)

### query

```cpp
int query(int from, int to) {
	int ansL = 0, ansR = 0;
	for(int l = N+from, r = N+to; l<r; l >>= 1, r >>= 1) {
		if (l&1) ansL = combine(ansL, t[l++]);
		if (r&1) ansR = combine(ansR, t[--r]);
	}
	return combine(ansL, ansR);
}
```
**Input:** Two integers representing a range.

**Output:** An integer value holding the result of the query.

Looks for the left-side and right-side nodes which cover the range queried and returns its sum. The _for_ loop starts from the value _N_ + _from_ and _N_ + _to_ because it starts looking from the leaves of the tree and then goes up (by doing _l_ >>= 1 and _r_ >>= 1 each iteration). If _l_ or _r_ is odd, this means we have found the node that represents its corresponding side. After the loop ends, _ansL_ and _ansR_ will store the value representing their own side, so now we just return the call to _combine()_ with these two values.
