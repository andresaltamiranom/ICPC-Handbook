# Polygon

### Polygon typedef

```cpp

typedef vector<Point> Polygon;

```

The standard way to represent a Polygon is to simply enumerate the vertices of the polygon in either clockwise or counterclockwise order, with the first vertex being equal to the last vertex.

## Helper functions

### cross

```cpp

ll cross(const Point &O, const Point &A, const Point &B) {
	return (A.x - O.x) * (B.y - O.y) - (A.y - O.y) * (B.x - O.x);
}

```

**Input:** Three Points.

**Output:** The cross product of the three points.

To get the cross product of three points, we have to calculate (q - p) x (r - p) where a x b is the determinant: a x b = (a.x * b.y - a.y * b.x).

## Polygon functions

### Convex hull

```cpp

Polygon convexHull(Polygon &P) {
	int n = P.size(), k = 0;
	Polygon H(2*n);
	sort(P.begin(), P.end());
	FOR(i, 0, n) {
		while (k >= 2 && cross(H[k-2], H[k-1], P[i]) <= 0) k--;
		H[k++] = P[i];
	}
	for (int i = n-2, t = k+1; i >= 0; i--) {
		while (k >= t && cross(H[k-2], H[k-1], P[i]) <= 0) k--;
		H[k++] = P[i];
	}
	H.resize(k);
	return H;
}

```

**Input:** A Polygon.

**Output:** The convex polygon of _P_.

Gets the convex polygon of a given polygon. The convex polygon is the smallest polygon which contains all of the vertices of the original polygon inside, as shown in this image:

https://gurus.pyimagesearch.com/wp-content/uploads/2015/03/contours_convex_hull.jpg

To get the convex polygon, we use //TODO algorithm. First, we create a new Polygon with twice the size of the original, just to make sure the convex Polygon will fit in _H_. Then we sort the points by their x-coordinates. We then loop through each set of points and, by getting its cross product, we get the direction of the turn of the current point and the previous two points. 
