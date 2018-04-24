# Closest Pair Problem

## Problem Description

Given a set S of _n_ points on a 2D plane, find two points with the closest Euclidean distance.

## Solution(s)

### Complete Search

A naïve solution computes the distances between all pairs of points and reports the minimum one. However, this requires O(n^2) time.

### Divide and Conquer

We can use the following Divide and Conquer strategy to achieve O(nlog(n)) time. We perform the following three steps:

1) **Divide**: We sort the points in set S by their x-coordinates (if tie, by their y-coordinates). Then, we divide set S into two sets of points S1 and S2 with a vertical line _x = d_ such that |S1| = |S2| or |S1| = |S2| + 1, i.e. the number of points in each set is balanced.
2) **Conquer**: If we only have one point in S, we return ∞. If we only have two points in S, we return their Euclidean distance.
3) **Combine**: Let _d1_ and _d2_ be the smallest distance in S1 and S2, respectively. Let _d3_ be the smallest distance between all pairs of points (_p1_, _p2_) where _p1_ is a point in S1 and _p2_ is a point in S2. Then, the smallest distance is _min(d1,d2,d3)_, i.e. the answer may be in the smaller set of points S1 or in S2 or one point in S1 and the other point in S2, crossing through line _x = d_.

The combine step, if done naïvely, will still run in O(n^2), but this can be optimized. Let _d′ = min(d1,d2)_. For each point in the left of the dividing line _x = d_, a closer point in the right of the dividing line can only lie within a rectangle with width _d′_ and height _2×d′_. It can be proven that there can be only at most 6 such points in this rectangle. This means that the combine step only requires O(6n) operations and the overall time complexity of this divide and conquer solution is T(n) = 2 × T(_n_/2) + O(n) which is O(nlog(n)).
