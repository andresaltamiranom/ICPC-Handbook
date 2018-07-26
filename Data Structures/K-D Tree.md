# K-D Tree

A K-D Tree (K-Dimensional Tree) is a binary search tree in which data at each node represents a K-Dimensional point in space. It is a space partitioning data structure for organizing points in a K-Dimensional space.

Every non-leaf node can be thought of as implicitly generating a splitting hyperplane that divides the space into two parts, known as half-spaces.

Points to the left of this hyperplane are represented by the left subtree of that node and points to the right of the hyperplane are represented by the right subtree. The hyperplane direction is chosen in the following way: every node in the tree is associated with one of the k-dimensions, with the hyperplane perpendicular to that dimension's axis. So, for example, if for a particular split the "x" axis is chosen, all points in the subtree with a smaller "x" value than the node will appear in the left subtree and all points with larger "x" value will be in the right subtree. In such a case, the hyperplane would be set by the x-value of the point, and its normal would be the unit x-axis.

The most important application of a K-D Tree is to do a Nearest Neighbor Search.

### Example

A 3-dimensional K-D Tree. The first split (the red vertical plane) cuts the root cell (white) into two subcells, each of which is then split (by the green horizontal planes) into two subcells. Finally, those four cells are split (by the four blue vertical planes) into two subcells. Since there is no more splitting, the final eight are called leaf cells.

![K-D Tree](https://upload.wikimedia.org/wikipedia/commons/b/b6/3dtree.png)

A typical K-D Tree includes operations for constructing the tree, inserting nodes, removing nodes, balancing, range search, and nearest neighbour search.

Due to the curse of dimensionality, which leads most searches in high dimensional spaces to end up being brute searches, K-D Trees are not suitable for efficiently finding the nearest neighbour in high-dimensional spaces. As a general rule, if the dimensionality is k, the number of points in the data, N, should be N >> 2^k. Otherwise, when K-D Trees are used with high-dimensional data, most of the points in the tree will be evaluated and the efficiency is no better than exhaustive search, and, if a good-enough fast answer is required, approximate nearest-neighbour methods should be used instead.

## Complexity

1) Building a static K-D Tree from n points has the following worst-case complexity:

-  O(n * log^2(n)) if an O(n * log(n)) sort such as Heapsort or Mergesort is used to find the median at each level of the nascent tree.

- O(n * log(n)) if an O(n) median of medians algorithm is used to select the median at each level of the nascent tree.

- O(k * n * log(n)) if n points are presorted in each of k dimensions using an O(n * log(n)) sort such as Heapsort or Mergesort prior to building the K-D Tree.

2) Inserting a new point into a balanced K-D Tree takes O(log(n)) time.

3) Removing a point from a balanced K-D Tree takes O(log(n)) time.

4) Querying an axis-parallel range in a balanced K-D Tree takes O(n^(1 − 1 / k) + m) time, where m is the number of the reported points, and k the dimension of the K-D Tree.

5) Finding 1 nearest neighbour in a balanced K-D Tree with randomly distributed points takes O(log(n)) time on average.

Following is an implementation of a K-D Tree:

