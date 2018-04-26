# Max Weighted Independent Set

## Problem Description

Given a _vertex-weighted_ graph G, find the Max **Weighted** Independent Set (MWIS) of G. An Independent Set (IS) is a set of vertices in a graph, no two of which are adjacent. Our task is to select an IS of G with the maximum total (vertex) weight. This is a hard problem on a general graph. However, if the given graph G is a tree or a bipartite graph, we have efficient solutions.

## Solution(s)

### On Tree

If graph G is a tree, we can find the MWIS of G using DP. Let _C(v, taken)_ be the MWIS of the subtree rooted at _v_ if it is _taken_ as part of the MWIS. We have the following complete search recurrences:

1) If _v_ is a leaf vertex
	- _C(v, true) = w(v)_
	  
	  If leaf _v_ is taken, then the weight of this subtree is the weight of this _v_.
	- _C(v, false) = 0_
	  
	  If leaf _v_ is not taken, then the weight of this subtree is 0.
2) If _v_ is an internal vertex
	- _C(v, true) = w(v)_ + ∑ch ∈ children(_v_) _C(ch, false)_
	  
	  If root _v_ is taken, we add weight of _v_ but all children of _v_ **cannot** be taken.
	- _C(v, false) =_ ∑ch ∈ children(_v_) _max(C(ch, true), C(ch, false))_
	  
	  If root _v_ is not taken, children of _v_ may or may not be taken. We return the larger one.

The answer is _max(C(root, 1), C(root, 0))_ — take or not take the root. This DP solution just requires O(V) space and O(V) time.

### On Bipartite Graph

If the graph G is a bipartite graph, we have to reduce MWIS problem into a Max Flow problem. We assign the original vertex cost (the weight of taking that vertex) as capacity from source to that vertex for the left set of the bipartite graph and capacity from that vertex to sink for right set of the bipartite graph. Then, we give ‘infinite’ capacity in between any edge in between the left and right sets. The MWIS of this bipartite graph is the weight of all vertex cost minus the max flow value of this flow graph.
