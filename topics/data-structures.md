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
* You can **check whether** a tree is a BST by visiting a node checking whether it's value is within a certain range \(start with int min - int max for root\), then with two recursive calls that it's true for both of its children, setting the new max = root for the left child, and min = root for the right child. This is **O\(n\)**. You could also print in-order and check at each step that it's sorted.
* **Balanced** means that the difference between the height of the left and right subtrees from `root` isn't greater than 1. It is generally good to rebalance after each insertion / deletion.
* Search, insertion, and deletion are **O\(log n\) **if tree is balanced.

### Tree Traversal

* DFS \(depth first, using a stack.\)
  * Left always before right. Root means you can read the data.  All three are **O\(n\)**.
* * Pre-order: Root -&gt; left -&gt; right \(Root is before \(pre\) children\). Used for pretty print.
  * In-order: Left -&gt; root -&gt; right \(Root is within \(in\) children\). Used for printing the sorted list of elements.
  * Post-order: left -&gt; right -&gt; root \(Root is after \(post\) children\).
* BFS \(breadth first, using a queue\)
  * Visit all nodes at current level, advance to next level \(doesn't use recursion\). Also called "level-order traversal". 
  * First you place a node into a queue, handle it, then enqueue both children \(left first\). **O\(n\)** time and space complexity.

#### Pre-Order

```cpp
void preOrder(Node * node) {
    if (node == NULL) return;

    cout << node->value << endl;
    preOrder(node->left);
    preOrder(node->right);
}
```

Without recursion \(Level-order / BFS looks the same as this, except you use a `queue` , `.front()`, and the left goes in first.\):

```cpp
void preOrder2(Node * node) {
    stack<Node*> stack;
    stack.push(node);
    Node * curr;

    while (!stack.empty()) {
        curr = stack.top();
        cout << curr->value << endl;

        stack.pop();

        if (curr->right != NULL) stack.push(curr->right);
        if (curr->left != NULL) stack.push(curr->left);
    }
}
```

#### In-Order

```cpp
void inOrder(Node * node) {
    if (node == NULL) return;

    inOrder(node->left);
    cout << node->value << endl;
    inOrder(node->right);
}
```

#### Post-Order

```cpp
void postOrder(Node * node) {
    if (node == NULL) return;

    postOrder(node->left);
    postOrder(node->right);
    cout << node->value << endl;
}
```

## Balanced BST

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

```cpp
struct Vertex {
    std::string value = NULL;
    set<Vertex *> connected;
    // node fields (name, word length, etc.)
    // ...
};

class Graph {
  private:
    std::unordered_set<Vertex *> vertices;
  public:
    Graph(int v); // create an empty graph with v vertices
    void add_edge(Vertex * a, const Vertex &b); // look up vertex a in map and add to the vertex adjacency list
    void dfs(Vertex * cur, unordered_set<Vertex *> &visited);
    void bfs(Vertex * start);
    // Other methods
    // ...
};
```

* **Directed** graphs / Digraph: the web \(web page is a node, links are the edges\), program execution flows. Edges are represented as an ordered pair. `(a, b)` where `a` is always the origin and `b` the destination.
* **Undirected** graphs: social network \(A user is a node, friendship is the edge\), large roads between cities. Edges are represented as an unordered pair.
* **Weighted** graph: Each edge has different weights. \(Giving different numbers to different path lengths, etc.\)
* In a simple \(no self loops or multi-edges\) directed graph, maximum `|e| = |v|(|v|-1)`. In an undirected graph, it's half that number. 0 is the minimum. This is only if the graph is simple. A denser graph is a graph with more edges. Sparse is the opposite. The list of nodes is always stored in a map with key = value, to avoid linear lookup if you're given a node value instead of an index. You use different structures based on whether the graph is dense or sparse to store the edges:
  * **Adjacency Matrix**: Used for _dense_ graphs \(number of edges closer to `|v|(|v|-1)` than not\). Use a matrix \(2d array\) with a size of `|v| * |v|` . Set element `[i,j] = 1` if an edge exists from point `i` to point `j`\), and `0` otherwise. 
    * There are two positions for each edge. If `A[i][j]` exists, so does `A[j][i]`. For an undirected graph, you only need to go half way.
    * To see if two nodes directly _connected_, you simply go to one node's row, and the column of the other `A[i][j]`. To see all adjacent nodes, just look for all indices with `1` as a value in its row.
    * For **weighted **graphs, use the `weight` of the edge for the matrix value instead of `1`,  and some arbitrarily low/high number that would never be reached for non-edge slots, like negative infinite.
    * This is terrible in terms of space consumption, unless there are more `1's` than `0's`.
  * **Adjacency Lists**: Used for _sparse_ graphs \(more common\). Simply use an array of lists to store the neighbors adjacent to each vertex \(typically a linked list, BST, or set \(not unordered set because they're slower with iteration\)\). Use a linked list for a weighted graph, because then you just add another field to each node for the weight.
    * Each vertex appears at least twice in the structure – once as a vertex with a list of connected vertices, and at least once in the list of vertex for the vertices it's connected to \(in undirected graphs\).
    * The parent list represents each of the vertices, and a vertex's inner list represents all the vertices the vertex is connected to \(the edges for that vertex\).

### Graph Traversal

##### DFS

```java
void Graph::dfs(Vertex * cur, unordered_set<Vertex *> &visited) {
    if (visited.count(cur)) 
        return;   
    cout << cur->value << endl;   
    visited.insert(cur);   
    for (Vertex * v : cur->connected)
        dfs(v, visited);
}
```

##### BFS

```cpp
void Graph::bfs(Vertex * start) {
    queue<Vertex*> q;
    unordered_set<Vertex*> visited;
    
    q.push(start);
    cout << start->value << ' ';
    visited.insert(start);
    
    while (!q.empty()) {
        Vertex * cur = q.front();
        q.pop();
        
        if (!visited.count(cur)) {
            cout << cur->value << endl;
            visited.insert(cur);
            for (Vertex * v : cur->connected)
                q.push(v);
        }
    }
}
```

##### 



