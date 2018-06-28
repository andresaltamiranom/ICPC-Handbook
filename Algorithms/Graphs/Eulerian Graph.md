# Eulerian Graph

An _Euler path_ is defined as a path in a graph which visits **each edge** of the graph **exactly once**. Similarly, an _Euler tour/cycle_ is an Euler path which starts and ends on the same vertex. A graph which has either an Euler path or an Euler tour is called an Eulerian graph.

Following is an example of each (Euler path on the left and Euler tour on the right):

![Euler path](https://i.imgur.com/sOpE2RO.png)
![Euler tour](https://i.imgur.com/LKte1Po.png)


### Eulerian Graph Check

To check whether a connected undirected graph has an Euler tour is simple. We just need to check if all vertices have even degrees. It is similar for the Euler path, i.e. an undirected graph has an Euler path if all except two vertices have even degrees. This Euler path will start from one of these odd degree vertices and end in the other. Such degree check can be done in O(V+E), usually done simultaneously when reading the input graph.

### Printing Euler Tour

While checking whether a graph is Eulerian is easy, finding the actual Euler tour/path requires more work. The code below produces the desired Euler tour when given an unweighted Eulerian graph stored in an Adjacency List where the second attribute in edge information pair is a Boolean 1 (this edge can still be used) or 0 (this edge can no longer be used).

```cpp

list<int> cyc; // we need list for fast insertion in the middle

void EulerTour(list<int>::iterator i, int u) {
	for (int j = 0; j < (int)AdjList[u].size(); j++ {
		ii v = AdjList[i][j];
		if (v.second) { // if the edge can still be used/not removed
			v.second = 0; // make the weight of this edge to be 0 (removed)
			for (int k = 0; k < (int)AdjList[v.first].size(); k++) {
				ii uu = AdjList[v.first][k]; // remove bi-directional edge
				if (uu.first == u && uu.second) {
					uu.second = 0;
					break;
				}
			}
			EulerTour(cyc.insert(i, u), v.first);
		}
	}
}

// inside int main()
	cyc.clear();
	EulerTour(cyc.begin(), A); // cyc contains an Euler tour starting at A
	for (list<int>::iterator it = cyc.begin(); it != cyc.end(); it++)
		printf("%d\n", *it); // the Euler tour

```