```cpp

// - constructs from n points in O(n lgˆ2 n) time
// - handles nearest-neighbor query in O(lg n) if points are well distributed
// - worst case for nearest-neighbor may be linear in pathological case
// -----------------------------------------
#include <limits>
// number type for coordinates, and its maximum value
typedef long long ntype;
const ntype sentry = numeric_limits<ntype>::max();
// point structure for 2D-tree, can be extended to 3D
struct point {
	ntype x, y;
	point(ntype xx = 0, ntype yy = 0) : x(xx), y(yy) {}};
	
bool operator==(const point &a, const point &b) {
	return a.x == b.x && a.y == b.y;}
	
// sorts points on x-coordinate
bool on_x(const point &a, const point &b) {
	return a.x < b.x;}
	
// sorts points on y-coordinate
bool on_y(const point &a, const point &b) {
	return a.y < b.y;}
	
// squared distance between points
ntype pdist2(const point &a, const point &b) {
	ntype dx = a.x-b.x, dy = a.y-b.y;
	return dx*dx + dy*dy;}
	
// bounding box for a set of points
struct bbox{
	ntype x0, x1, y0, y1;
	bbox() : x0(sentry), x1(-sentry), y0(sentry), y1(-sentry) {}
	
	// computes bounding box from a bunch of points
	void compute(const vector<point> &v) {
		for (int i = 0; i < v.size(); ++i) {
			x0 = min(x0, v[i].x); x1 = max(x1, v[i].x);
			y0 = min(y0, v[i].y); y1 = max(y1, v[i].y);}}
			
	// squared distance between a point and this bbox, 0 if inside
	ntype distance(const point &p) {
		if (p.x < x0) {
			if (p.y < y0) return pdist2(point(x0, y0), p);
			else if (p.y > y1) return pdist2(point(x0, y1), p);
			else return pdist2(point(x0, p.y), p);}
		else if (p.x > x1) {
			if (p.y < y0) return pdist2(point(x1, y0), p);
			else if (p.y > y1) return pdist2(point(x1, y1), p);
			else return pdist2(point(x1, p.y), p);}
		else {
			if (p.y < y0) return pdist2(point(p.x, y0), p);
			else if (p.y > y1) return pdist2(point(p.x, y1), p);
			else return 0;}}};
			
// stores a single node of the kd-tree, either internal or leaf
struct kdnode {
	bool leaf; // true if this is a leaf node (has one point)
	point pt; // the single point of this is a leaf
	bbox bound; // bounding box for set of points in children
	kdnode *first, *second; // two children of this kd-node
	kdnode() : leaf(false), first(0), second(0) {}
	~kdnode() { if (first) delete first; if (second) delete second; }

	// intersect a point with this node (returns squared distance)
	ntype intersect(const point &p) {
		return bound.distance(p);}
	
	// recursively builds a kd-tree from a given cloud of points
	void construct(vector<point> &vp){
		// compute bounding box for points at this node
		bound.compute(vp);
		
		// if we’re down to one point, then we’re a leaf node
		if (vp.size() == 1) {
			leaf = true;
			pt = vp[0];}
		else {
			// split on x if the bbox is wider than high (not best heuristic...)
			if (bound.x1-bound.x0 >= bound.y1-bound.y0)
				sort(vp.begin(), vp.end(), on_x);
			// otherwise split on y-coordinate
			else
				sort(vp.begin(), vp.end(), on_y);
				
			// divide by taking half the array for each child
			// (not best performance if many duplicates in the middle)
			int half = vp.size()/2;
			vector<point> vl(vp.begin(), vp.begin()+half);
			vector<point> vr(vp.begin()+half, vp.end());
			first = new kdnode(); first->construct(vl);
			second = new kdnode(); second->construct(vr);}}};

// simple kd-tree class to hold the tree and handle queries
struct kdtree{
	kdnode *root;
	
	// constructs a kd-tree from a points (copied here, as it sorts them)
	kdtree(const vector<point> &vp) {
		vector<point> v(vp.begin(), vp.end());
		root = new kdnode();
		root->construct(v);}
		
	~kdtree() { delete root; }
	
	// recursive search method returns squared distance to nearest point
	ntype search(kdnode *node, const point &p){
		if (node->leaf) {
			// commented special case tells a point not to find itself
			// if (p == node->pt) return sentry;
			// else
			return pdist2(p, node->pt);}
		ntype bfirst = node->first->intersect(p);
		ntype bsecond = node->second->intersect(p);
		
		// choose the side with the closest bounding box to search first
		// (note that the other side is also searched if needed)
		if (bfirst < bsecond) {
			ntype best = search(node->first, p);
			if (bsecond < best)
				best = min(best, search(node->second, p));
			return best;}
		else {
			ntype best = search(node->second, p);
			if (bfirst < best)
				best = min(best, search(node->first, p));
			return best;}}
	
	// squared distance to the nearest
	ntype nearest(const point &p) {
		return search(root, p);}};

// --------------------------------------------
// some basic test code here
int main() {
	// generate some random points for a kd-tree
	vector<point> vp;
	for (int i = 0; i < 100000; ++i) {
		vp.push_back(point(rand()%100000, rand()%100000));
	}
	kdtree tree(vp);
	
	// query some points
	for (int i = 0; i < 10; ++i) {
		point q(rand()%100000, rand()%100000);
		cout << "Closest squared distance to (" << q.x << ", " << q.y << ")" << " is " << tree.nearest(q) << endl;}
	return 0;}
// --------------------------------------------
		
```
