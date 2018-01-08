# Polygon

### Polygon typedef

```cpp

typedef vector<Point> Polygon;

```

The standard way to represent a Polygon is to simply enumerate the vertices of the polygon in either clockwise or counterclockwise order, with the first vertex being equal to the last vertex.

## Helper functions

### cross product

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

![alt text](https://gurus.pyimagesearch.com/wp-content/uploads/2015/03/contours_convex_hull.jpg)

To get the convex polygon, we use **Andrew's monotone chain algorithm**. First, we create a new Polygon with twice the size of the original, just to make sure the convex Polygon will fit in _H_. Then we sort the points lexicographically (first by x-coordinate, and in case of a tie, by y-coordinate). We'll then construct the upper and lower hulls of the points in O(n) time. An upper hull is the part of the convex hull, which is visible from the above. It runs from its rightmost point to the leftmost point in counterclockwise order. The lower hull is the remaining part of the convex hull. To do this, we loop twice through each set of points and, by getting its cross product, we get the direction of the turn of the current point and the previous two points. The first _for_ loop builds the lower hull, from left to right, by moving only in counterclockwise order (_cross() <= 0_). Then, for the upper hull, we go from right to left (hence starting at _n-2_), also moving in counterclockwise order.

This algorithm runs in O(n\*log(n)) time complexity.

### Polygon Area

```cpp

double area(const Polygon &P) {
	double result = 0.0, x1, y1, x2, y2;
	for (int i = 0; i < (int)P.size()-1; i++) {
		x1 = P[i].x; x2 = P[i+1].x;
		y1 = P[i].y; y2 = P[i+1].y;
		result += (x1*y2-x2*y1);
	}
	return abs(result) / 2.0;
}

```

**Input:** A Polygon.

**Output:** A double value, the area of the polygon _P_.

Calculates the area of a polygon of _n_ vertices given in either clockwise or counterclockwise order. This can be calculated by computing the determinant of the matrix of form [[X0, Y0], [X1, Y1], [X2, Y2], ... [Xn-2, Yn-1], [Xn-1, Yn-1]].

### isConvex

```cpp

bool isConvex(const Polygon &P) {
	int sz = (int)P.size();
	if (sz <= 3) return false;
	bool isLeft = ccw(P[0], P[1], P[2]);
	for (int i = 1; i < sz-1; i++)
		if (ccw(P[i], P[i+1], P[(i+2) == sz ? 1 : i+2]) != isLeft)
			return false;
	return true;
}

```

**Input:** A Polygon.

**Output:** A boolean value, whether the polygon _P_ is convex or not.

//TODO

### inPolygon
