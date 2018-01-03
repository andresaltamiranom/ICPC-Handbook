# Triangle

## Struct Triangle

```cpp

struct Triangle {
	Point A, B, C;
	Triangle() {}
	Triangle(Point A, Point B, Point C) : A(A), B(B), C(C) {}
};

```

### Perimeter

```cpp

double perimeter(double a, double b, double c) { return a+b+c; }

```

**Input:** The lengths of the three sides of the triangle.

**Output:** A double value, the perimeter of the triangle.

Calculates the perimeter of a triangle.

### Area

```cpp

double area(double a, double b, double c){
	double s = perimeter(a, b, c)*0.5;
	return sqrt(s*(s-a)*(s-b)*(s-c));
}

```

**Input:** The lengths of the three sides of the triangle.

**Output:** A double value, the area of the triangle.

Calculates the area of a triangle using Heron's formula.

### Area

```cpp

double area(const Triangle &T) {
	double ab = dist(T.A, T.B);
	double bc = dist(T.B, T.C);
	double ca = dist(T.C, T.A);
	return area(ab, bc, ca);
}

```

**Input:** A Triangle struct.

**Output:** A double value, the area of the triangle.

Calculates the area of a triangle by obtaining the distance of each of the triangle's sides and then using Heron's formula to get the area.

## InCircle

The incircle of a triangle is the largest circle that can be contained in a triangle. This circle is tangent to the three sides of the triangle, such as shown in this image:

![alt text](https://www.mathopenref.com/images/circles/incircle.gif)

### InCircle radius

```cpp

double rInCircle(double ab, double bc, double ca) {
	return area(ab, bc, ca) / (0.5 * perimeter(ab, bc, ca));
}

```

**Input:** The lengths of the three sides of the triangle.

**Output:** A double value, the radius of the incircle of the triangle.

Calculates the radius of the incircle of a triangle. 

### InCircle radius

```cpp
double rInCircle(Point a, Point b, Point c) {
	return rInCircle(dist(a, b), dist(b, c), dist(c, a));
}

```

**Input:** The three points of the triangle.

**Output:** A double value, the radius of the incircle of the triangle.

Calculates the radius of the incircle of a triangle.

### inCircle

```cpp

bool inCircle(Point p1, Point p2, Point p3, Point &ctr, double &r) {
	r = rInCircle(p1, p2, p3);
	if(abs(r) < EPS) return false;
	Line l1, l2;
	double ratio = dist(p1, p2) / dist(p1, p3);
	Point p = translate(p2, scale(toVec(p2, p3), ratio/(1+ratio)));
	l1 = Line(p1, p);
	ratio = dist(p2, p1) / dist(p2, p3);
	l2 = Line(p2, p);
	areIntersect(l1, l2, ctr);
	return true;
}

```

**Input:** The three points of the triangle, a point of the center of a circle passed by reference, and the value of the radius of the circle passed by reference.

**Output:** A boolean value, whether there exists an incircle or not. Also stores in _ctr_ the center point of the incircle, if there is one, and in _r_ the radius of the incircle.

Returns the incircle of a given triangle. First, the radius of the incircle is obtained. If it is too small (less than _EPS_), we assume there is no incircle and return false. We then calculate the ratio of line _p1-p2_ to line _p1-p3_ and use this to get point _p_, which, by translating to _p2_ and scaling to the vector formed by _p2_ and _p3_, gives us point _p_. Then we create two new lines, _p1-p_ and _p2-p_. Their intersection point will be the center of the incircle. This is calculated in the function _areIntersect()_, which stores the point of intersection in _ctr_.

## CircumCircle

The circumcircle is the unique circle that passes through each of the triangle's three vertices, such as shown in this image:

![alt text](http://jwilson.coe.uga.edu/EMAT6680Fa11/Lewis/SciptTools/CircumCircle.gif)

### CircumCircle radius

```cppp

double rCircumCircle(double ab, double bc, double ca) { return ab * bc * ca / (4.0 * area(ab, bc, ca)); }

```

**Input:** The lengths of the three sides of the triangle.

**Output:** A double value, the radius of the circumcircle of the triangle.

Calculates the radius of the circumcircle of a triangle.

### CircumCenter

```cpp

Point circumcenter(const Triangle &T) {
    Point A = T.A, B = T.B, C = T.C;
	double D = 2*(A.x*(B.y - C.y) + B.x*(C.y - A.y) + C.x*(A.y - B.y));
	double AA = A.x*A.x + A.y*A.y, BB = B.x*B.x + B.y*B.y, CC = C.x*C.x + C.y*C.y;
	return Point((AA*(B.y - C.y) + BB*(C.y - A.y) + CC*(A.y - B.y)) / D, (AA*(C.x - B.x) + BB*(A.x - C.x) + CC*(B.x - A.x)) / D);
}

```

**Input:** A Triangle struct.

**Output:** The center of the circumcircle.

Calculates the center point of the circumcircle of a triangle.
