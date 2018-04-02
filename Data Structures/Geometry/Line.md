# Line

A line is represented by the formula _ax+by+c=0_.

It's important to distinguish the difference between a line and a line segment. A line goes on infinitely, while a line segment has a start and an end point.

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

**Output:** A boolean, whether the two lines intersect or not. If they intersect, _p_ will contain the point of the intersection.

Checks if two lines intersect each other. There are three possibilities of intersections with lines: if they are the same line, they have an infinitude of points in common; if they are different lines but have the same slope, they are parallel and have no point of intersection; otherwise they have a single point of intersection. We know that the intersection point has to satisfy both line equations so, in order to find its coordinates, we solve them simultaneously:

	l1.a*x + l1.b*y + l1.c = 0 (multiply by l2.b)
	l2.a*x + l2.b*y + l2.c = 0 (multiply by l1.b)
	
	l1.a*l2.b*x + l1.b*l2.b*y + l1.c*l2.b = 0 (-) (substract the second one from the first one)
	l2.a*l1.b*x + l1.b*l2.b*y + l1.b*l2.c = 0
	
	(l1.a*l2.b - l2.a*l1.b)*x + l1.c*l2.b - l1.b*l2.c = 0
	
	x = (l1.b*l2.c - l1.c*l2.b) / (l1.a*l2.b - l2.a*l1.b)
	
To find _y_, we just replace _x_ with the value obtained and solve for _y_.

### lineSegIntersect

```cpp

bool lineSegIntersect(Point a, Point b, Point c, Point d) {
	double ucrossv1 = cross(toVec(a, b), toVec(a, c));
	double ucrossv2 = cross(toVec(a, b), toVec(a, d));
	if (ucrossv1 * ucrossv2 > 0) return false;
	double vcrossu1 = cross(toVec(c, d), toVec(c, a));
	double vcrossu2 = cross(toVec(c, d), toVec(c, b));
	return (vcrossu1 * vcrossu2 <= 0);
}

```

**Input:** Four points representing two line segments (AB and CD).

**Output:** A boolean, whether the two line segments intersect or not.

Checks if two line segments intersect each other, given the points for each line segment.

For this problem, it is important to understand **orientation**. Orientation of an ordered triplet of points in a plane can be counterclockwise, clockwise, or collinear, as in the following image.

![alt text](https://www.geeksforgeeks.org/wp-content/uploads/orientation11.png)

Two lines intersect only if both of the following conditions are satisfied:

	– Points A-B-C and A-B-D have different orientations
	– Points C-D-A and C-D-B have different orientations
	
We also know that if the magnitude of the cross product of two vectors is positive, it forms a left turn. If it is negative, it forms a right turn. If it is zero, it is collinear.

Knowing all this information, then we just have to check that the cross product of A-B-C and A-B-D have different orientations and that the cross product of C-D-A and C-D-B have different orientations. These have different orientations if their product is greater than zero because only two positives or two negatives produce this result.

**Note:** This function does not work for collinear line segments.

### distToLine

```cpp

double distToLine(Point p, Point a, Point b, Point &c) {
	Vec ap = toVec(a, p), ab = toVec(a, b);
	double u = dot(ap, ab) / norm_sq(ab);
	c = translate(a, scale(ab, u));
	return dist(p, c);
}

```

**Input:** Four points. The first point is a single point. The second and third points represent a line. The fourth point is passed by reference.

**Output:** A double value, the distance from point _p_ to the line.

Calculates the distance from point _p_ to the line defined by _a_ and _b_ (_a_ and _b_ must be different) and stores the intersection in _c_. To do this, we first compute the location of the point in the line _AB_ that is closest to point _p_ (we'll call this point _c_) and then obtain the Euclidean distance between _p_ and _c_. We can view point _c_ as point _a_ translated by a scaled magnitude _u_ of vector _AB_. To get _u_, we do a scalar projection of vector _AP_ onto vector _AB_ by using dot product. Finally, we return the Euclidean distance between _p_ and _c_.

### distToLineSegment

```cpp

double distToLineSegment(Point p, Point a, Point b, Point &c) {
	Vec ap = toVec(a, p), ab = toVec(a, b);
	double u = dot(ap, ab) / norm_sq(ab);
	if (u < 0.0) { c = a; return dist(p, a); }
	if (u > 1.0) { c = b; return dist(p, b); }
	return distToLine(p, a, b, c);
}

```

**Input:** Four points. The first point is a single point. The second and third points represent a line segment. The fourth point is passed by reference.

**Output:** A double value, the distance from point _p_ to the line segment _AB_.

Calculates the distance from point _p_ to the line segment _AB_ and stores the intersection in _c_. If we are given a line segment instead of a line, then the minimum distance from point _p_ to line segment _AB_ must also consider two special cases: the end points _a_ and _b_ of that line segment. We handle these cases by adding the conditions to check if _p_ is closer to _a_ (u < 0.0) or to _b_ (u > 1.0). If neither of these conditions is true, we run the algorithm as with a normal line.
