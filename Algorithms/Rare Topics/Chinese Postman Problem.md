# Chinese Postman Problem

## Problem Description

The Chinese Postman/Route Inspection Problem is the problem of finding the (length of the) shortest tour/circuit that visits every edge of a (connected) undirected weighted graph. If the graph is Eulerian, then the sum of edge weights along the Euler tour that covers all the edges in the Eulerian graph is the optimal solution for this problem. This is the easy case. But when the graph is non Eulerian, e.g. see the left graph in the following figure, then this Chinese Postman Problem is harder.

![CPP](https://i.imgur.com/2l3KDRw.png)

## Solution(s)

The important insight to solve this problem is to realize that a non Eulerian graph G must have an **even number** of vertices of odd degree (the Handshaking lemma found by Euler himself). Let’s name the subset of vertices of G that have odd degree as T. Now, create a complete graph Kn where _n_ is the size of T. T form the vertices of Kn. An edge (_i_, _j_) in Kn has weight which is the shortest path weight of a path from _i_ to _j_, e.g. in the middle graph of the figure, edge 2-5 in K4 has weight 2 + 1 = 3 from path 2-4-5 and edge 3-4 in K4 has weight 3 + 1 = 4 from path 3-5-4.

Now, if we **double** the edges selected by the _minimum weight perfect matching_ on this complete graph Kn, we will convert the non Eulerian graph G to another graph G′ which is Eulerian. This is because by doubling those edges, we actually add an edge between a pair of vertices with odd degree (thus making them have even degree afterwards). The minimum weight perfect matching ensures that this transformation is done in the least cost way. The solution for the minimum weight perfect matching on the K4 shown in the middle graph of the figure is to take edge 2-4 (with weight 2) and edge 3-5 (with weight 3).

After doubling edge 2-4 and edge 3-5, we are now back to the easy case of the Chinese Postman Problem. In the right graph of the figure, we have an Eulerian graph. The tour is simple in this Eulerian graph. One such tour is: 1->2->4->5->3->6->5->3->2->4->1 with a total weight of 34 (the sum of all edge weight in the modified Eulerian graph G′, which is the sum of all edge weight in G plus the cost of the minimum weight perfect matching in Kn).

The hardest part of solving the Chinese Postman Problem is therefore in finding the minimum weight perfect matching on Kn, which is **not** a bipartite graph (a complete graph). If _n_ is small, this can be solved with DP with bitmask technique.
