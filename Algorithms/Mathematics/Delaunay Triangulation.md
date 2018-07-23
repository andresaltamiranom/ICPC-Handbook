# Delaunay Triangulation

A Delaunay triangulation for a given set P of discrete points in a plane is a triangulation DT(P) such that no point in P is inside the circumcircle of any triangle in DT(P). Delaunay triangulations maximize the minimum angle of all the angles of the triangles in the triangulation; they tend to avoid sliver triangles (a triangle with one or two extremely acute angles).

![Delaunay Triangulation](https://upload.wikimedia.org/wikipedia/commons/d/db/Delaunay_circumcircles_vectorial.svg)

The problem of finding the Delaunay triangulation of a set of points in d-dimensional Euclidean space can be converted to the problem of finding the convex hull of a set of points in (d + 1)-dimensional space, by giving each point p an extra coordinate equal to |p|^2, taking the bottom side of the convex hull, and mapping back to d-dimensional space by deleting the last coordinate. As the convex hull is unique, so is the triangulation, assuming all facets of the convex hull are simplices. 

## Applications

1) The Euclidean minimum spanning tree of a set of points is a subset of the Delaunay triangulation of the same points, and this can be exploited to compute it efficiently.
2) For modelling terrain or other objects given a set of sample points, the Delaunay triangulation gives a nice set of triangles to use as polygons in the model. In particular, the Delaunay triangulation avoids narrow triangles (as they have large circumcircles compared to their area).
3) Delaunay triangulations can be used to determine the density or intensity of points samplings by means of the Delaunay tessellation field estimator (DTFE).
4) Delaunay triangulations are often used to build meshes for space-discretised solvers such as the finite element method and the finite volume method of physics simulation, because of the angle guarantee and because fast triangulation algorithms have been developed.
5) Constrained Delaunay triangulation (a generalization of the Delaunay triangulation that forces certain required segments into the triangulation) has found applications in path planning in automated driving.

Following is an implementation that can find the Delaunay triangulation of a given set of points.

```cpp

// Slow but simple Delaunay triangulation.
// Does not handle degenerate cases (from O’Rourke, Computational Geometry in C)
// 
// Running time: O(n^4)
//
// INPUT: x[] = x-coordinates
//        y[] = y-coordinates
//
// OUTPUT: triples = a vector containing m triples of indices corresponding to triangle vertices

#include<vector>
using namespace std;
typedef double T;

struct triple {
	int i, j, k;
	triple() {}
	triple(int i, int j, int k) : i(i), j(j), k(k) {}};
	
vector<triple> delaunayTriangulation(vector<T>& x, vector<T>& y) {
	int n = x.size();
	vector<T> z(n);
	vector<triple> ret;
	for (int i = 0; i < n; i++)
		z[i] = x[i] * x[i] + y[i] * y[i];
	for (int i = 0; i < n-2; i++) {
		for (int j = i+1; j < n; j++) {
			for (int k = i+1; k < n; k++) {
				if (j == k) continue;
				double xn = (y[j]-y[i])*(z[k]-z[i]) - (y[k]-y[i])*(z[j]-z[i]);
				double yn = (x[k]-x[i])*(z[j]-z[i]) - (x[j]-x[i])*(z[k]-z[i]);
				double zn = (x[j]-x[i])*(y[k]-y[i]) - (x[k]-x[i])*(y[j]-y[i]);
				bool flag = zn < 0;
				for (int m = 0; flag && m < n; m++)
					flag = flag && ((x[m]-x[i])*xn + (y[m]-y[i])*yn + (z[m]-z[i])*zn <= 0);
				if (flag) ret.push_back(triple(i, j, k));}}}
	return ret;}

int main() {
	T xs[]={0, 0, 1, 0.9};
	T ys[]={0, 1, 0, 0.9};
	vector<T> x(&xs[0], &xs[4]), y(&ys[0], &ys[4]);
	vector<triple> tri = delaunayTriangulation(x, y);
	// expected: 0 1 3
	//           0 3 2
	int i;
	for(i = 0; i < tri.size(); i++)
		printf("%d %d %d\n", tri[i].i, tri[i].j, tri[i].k);
	return 0;
}

```
