# Bitonic Traveling Salesman Problem

## Problem Description

The Bitonic Traveling Salesman Problem (TSP) can be described as follows: Given a list of coordinates of _n_ vertices on 2D Euclidean space that are already sorted by x-coordinates (and if tie, by y-coordinates), find a tour that starts from the leftmost vertex, then goes strictly from left to right, and then upon reaching the rightmost vertex, the tour goes strictly from right to left back to the starting vertex. This tour behavior is called ‘bitonic’.

The resulting tour may not be the shortest possible tour under the standard definition of TSP. The following figure shows a comparison of these two TSP variants. The TSP tour: 0-3-5-6-4-1-2-0 is not a Bitonic TSP tour because, although the tour initially goes from left to right (0-3-5-6) and then goes back from right to left (6-4-1), it then makes another left to right (1-2) and then right to left (2-0) steps. The tour: 0-2-3-5-6-4-1-0 is a valid Bitonic TSP tour because we can decompose it into two paths: 0-2-3-5-6 that goes from left to right and 6-4-1-0 that goes back from right to left.

![Edmonds Karp](https://i.imgur.com/Tl78PnJ.png)

## Solution(s)

Although a Bitonic TSP tour of a set of _n_ vertices is usually longer than the standard TSP tour, this bitonic constraint allows us to compute a ‘good enough tour’ in O(n^2) time using Dynamic Programming, compared with the O(2^n × n^2) time for the standard TSP tour.

The main observation needed to derive the DP solution is the fact that we can (and have to) split the tour into two paths: Left-to-Right (LR) and Right-to-Left (RL) paths. Both paths include vertex 0 (the leftmost vertex) and vertex _n-1_ (the rightmost vertex). The LR path starts from vertex 0 and ends at vertex _n-1_. The RL path starts from vertex _n-1_ and ends at vertex 0.

Remember that all vertices have been sorted by x-coordinates (and if tie, by y-coordinates). We can then consider the vertices one by one. Both LR and RL paths start from vertex 0. Let _v_ be the next vertex to be considered. For each vertex _v_ in [1...n−2], we decide whether to add vertex _v_ as the next point of the LR path (to extend the LR path further to the right) or as the previous point the returning RL path (the RL path now starts at _v_ and goes back to vertex 0). For this, we need to keep track of two more parameters: _p1_ and _p2_. Let _p1_/_p2_ be the current ending/starting vertex of the LR/RL path, respectively.

The base case is when vertex _v_ = _n_ − 1 where we just need to connect the two LR and RL paths with vertex _n_ − 1.
With these observations in mind, we can write a simple DP solution like this:

```cpp

double dp1(int v, int p1, int p2) { // called with dp1(1, 0, 0)
	if (v == n-1)
		return d[p1][v] + d[v][p2];
	if (memo3d[v][p1][p2] > -0.5)
		return memo3d[v][p1][p2];
	return memo3d[v][p1][p2] = min(
		d[p1][v] + dp1(v+1, v, p2),  // extend LR path: p1->v, RL stays: p2 
		d[v][p2] + dp1(v+1, p1, v)); // LR stays: p1, extend RL path: p2<-v
}

```

However, the time complexity of _dp1_ with three parameters: (_v_, _p1_, _p2_) is O(n^3). This is not efficient, as parameter _v_ can be dropped and recovered from 1+_max(p1,p2)_. The improved DP solution is shown below and runs in O(n^2).

```cpp

double dp2(int p1, int p2) { // called with dp2(0, 0)
	int v = 1 + max(p1, p2); // this single line speeds up Bitonic TSP tour
	if (v == n-1)
		return d[p1][v] + d[v][p2];
	if (memo2d[p1][p2] > -0.5)
		return memo2d[p1][p2];
	return memo2d[p1][p2] = min(
		d[p1][v] + dp2(v, p2),  // extend LR path: p1->v, RL stays: p2
		d[v][p2] + dp2(p1, v)); // LR stays: p1, extend RL path: p2<-v
}

```
