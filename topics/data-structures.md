# Data Structures

* Choose the data structure based on what needs to be stored, and what the most frequent operations are.

## Arrays

```cpp
array<int, 5> num_arr = {1, 2, 3, 4, 5};
array<array<int, 3>, 2> my_arr = {{{1, 2, 3}, {4, 5, 6}}};

vector<int> vec = {1, 2, 3, 4, 5};
vec.push_back(6);
```

* Use when: You're required to. Typically use `vector` instead. Use pretty much whenever you need a sequence container, but especially when you want random access.
* Access: **O\(1\)**
* Insertion/Deletion: **O\(n\)** anywhere other than the end. `push_back` is **O\(1\) **for the end.

## Linked Lists

* Use when: You have to. Usually for an interview questions or competition problem. `Vector` typically a better use.
* Access: **O\(n\)**
* Insertion/Deletion: **O\(n\) **anywhere but the beginning unless you have the iterator \(which is linear to achieve\) \(No random access\). **O\(1\)** for `push_front`. \(No shifting. Just change pointers.\)  .

## Stacks and Queues

* **Stacks** – LIFO \(push, pop\)
  * Good for checking even numbered symmetry and walking backwards to check your actions \(function calls, CTRL+Z methods, etc\).
  * Can only see the `top()` of it. Think real life. If standing directly over a stack of plates, you couldn't see any other plates.
  * **O\(1\)** for `push`, `pop`, `top`, `size`, and `empty`.
* **Queues** – FIFO \(push = enqueue, pop = dequeue in C++\)
  * Good for checking repitition and keeping track of waiting requests \(processors, printers, etc\).
  * Can see `front()` and `back()`.
  * **O\(1\)** for `push`, `pop`, `front`, `back`, `size`, and `empty`.
* No iterating over these. Just accessing and modifying the edges.

## Associative Structures

* **Set**: When you want the elements to be unique. **O\(1\)** insert/erase only if you give the hint. Else **O\(log n\)** since they're implemented with binary search trees. Search always logarithmic.
* **Hash Table / Unordered\_map**: When you want **O\(1\)** for key/value lookup. Can also get first element in a map \(which is ordered\): `(A.begin())->first`
* **Hashset**: When you want **O\(1\)** for `contains`, which is done with `count()`. Insert and delete also **O\(1\)** for one element.
* **Priority Queue** / **Heap**: When you want **O\(1\)** for the min/max element in a collection. _Priority_ can be value, length, id, etc.

## Binary Search Trees

* A hierarchical structure used for storing naturally hierarchical data \(DOM, File system\), data to be organized \(numbers\), Tries for dictionaries, etc.
* Depth of a node = length from root to itself. Height = length from itself to it furthest leaf. **Find the total** height of a tree by recursively calculating the height of the left, and the height of the right subtree, and returning the max among that. **O\(n\)**
* A binary tree is a tree with at most two children. Each node has 3 properties. Value, left child, and right child. Children both of type node.
* A binary _search_ tree is a binary tree where _every_ element greater than a node on its right, and every element less than a node on its left. The same must be true for both the root tree and every subtree. This also means that the min element is the leftmost descendant of the root, and the max is the rightmost.
* **Balanced** means that the difference between the height of the left and right subtrees from `root` isn't greater than 1. It is generally good to rebalance after each insertion / deletion.
* Search, insertion, and deletion are **O\(log n\) **if tree is balanced.

### Tree Traversal

* DFS \(depth first, using a stack.
  * Left always before right. Root means you can read the data. 
* * Pre-order: Root -&gt; left -&gt; right \(Root is before \(pre\) children\).
  * In-order: Left -&gt; root -&gt; right \(Root is within \(in\) children\). Used for printing the sorted list of elements.
  * Post-order: left -&gt; right -&gt; root \(Root is after \(post\) children\).
* BFS \(breadth first, using a queue\)
  * Visit all nodes at current level, advance to next level \(doesn't use recursion\). Also called "level-order traversal". 

### Balanced BST

* A self-balancing binary search tree is any node-based binary search tree that automatically keeps its height small in the face of arbitrary item insertions and deletions.

* **Red-Black Tree**

  * Self-balancing is provided by painting each node with one of two colors in such a way that the resulting painted tree satisfies certain properties that don't allow it to become significantly unbalanced.
  * When the tree is modified, the new tree is subsequently rearranged and repainted to restore the coloring properties.
  * The properties are designed in such a way that this rearranging and recoloring can be performed efficiently.
  * The balancing of the tree is not perfect but it is good enough to allow it to guarantee searching, insertion, and deletion operations, along with the tree rearrangement and recoloring in `O(log(n))` time.
  * Tracking the color of each node requires only 1 bit of information per node because there are only two colors.
  * Properties: root is black, all null leaves are black, both children of every red node are black, every simple path from a given node to any of its descendant leaves contains the same number of black nodes.
  * From this we get =&gt; the path from the root to the furthest leaf is no more than twice as long as the path from the root to the nearest leaf. The result is that the tree is roughly height-balanced.
  * Insertion/deletion may violate the properties of a red–black tree. Restoring the red-black properties requires a small number \(`O(log(n))` or amortized `O(1)`\) of color changes. Although insert and delete operations are complicated, their times remain `O(log(n))`.

## Graphs

* Go [here](https://www.topcoder.com/community/data-science/data-science-tutorials/introduction-to-graphs-and-their-data-structures-section-1/), [here](https://www.topcoder.com/community/data-science/data-science-tutorials/introduction-to-graphs-and-their-data-structures-section-2/), and [here](https://www.topcoder.com/community/data-science/data-science-tutorials/introduction-to-graphs-and-their-data-structures-section-3/).
* A Graph is comprised of vertices `V` \(the points\) and edges `E` \(the lines connecting the points\).
* Assume `n` is the number of vertices and `m` is the number of edges.
* Directed graphs: inner city streets \(one-way\), program execution flows.
* Undirected graphs: family hierarchy, large roads between cities.
* Data structures:
  * **Adjacency Matrix**: we can represent a graph using an `n * n` matrix, where element `[i,j] = 1` if `(i, j)` is an edge \(an edge from point `i` to point `j`\), and `0` otherwise.
  * The matrix representation allows rapid updates for edges \(insertion, deletion\) or to tell if an edge is connecting two vertices, but uses a lot of space for graphs with many vertices but relatively few edges.
  * **Adjacency Lists**: we can represent a sparse graph by using linked lists to store the neighbors adjacent to each vertex.
  * Lists make it harder to tell if an edge is in the graph, since it requires searching the appropriate list, but this can be avoided.
  * The parent list represents each of the vertices, and a vertex's inner list represents all the vertices the vertex is connected to \(the edges for that vertex\).
  * Each vertex appears at least twice in the structure – once as a vertex with a list of connected vertices, and at least once in the list of vertex for the vertices it's connected to \(in undirected graphs\).
* We'll mainly use adjacency lists as a graph's data structure.
* Traversing a graph \(breadth or depth\) uses 3 states for vertices: undiscovered, discovered, processed \(all edges have been explored\).
* Most fundamental graph operation is traversal.



