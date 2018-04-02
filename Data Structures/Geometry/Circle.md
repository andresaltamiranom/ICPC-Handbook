# Circle

### Circle Through 2 Points and Radius

```cpp

bool circle2PtsRad(Point p1, Point p2, double r, Point &c) {
  double d2 = (p1.x - p2.x) * (p1.x - p2.x) + (p1.y - p2.y) * (p1.y - p2.y);
  double det = r * r / d2 - 0.25;
  if (det < 0.0) return false;
  double h = sqrt(det);
  c.x = (p1.x + p2.x) * 0.5 + (p1.y - p2.y) * h;
  c.y = (p1.y + p2.y) * 0.5 + (p2.x - p1.x) * h;
  return true;
}

```

**Input:** Two Points (somewhere on the edge of a circle), the radius of the circle, and a Point passed by reference.

**Output:** A boolean value, whether it is possible to find the center of a circle given the two points and a radius. Point _c_ will contain the center point of the corresponding circle.

Given two points that are somewhere on the edge of a circle and the radius of the corresponding circle, we can determine the location of the centers of the two possible circles. Let _c1_ and _c2_ be the centers of the 2 possible circles that go through 2 given points _p1_ and _p2_ and have radius _r_. The quadrilateral _p1_-_c2_-_p2_-_c1_ is a rhombus, since its four sides are equal. Let _m_ be the intersection of the 2 diagonals of the aforementioned rhombus. According to the property of a rhombus, _m_ bisects the 2 diagonals, and the 2 diagonals are perpendicular to each other. We realize that _c1_ and _c2_ can be calculated by scaling the vectors _m-p1_ and _m-p2_ by an appropriate ratio (_m-c1_/_m-p1_) to get the same magnitude as _m-c1_, then rotating the points _p1_ and _p2_ around _m_ by 90 degrees. In this implementation, the variable _h_ is half the ratio _m-c1_/_m-p1_. In the 2 lines calculating the coordinates of one of the centers, the first operands of the additions are the coordinates of _m_, while the second operands of the additions are the result of scaling and rotating vector _m-p2_ around _m_.

**Note:** To get the other center, reverse _p1_ and _p2_.
