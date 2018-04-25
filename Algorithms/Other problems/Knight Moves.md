# Knight Moves

## Problem Description

In chess, a knight can move in an interesting ‘L-shaped’ way. Formally, a knight can move from a cell (r1, c1) to another cell (r2, c2) in an _n_ × _n_ chessboard if and only if (r1 − r2)^2 + (c1 − c2)^2 = 5. A common query is the length of the shortest moves to move a knight from a starting cell to another target cell. There can be many queries on the same chessboard.

## Solution(s)

### One BFS per Query

If the chessboard size is small, we can afford to run one BFS per query. For each query, we run BFS from the starting cell. Each cell has at most 8 edges connected to other cells (some cells around the border of the chessboard have less edges). We stop BFS as soon as we reach the target cell. We can use BFS on this shortest path problem as the graph is unweighted. As there are up to O(n^2) cells in the chessboard, the overall time complexity is O(n^2 + 8n^2) = O(n^2) per query or O(Q×n^2) if there are Q queries.

### One Precalculated BFS and Handling Special Cases

The solution above is not the most efficient way to solve this problem. If the given chessboard is large and there are several queries, e.g. _n_ = 1000 and Q = 16, the approach above will get **TLE**.

A better solution is to realize that if the chessboard is large enough and we pick two random cells (ra,ca) and (rb,cb) in the middle of the chessboard with shortest knight moves of _d_ steps between them, shifting the cell positions by a constant factor does not change the answer, i.e. the shortest knight moves from (ra + k, ca + k) and (rb + k, cb + k) is also _d_ steps, for a constant factor _k_.

Therefore, we can just run one BFS from an arbitrary source cell and do some adjustments to the answer. However, there are a few special (literally) corner cases to be handled. Finding these special cases can be a headache and many **Wrong Answers** are expected if one does not know them yet.

To find these cases, consider the following:

1) Separate cases when 3 ≤ _n_ ≤ 4 and _n_ ≥ 5.
2) Literally concentrate on corner cells and side cells.
3) What happens if the starting cell and the target cell are too close?
