# Line

We represent a line using the formula _ax+by+c=0_.

## Struct Line

```cpp

struct Line {
	double a, b, c;
	Line() : a(0), b(0), c(0) {}
	Line(Point p1, Point p2) {
		if(abs(p1.x-p2.x) < EPS) {
			a = 1.0; b = 0.0; c = -p1.x;
		} else {
			a = -(double)(p1.y-p2.y)/(p1.x-p2.x);
			b = 1.0;
			c = -(double)(a*p1.x)-p1.y;
		}
	}
};

``` 

## areParallel

```cpp

bool areParallel(Line l1, Line l2) {
	return (abs(l1.a-l2.a) < EPS) && (abs(l1.b-l2.b) < EPS);
}

```

**Input:** Two Lines.

**Output:** A boolean, whether the two lines are parallel or not.

Checks if two given lines are parallel. Two lines are parallel if they have the same x and y intercepts.

### areSame

```cpp

bool areSame(Line l1, Line l2) {
	return areParallel(l1, l2) && (abs(l1.c-l2.c) < EPS);
}

```

**Input:** Two Lines.

**Output:** A boolean, whether the two lines are the same line or not.

Checks if two lines are the same line. The two lines are the same if their three values are equal. This function leverages from the _areParallel()_ function.

### areIntersect

```cpp

bool areIntersect(Line l1, Line l2, Point &p) {
	if (areParallel(l1, l2)) return false;
	p.x = (l2.b * l1.c - l1.b * l2.c) / (l2.a * l1.b - l1.a * l2.b);
	if (abs(l1.b) > EPS) p.y = -(l1.a * p.x + l1.c);
	else                 p.y = -(l2.a * p.x + l2.c);
	return true;
}

```

**Input:** Two Lines and a Point passed by reference.

**Output:** A boolean, whether the two lines intersect or not. If they intersect, the point _p_ will contain the point of the intersection.

Checks if two lines intersect each other. There are three possibilities of intersections with lines: if they are the same line, they have an infinitude of points in common; if they are different lines but have the same slope, they are parallel and have no point of intersection; otherwise they have a single point of intersection. To find the point of intersection, //TODO

### lineSegIntersect

```cpp

// Interseccion de AB con CD
// * WARNING: Does not work for collinear line segments!
bool lineSegIntersect(Point a, Point b, Point c, Point d) {
	double ucrossv1 = cross(toVec(a, b), toVec(a, c));
	double ucrossv2 = cross(toVec(a, b), toVec(a, d));
	if (ucrossv1 * ucrossv2 > 0) return false;
	double vcrossu1 = cross(toVec(c, d), toVec(c, a));
	double vcrossu2 = cross(toVec(c, d), toVec(c, b));
	return (vcrossu1 * vcrossu2 <= 0);
}

```

**Input:** Four points representing two line segments.

**Output:** A boolean, whether the two line segments intersect or not.

//TODO

### distToLine

```cpp

// Calcula la distancia de un punto P a una recta AB, y guarda en C la interseccion
double distToLine(Point p, Point a, Point b, Point &c) {
	Vec ap = toVec(a, p), ab = toVec(a, b);
	double u = dot(ap, ab) / norm_sq(ab);
	c = translate(a, scale(ab, u));
	return dist(p, c);
}

```

**Input:** Four points. The second and third points represent a line. The fourth point is passed by reference.

**Output:** A double value, the distance from point _p_ to the line _AB_.

//TODO

### distToLineSegment

```cpp

// Distancia a de P a segmento AB
double distToLineSegment(Point p, Point a, Point b, Point &c) {
	Vec ap = toVec(a, p), ab = toVec(a, b);
	double u = dot(ap, ab) / norm_sq(ab);
	if (u < 0.0) { c = a; return dist(p, a); }
	if (u > 1.0) { c = b; return dist(p, b); }
	return distToLine(p, a, b, c);
}

```

**Input:** Four points. The second and third points represent a line segment. The fourth point is passed by reference.

**Output:** A double value, the distance from point _p_ to the line segment _AB_.

//TODO
