# Great-Circle Distance

## Problem Description

A Sphere is a perfectly round geometrical object in 3D space.

The Great-Circle Distance between any two points A and B on a sphere is the shortest distance along a path on the **surface of the sphere**. This path is an _arc_ of the Great-Circle of that sphere that passes through the two points A and B. We can imagine the Great-Circle as the resulting circle that appears if we cut the sphere with a plane so that we have two _equal_ hemispheres (see the left and middle figures).

![alt text](https://i.imgur.com/bRPMQ8U.png)

## Solution(s)

To find the Great-Circle Distance, we have to find the central angle AOB (see the figure on the right) of the Great-Circle where O is the center of the Great-Circle (which is also the center of the sphere). Given the radius of the sphere/Great-Circle, we can then determine the length of arc A-B, which is the required Great-Circle distance.

Although quite rare nowadays, some contest problems involving ‘Earth’, ‘Airlines’, etc. use this distance measurement. Usually, the two points on the surface of a sphere are given as the Earth coordinates, i.e. the (latitude, longitude) pair. The following library code will help us obtain the Great-Circle distance given two points on the sphere and the radius of the sphere. We omit the derivation as it is not important for competitive programming.

```cpp
double gcDistance(double pLat, double pLong, double qLat, double qLong, double radius) {
	pLat *= PI / 180; pLong *= PI / 180; // convert degree to radian
	qLat *= PI / 180; qLong *= PI / 180;
	return radius * acos(cos(pLat)*cos(pLong)*cos(qLat)*cos(qLong) + cos(pLat)*sin(pLong)*cos(qLat)*sin(qLong) + sin(pLat)*sin(qLat)); 
}
```
