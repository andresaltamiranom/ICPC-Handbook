# Vector

## Struct Vec

```cpp

struct Vec {
	double x, y, z;
	Vec(double x, double y, double z) : x(x), y(y), z(z) {}
	Vec() : x(0), y(0), z(0) {}
	Vec(double x, double y) : x(x), y(y), z(0) {}
	Vec(Point a, Point b) : x(b.x-a.x), y(b.y-a.y), z(b.z-a.z) {}
};

```

### To vector

```cpp

Vec toVec(Point a, Point b){ return Vec(a, b); }

```

**Input:** Two Point structs.

**Output:** A Vec struct formed with _a_ and _b_.

Converts two Points to a Vector.

### Scale

```cpp

Vec scale(Vec v, double s) { return Vec(v.x*s, v.y*s, v.z*s); }

```

**Input:** A Vec struct and a double value representing the scaling ratio.

**Output:** A Vec struct after scaling.

Scales a Vector (larger or smaller) given a ratio _s_.

### Translate

```cpp

Point translate(Point p, Vec v) { return Point(p.x+v.x, p.y+v.y, p.z+v.z); }

```

**Input:** A Point and a vector.

**Output:** A Point after translation.

Translates a Point from its original position through a vector.

### Dot product

```cpp

double dot(Vec a, Vec b) { return (a.x*b.x + a.y*b.y + a.z*b.z); }

```

**Input:** Two vectors.

**Output:** A double value, the dot product of the two vectors.

Calculates the dot product of two vectors.

### Normal square

```cpp

double norm_sq(Vec v) { return v.x*v.x + v.y*v.y + v.z*v.z; }

```

**Input:** A vector.

**Output:** A double value, the normal square of the given vector.

Calculates the normal square of a vector by squaring each of its components.

### Rotate 

```cpp

Vec rotate(Vec v, double angle) {
	Matrix rotation = CREATE(2, 2);
	rotation[0][0] = rotation[1][1] = cos(angle);
	rotation[1][0] = sin(angle);
	rotation[0][1] = -rotation[1][0];

	Matrix vec = CREATE(2, 1);
	vec[0][0] = v.x, vec[0][1] = v.y;

	Matrix res = multiply(rotation, vec);
	Vec result(res[0][0], res[0][1]);
	return result;
}

```

**Input:** A vector and an angle in radians, in which the vector will be rotated.

**Output:** A rotated vector.

Rotates a vector by an angle. A rotation matrix of 2x2 is used to support this process. The rotation matrix's top-left and bottom-right cells must be the cosine of the angle, while the bottom left must be the sine of the angle and the top left the negative sine of the angle. Once this matrix is formed, we create a matrix of 2x1 with the vector's _x_ and _y_ values. Finally, matrix multiplication is performed between the two matrices. The resulting _x_ and _y_ values after rotation will be stored on the resulting matrix's top left and bottom left cells.

### Cross product

```cpp

double cross (Vec a, Vec b) { return a.x*b.y - a.y*b.x; }

```

**Input:** Two vectors.

**Output:** A double value, the cross product of the two vectors.

Calculates the cross product of two vectors.

### Counterclockwise

```cpp

bool ccw(Point p, Point q, Point r) { return cross(toVec(p, q), toVec(p, r)) > 0; }

```

**Input:** Three points.

**Output:** A boolean value, whether the three Points form a counterclockwise angle.

Given three points _p_, _q_, and _r_, determines whether these form a counterclockwise angle. Uses helper functions _toVec()_ and _cross()_. If the result of the cross product of the vectors formed by these three points is greater than zero, then it is a counterclockwise angle.

### Collinear

```cpp

bool collinear(Point p, Point q, Point r) { return abs(cross(toVec(p, q), toVec(p, r))) < EPS; }

```

**Input:** Three points.

**Output:** A boolean value, whether the three Points are collinear.

Given three points _p_, _q_, and _r_, determines whether these are collinear. This means that the three points can be found on the same straight line. Uses helper functions _toVec()_ and _cross()_. If the result of the cross product of the vectors formed by these three points is zero (or, in this case, less than _EPS_), then they are collinear.

### Angle

```cpp

double angle(Point a, Point o, Point b) {
	Vec oa = toVec(o, a), ob = toVec(o, b);
	return acos(dot(oa, ob) / sqrt(norm_sq(oa) * norm_sq(ob)));
}

```

**Input:** Three points.

**Output:** A double value, the angle _aob_ formed by the three points, in radians.

Given three points _a_, _o_, and _b_, calculates the angle formed by the vectors _oa_ and _ob_. In this case, _o_ is the fixed point and _a_ and _b_ are the ones that vary to create an angle. Uses helper functions _toVec()_, _dot()_, and _norm_sq()_ to calculate the angle in radians.

