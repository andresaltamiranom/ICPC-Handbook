# Treap

A treap is a binary tree in which every node has both a **search key** and a **priority**, where the inorder sequence of search keys is sorted and each node’s priority is smaller than the priorities of its children. In other words, a treap is simultaneously a binary search tree for the search keys and a (min-)heap for the priorities. From this comes its name: "treap" = "tree heap".

![Treap](https://i.imgur.com/8XHatEf.png)

The idea behind treaps is to use randomness to balance binary search trees. Binary search trees are great as long as they are balanced. But if the elements of the tree are inserted in an ordered way, the tree can turn into a linked list (or at least become extremely unbalanced), leading to O(n) performance. On the other hand, if a set of elements is inserted in a _random order_, the expected distance in the tree to a randomly chosen element is O(log(n)).

Given a set of elements and associated priorities it is not completely obvious that we can construct a treap that satisfies both invariants simultaneously. Clearly the root of the treap must be the node with highest priority. To satisfy the BST invariant, all the nodes whose keys are less than this node must be in the left subtree of this node, and the nodes whose keys are greater must be in the right subtree. Therefore, we can apply this tree construction recursively to the left and right subtrees, resulting in a treap.

Given an existing treap, how do we insert a new element? The algorithm follows the same strategy as in red-black trees: it finds the unique leaf where the element can be inserted while preserving the BST invariant. However, we also assign this element a random priority. The final treap had better look like the binary search tree that one would get if the newly inserted element had been inserted according to its priority. This is achieved by performing a series of tree rotations to enforce the heap ordering invariant. Rotations can be performed in constant time, since they only involve simple pointer manipulation.

![Treap](https://i.imgur.com/3eE4KTJ.png)

The overall time to insert a node z is proportional to the depth of z before the rotations—we have to walk down the treap to insert z, and then walk back up the treap doing rotations. Another way to say this is that the time to insert z is roughly twice the time to perform an unsuccessful search for _key(z)_.

![Treap](https://i.imgur.com/imJ73iT.png)

To delete a node, we just run the insertion algorithm backward in time. Suppose we want to delete node z. As long as z is not a leaf, perform a rotation at the child of z with smaller priority. This moves z down a level and its smaller-priority child up a level. The choice of which child to rotate preserves the heap property everywhere except at z. When z becomes a leaf, chop it off.

### Treaps in Competitive Programming

Since most tree rotations occur near the bottom (i.e., close to the leaves), this makes them useful if for some reason you need a BST where rotations on larger subtrees are more expensive. Consider a subtree T containing the inserted node. You will only have to rotate the root of T if the new node has the highest priority of all nodes in the tree. The probability of this occurring is 1/T. This means that even if rotating a subtree of size T takes time O(T), in expectation you will spend O(1) time doing the rotation, since the probability that you do it at all is 1/T.

Sometimes, in a BST, meta-data is stored in the nodes. It could store aggregate sums over all the nodes in a subtree at its root, for example. Of course, statistics like that can be updated in O(1) time. But if we store more complicated data, especially data that takes linear time to compute, then we'll find this treap property useful.

One example would be a 2D range tree. Our treap stores points sorted by the first dimension. Then each node would store, over the points in its subtree, its own 1D range tree for the second dimension. When we do a rotation, we have to construct a new range tree from scratch. It can also be used when we want to be able to answer connectivity queries efficiently but also allow for edge insertions and removals.

Following is an implementation of a Treap:

```cpp

#define LC(a) ((a) == ((a)->parent->left))
template<typename K>
struct Treap {
	struct Node {
		K key;
		int priority;
		Node *left, *right, *parent;};
	Node *root = 0;
	Treap(Node *root = 0) : root(root) { srand(time(0)); }
	void fixDown(Node *n) {
		bool a, b;
		while((a = (n->left && n->priority < n->left->priority)) || (b = (n->right && n->priority < n->right->priority))) rotate(n, a && b ? rand()%2 : b); }
	Node *find(K key, bool leaf, Node *start, bool onlyLeft = 0) {
		if (!start) return 0;
		Node *n = start, *next;
		while((next = (key < n->key || onlyLeft ? n->left : n->right)) && (leaf || key != n->key)) n = next;
		return n; }
	void insert(K key, int priority = -1) {
		Node *p = find(key, 1, root), *n = new Node { key, priority == -1 ? rand()%100000 : priority , 0, 0, p };
		if (!root) { root = n; return; }
		(key < p->key ? p->left : p->right) = n;
		while(p && n->priority > p->priority) rotate(p, !LC(n)), p = n->parent; }
	void erase(K key) {
		if (!root) return;
		Node *n = find(key, 0, root), *del = n;
		if (!n || n->key != key) return;
		if (n->left && n->right) {
			del = find(key, 1, n->right, 1);
			n->key = del->key, n->priority = del->priority;
			fixDown(n); }
		if (del->left || del->right) (del->left ? del->left : del->right)->parent = del->parent;
		if (del->parent) (LC(del) ? del->parent->left : del->parent->right) = del->left ? del->left : del->right;
		else root = del->left ? del->left : del->right;
		delete del; }
	void rotate(Node *n, bool left) {
		Node *u = (left ? n->right : n->left), *p = n->parent;
		if (p) (LC(n) ? p->left : p->right) = u;
		else root = u;
		Node *c = left ? u->left : u->right;
		(left ? n->right : n->left) = c;
		if (c) (left ? u->left : u->right)->parent = n;
		(left ? u->left : u->right) = n;
		u->parent = p, n->parent = u; }
};

```
