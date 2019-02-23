# Point

### Declaring π (pi) constant
```cpp
const double PI = 2*asin(1);
```
## Helper functions

### equals
```cpp

bool eq(double a, double b) { return fabs(a-b) < EPS; }

```

**Input:** Two double values _a_ and _b_.

**Output:** A boolean, whether _a_ is equal to _b_ or not.

Gets the absolute value of the subtraction of _a_ and _b_ and considers them equal if the difference is less than a margin of error _EPS_.

### less than
```cpp

bool les(double a, double b) { return !eq(a, b) && a < b; }

```

**Input:** Two double values _a_ and _b_.

**Output:** A boolean, whether _a_ is less than _b_ or not.

Uses the helper function _eq()_ to make sure the two values are not equal and then compares  _a_ < _b_. It's necessary to call _eq()_ first because _a_ could be less than _b_ by a very small margin less than _EPS_, by which we consider _a_ to be equal to _b_.

## Struct Point

```cpp

struct Point {
	double x, y, z;
	Point() : x(0), y(0), z(0) {}
	Point(double x, double y) : x(x), y(y), z(0) {}
	Point(double x, double y, double z) : x(x), y(y), z(z) {}
	bool operator <(const Point &p) const {
        return les(x, p.x) || (eq(x, p.x) && les(y, p.y)) || (eq(x, p.x) && eq(y, p.y) && les(z, p.z));
	}
    bool operator==(const Point &p) {
        return eq(x, p.x) && eq(y, p.y) && eq(z, p.z);
    }
};

```

### DEG_to_RAD

Converts degrees to radians.

```cpp

double DEG_to_RAD(double deg) {
    return deg/180*2*asin(1);
}

```

**Input:** A double value _deg_ which represents a value in degrees.

**Output:** A double value which represents the value of _deg_ in radians.

Uses the following formula to convert degrees to radians:
![alt text](http://www.radianstodegrees.net/img/degrees-to-radians2.png)

### distance 2D

Gets the distance between two points in 2-dimensional space.

```cpp

double dist(Point p1, Point p2) {
	return sqrt(pow(p1.x-p2.x, 2) + pow(p1.y-p2.y, 2));
}
	
```

**Input:** Two Point structs, _p1_ and _p2_.

**Output:** Double value representing the distance between _p1_ and _p2_.

Uses the following formula to get the distance between two points:

![alt text](http://www.onlinemath4all.com/images/dbtpformula.png)

### distance 3D

Gets the distance between two points in 3-dimensional space.

```cpp

double dist(Point p1, Point p2) {
	return sqrt(pow(p1.x-p2.x, 2) + pow(p1.y-p2.y, 2) + pow(p1.z-p2.z, 2));
}
	
```

**Input:** Two Point structs, _p1_ and _p2_.

**Output:** Double value representing the distance between _p1_ and _p2_.

### rotate

```cpp

Point rotate(Point p, double theta) {
	double rad = DEG_to_RAD(theta);
	return Point(p.x*cos(rad) - p.y*sin(rad),
				 p.x*sin(rad) + p.y*cos(rad));
}

```

**Input:** A Point _p_ relative to the plane's origin and a double value _theta_ representing the angle of rotation in degrees.

**Output:** A Point after the rotation _theta_ has been applied.

Constructs a new Point struct, passing to the constructor the X and Y coordinates adjusted to the given angle, using the following equation to rotate a point:

![alt text](https://i.imgur.com/ryNN05z.png)

### ANG

```cpp

double ANG(double rad) { return rad*180/PI; }

```
**Input:** A double value representing an angle in radians.

**Output:** A double valie which represents the value of _rad_ in degrees.

Uses the same formula from _DEG_to_RAD()_ to convert the given radians to degrees.

### angulo

```cpp

double angulo(Point p) {
	double d = atan(double(p.y)/p.x);
	if(p.x < 0)
		d += PI;
	else if(p.y < 0)
		d += 2*PI;
	return ANG(d);
}

```
**Input:** A point _p_.

**Output:** A double value that represents the angle of rotation of _p_ expressed in degrees.

Gets the angle of rotation of _p_ in radians and adjusts it depending on its _x_ and _y_ coordinates. Uses the function _ANG()_ to convert the resulting radians to degrees.
