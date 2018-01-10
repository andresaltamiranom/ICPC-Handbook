# Edit Distance

Given two strings _a_ and _b_ on an alphabet Σ (e.g. the set of ASCII characters, the set of bytes [0..255], etc.), the edit distance is the number of edit operations that transforms _a_ into _b_.

There are three operations defined for this problem:

Insertion of a single symbol.
Deletion of a single symbol.
Substitution of a single symbol.

For example, the distance between "kitten" and "sitting" is 3.

1) **k**itten → **s**itten (substitution of "s" for "k")
2) sitt**e**n → sitt**i**n (substitution of "i" for "e")
3) sittin → sittin**g** (insertion of "g" at the end).

### editDistance

```cpp

int editDistance(string A, string B) {
	int n = A.length(), m = B.length();
	int dist[n+1][m+1];
	dist[0][0] = 0;
	FOR(i, 1, n+1) dist[i][0] = i;
	FOR(j, 1, m+1) dist[0][j] = j;
	FOR(i, 1, n+1)
		FOR(j, 1, m+1)
			dist[i][j] = min(dist[i-1][j-1] + (A[i-1] != B[j-1]), min(dist[i-1][j] + 1, dist[i][j-1] + 1));
	return dist[n][m];
}

```

**Input:** Two strings _A_ and _B_.

**Output:** An integer, the edit distance from _A_ to _B_.

We solve this problem using dynamic programming. We initialize a matrix with the amount of rows equal to the length of the first string, and the amount of columns equal to the length of the second string. We initialize the matrix by numbering the first row from 0 to _n_, and the first column from 0 to _m_. After this, we will loop through and fill the matrix. At each cell, there are two possibilities:
- The current letter in _A_ is the same as the current letter in _B_. In this case, _dist[i][j]_ = _dist[i-1][j-1]_, since there was no cost at this step.
- The current letter in _A_ is different as the current letter in _B_. In this case, we can either, add, delete, or subsitute a letter. We will choose the _minimum_ among these options. The cost of substituting is _dist[i-1][j-1] + 1_. The cost of adding a letter is ////////////// _dist[i-1][j] + 1_, and the cost of removing a letter is _dist[i][j-1] + 1_.
