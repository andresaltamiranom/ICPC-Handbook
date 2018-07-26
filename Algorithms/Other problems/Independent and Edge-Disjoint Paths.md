# Independent and Edge-Disjoint Paths

## Problem Description

Two paths that start from a source vertex _s_ to a sink vertex _t_ are said to be independent (vertex-disjoint) if they do not share any vertex apart from _s_ and _t_.

Two paths that start from a source _s_ to sink _t_ are said to be edge-disjoint if they do not share any edge (but they can share vertices other than _s_ and _t_).

Given a graph G, find the maximum number of independent and edge-disjoint paths from source _s_ to sink _t_.

## Solution(s)

The problem of finding the (maximum number of) independent paths from source _s_ to sink _t_ can be reduced to the Network (Max) Flow problem. We construct a flow network N = (V, E) from G with vertex capacities, where N is the carbon copy of G except that the capacity of each v belonging to V is 1 (i.e. each vertex can only be used once) and the capacity of each e belonging to E is also 1 (i.e. each edge can only be used once too). Then run the Edmonds Karp’s algorithm as per normal.

![alt text](https://i.imgur.com/SeWPV8f.png)

Finding the (maximum number of) edge-disjoint paths from _s_ to _t_ is similar to finding (maximum) independent paths. The only difference is that this time we do not have any vertex capacity which implies that two edge-disjoint paths can still share the same vertex. See the previous figure for a comparison between maximum independent paths and edge-disjoint paths from _s = 0_ to _t = 6_.
