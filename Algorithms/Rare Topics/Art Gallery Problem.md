# Art Gallery Problem

## Problem Description

The ‘Art Gallery’ Problem is a family of related visibility problems in computational geometry. In this section, we discuss several variants. The common terms used in the variants discussed below are the simple (not necessarily convex) polygon P to describe the art gallery; a set of points S to describe the guards where each guard is represented by a point in P; a rule that a point A ∈ S can guard another point B ∈ P if and only if A ∈ S,B ∈ P, and line segment AB is contained in P; and a question on whether all points in polygon P are guarded by S. Many variants of this Art Gallery Problem are classified as NP-hard problems.

1. Variant 1: Determine the upper bound of the smallest size of set S.
2. Variant 2: Determine if ∃ a critical point C in polygon P and ∃ another point D ∈ P
such that if the guard is at position C, the guard cannot protect point D.
3. Variant 3: Determine if polygon P can be guarded with just one guard.
4. Variant 4: Determine the smallest size of set S if the guards can only be placed at the vertices of polygon P and only the vertices need to be guarded.

Note that there are many more variants other than the ones presented.

## Solution(s)

1. The solution for variant 1 is a theoretical work of the Art Gallery theorem by Václav Chvátal. He states that ⌊n/3⌋ guards are always sufficient and sometimes necessary to guard a simple polygon with _n_ vertices.

2. The solution for variant 2 involves testing if polygon P is concave (and thus has a critical point). We can use the negation of the _isConvex()_ function.

3. The solution for variant 3 can be hard if one has not seen the solution before. We can use the _cutPolygon()_ function. We cut polygon P with all lines formed by the edges in P in counter clockwise fashion and retain the left side at all times. If we still have a non-empty polygon at the end, one guard can be placed in that non empty polygon which can protect the entire polygon P.

4. The solution for variant 4 involves the computation of Minimum Vertex Cover of the ‘visibility graph’ of polygon P. In general this is another NP-hard problem.
