# Algorithms

## Process

1. Confirm your assumptions out loud:
   1. What does the function do exactly? \(Ex: Produce two times the given number.\)
   2. What type of data does the function receive, and what type of data does the function produce. \(Ex: int-&gt; bool\)
   3. What can I assume about: null/empty cases, range cases \(positive/negative\), edge cases
2. "Obviously I could do " {some brute force solution} ". Which is a " {time complexity of solution} " solution."
3. Talk your way to an optimized solution until you both agree that it is a good solution. Don't stop until at least a linear solution is reached, but keep thinking until you can't think of anything better or they tell you to go ahead.
   1. Is this a permutation of an algorithm you already know?
   2. Less looping: Can I loop through my structure only once and get my answer? What about not looping through the entire thing at all? \(Binary search\)
   3. Did you use the best data structure for the job? What's important/unimportant? Access by value? Access by index? Inserting and removing? Etc.
   4. Can you do it in place?
4. Write the code and optimize the solution with your interviewers.
   1. Can the code be cleaner by refactoring?

## Complexity / Analysis

* The analysis of an algorithm is compared against itself. Not other algorithms. So even if it takes 1000 steps, if it does that every time, it's constant.
* The purpose of **asymptotic notation** \(getting rid of everything around the _n_ that become insignificant at infinite\) is to give you an idea for the rate of growth on any machine, since assuming each operation takes 1 unit of time is overly presumptuous. \(Machines differ with number of cores, read/write speed, etc.\)
* Big O is the worst case, and most common that we care about. Omega \(As in, Ω\(1\)\) is the **best** case. Theta is for when the best and worst case are the same. Like if you store a `len` variable to check the length of a sequence. That's theta of 1. Or constant.
* Big O \(In order of efficiency\):
  * **O\(1\)**: Constant: Same number of iterations, no matter how many elements you're dealing with.
  * **O\(log n\)**: Logarithmic : Possible iterations cuts in half every run of the loop.
  * **O\(n\)**: Linear: You only have to go through the elements once.
  * **O\(n log n\)**: n log n or "linearithmic": Happens when you do something logarithmic, but you also have to do something for every element. \(Ex: Logarithmic sort, then a linear solution. Or a solution that compares elements in a logarithmic way, but has to repeat that check for every element.\)
  * **O\(n^2\):** Quadratic: Usually a loop within a loop, unless the inner loop's counter is not initialized every time the outer loop resets.
  * **O\(2^n\)**: Exponential.
* Space complexity works the same way. If your function take a collection of n elements, and creates an exact copy of that collection, then your function is linear in its space complexity. Even if you erase it later.

## Quick and Dirty Rules

* Set an upper limit on an incrementing variable w/ modulo by the upper limit from the variable. So `hour % = 24` will never increment higher than 24.
* Reset to that upper limit with a decrementing variable with `(variable + upper_limit) % upper_limit`. Then if it goes to -1, it'll be 1 minus the upper limit.
* To get the last digit of an int, modulo by 10. To chop off that last digit, divide by 10.
* Convert from decimal to any base:
* To get the number of a char in the alphabet, convert it to uppercase, then to an int, then subtract it from 64 \(1 under the char 'A', which is 65\).
* For n characters, there are n! \(n factorial\) permutations of their arrangement. Unless they can be both upper and lower case, then it's $$2^n*(n!)$$
* **Euclidean Algo \(Find the greatest common divisor\)**: The GCD of two numbers does not change when one is replaced by the difference between the two. Therefore if you repeatedly replace the larger number with the difference between the two numbers, the numbers will eventually be equal to each other. This is the GCD. Since this can take very long if the larger number is much greater than the smaller, the algo becomes more efficient if you use the modulo of the two numbers until you get 0 \(making the larger number the GCD\), instead of subtraction.
* **Reverse and Integer**: To get the last digit of any int, just run mod 10 \(remainder of 10 is always the last digit\). Now the current reversed number is the previous reversed number \(originally set to 0\), times 10 \(to give a blank digit\), plus the last digit you just got. Divide the int by 10, which simply erases the last digit since ints don't have any decimals, and save that to the new original number. This will eventually become 0 \(since ints don't have decimals\). That's when the loop stops.

## Sorting

* "Naive" sorting algorithms run in `O(n^2)` while enumerating all pairs.
* "Sophisticated" sorting algorithms run on `O(nlog(n))`.
* An important algorithm design technique is to use sorting as a basic building block, because many other problems become easy once a set of items is sorted, like: searching, finding closest pair, finding unique elements, frequency distribution, selection by order, set of numbers intersection, etc.

##### Selection Sort

```java
public void sort(int[] a) {
    for (int i = 0; i < a.length; i++)
        swap(a, i, min(a, i));
}

private int min(int[] a, int start) {
    int smallest = start;

    for (int i = start + 1; i < a.length; i++)
        if (a[i] < a[smallest])
            smallest = i;

    return smallest;
}
```

* Each time find the minimum item, remove it from the list, and continue to find the next minimum; takes `O(n^2)`.
* Selection and Insertion are very similar, with a difference that after `k` iterations Selection will have the `k` smallest elements in the input, and Insertion will have the arbitrary first `k` elements in the input that it processed.
* Selection Sort _writes_ less to memory \(Insertion writes every step because of swapping\), so it may be preferable in cases where writing to memory is significantly more expensive than reading.

##### Insertion Sort

```java
public void sort(int[] a) {
    for (int j = 1; j < a.length; j++) {
        int key = a[j];
        int i = j - 1;

        while (i >= 0 && a[i] > key) {
            a[i + 1] = a[i];
            i--;
        }

        a[i + 1] = key;
    }
}
```

* Iterate thru `i = 2 to n`, `j = i to 1` and swap needed items; takes `O(n^2)`.
* Insertion sort is a little more efficient than selection, because the inner `j` loop uses a while, only scanning until the right place in the sorted part of the array is found for the new item. Selection sort scans all items to always find the minimum item.

##### Mergesort

```java
public int[] sort(int[] a) {
    if (a.length <= 1) return a;
    return sort(a, 0, a.length - 1);
}

private int[] sort(int[] a, int low, int high) {
    if (low == high)
        return new int[]{a[low]};

    int mid = (low + high) / 2;
    int[] sorted1 = sort(a, low, mid);
    int[] sorted2 = sort(a, mid + 1, high);

    return merge(sorted1, sorted2);
}

private int[] merge(int[] a, int[] b) {
    int[] res = new int[a.length + b.length];
    int i = 0, j = 0;

    while (i < a.length && j < b.length) {
        if (a[i] < b[j])
            res[i + j] = a[i++];
        else
            res[i + j] = b[j++];
    }

    while (i < a.length)
        res[i + j] = a[i++];

    while (j < b.length)
        res[i + j] = b[j++];

    return res;
}
```

* A recursive approach to sorting involves partitioning the elements into two groups, sorting each of the smaller problems recursively, and then interleaving \(merging\) the two sorted lists to totally order the elements.
* The efficiency of mergesort depends upon how efficiently we combine the two sorted halves into a single sorted list.
* Merging on each level is done by examining the first elements in the two merged lists. The smallest element must be the head of either of the lists. Removing it, the next element must again be the head of either of the lists, and so on. So the merge operation in each level is linear.
* This yields an efficiency of `O(nlog(n))`.
* Mergesort is **great** for sorting **linked lists** because it does not access random elements directly \(like heapsort or quicksort\), but **DON'T** try to sort linked lists in an interview.
* When sorting arrays with mergesort an additional 3rd array buffer is _required_ for the merging operation \(can be implemented in-place tho without an additional buffer, but requires complicated buffer manipulation\).
* Classic _divide-and-conquer_ algorithm, the key is in the merge implementation.

##### Quicksort

```java
public void sort(int[] a) {
    sort(a, 0, a.length - 1);
}

private void sort(int[] a, int low, int high) {
    if (low >= high) return;
    int pivot = partition(a, low, high);
    sort(a, low, pivot - 1);
    sort(a, pivot + 1, high);
}

private int partition(int[] a, int low, int high) {
    int pivot = low;
    int rand = new Random().nextInt(high - low + 1) + low;
    swap(a, low, rand);

    for (int i = low + 1; i <= high; i++) {
        if (a[i] < a[pivot]) {
            swap(a, i, pivot + 1);
            swap(a, pivot, pivot + 1);
            pivot++;
        }
    }

    return pivot;
}
```

* Can be done in-place \(using swaps\), doesn't require an additional buffer.
* Select a random item `p` from the items we want to sort, and split the remaining `n-1` items to two groups, those that are above `p` and below `p`. Now sort each group in itself. This leaves `p` in its exact place in the sorted array.
* Partitioning the remaining `n-1` items is linear \(this step is equivalent to the "merge" part in merge sort; merge =&gt; partition\).
* Total time is `O(n * h)` where `h` = height of the recursion tree \(number of recursions\).
* If we pick the median element as the pivot in each step, `h = log(n)`; this is the best case of quicksort.
* If we pick the left- or right-most element as the pivotal element each time \(biggest or smallest value\), `h = n` \(worst case\).
* On average quicksort will produce good pivots and have `nlog(n)` efficiency \(like binary search trees insertion\).
* If we are most unlucky, and select the extreme values, quicksort becomes selection sort and runs `O(n^2)`.
* For the best case to work, we need to actually select the pivot randomly, or always select a specific index and randomize the input array beforehand.
* Randomization is a powerful tool to improve algorithms with bad worst-case but good average-case complexity.
* Quicksort can be applied to real world problems – like the _Nuts and Bolts_ problem \(first find a match between a random nut and bolt, then split the rest into two groups: bigger and smaller. Repeat in each group\).
* Experiments show that a well implemented quicksort in typically 2-3 times faster than mergesort or heapsort. The reason is the innermost loop operations are simpler. This could change on specific real world problems, because of system behavior and implementation. They have the same asymptotic behavior after all. Best way to know is to implement both and test.

##### Heapsort

\(See also [Heap in Data Structures](topics/data-structures.md#heap)\)

```java
public void sort(int[] a) {
    // build a max-heap
    for (int i = a.length - 1; i >= 0; i--)
        heapify(a, i, a.length);

    // extract max element from the head to the end and shrink the size of the heap
    for (int last = a.length - 1; last >= 0; last--) {
        swap(a, 0, last);
        heapify(a, 0, last);
    }
}

// heapify for a max-heap:
private void heapify(int[] a, int root, int length) {
    int left = 2 * root + 1;
    int right = 2 * root + 2;
    int largest = root;

    if (left < length && a[largest] < a[left])
        largest = left;

    if (right < length && a[largest] < a[right])
        largest = right;

    if (largest != root) {
        swap(a, root, largest);
        heapify(a, largest, length);
    }
}
```

* An implementation of selection sort, but with a priority queue \(implemented by a balanced binary tree\) as the underlying data structure, and not a linked list.
* It's an in-place sort, meaning it uses no extra memory besides the array containing the elements to be sorted.
* The first stage of the algorithm builds a max-heap from the input array \(in-place\) by iterating elements from last to first and calling _heapify_ on each \(cost: `O(n)`\).
* Next, the largest element of the heap is the root; we "extract" it \(swap the root with the last element\) _reducing_ the size of the heap \(this is important\), and calling _heapify_ again for the new root of the smaller heap.
* Continue until we're done with the entire heap.
* Time complexity is `O(nlog(n))`.

##### Distribution Sort

* Split a big problem into sub \(ordered\) "buckets" which need to be sorted themselves, but then all results can just be concatenated. An example: a phone book. Split names into buckets for the starting letter of the last name, sort each bucket, and then combine all strings to one big sorted phone book. We can further split each pile based on the second letter of the last name, reducing the problem even further.
* Bucketing is effective if we are confident that the distribution of data will be **roughly uniform** \(much like hash tables\).
* Performance can be terrible if data distribution is not like we thought it is.

##### External Sort

* Allows sorting more data then can fit into memory.
* For example, assume we have 900MB file, 100MB RAM.
* Read 100MB chunks of the data to memory and sort \(quicksort, mergesort, etc.\) then save to file, until all 900MB is sorted in chunks.
* Read the first 10MB of each sorted chunk to input buffers \(=90MB\) + allocate an output buffer \(10MB\) – sizes can be adjusted.
* Perform a 9-way merge \(mergesort is 2-way by default\) and store the result in the output buffer.
* Once the output buffer is full, write it to disk to the sorted destination file, empty it, and continue merging the input buffers.
* If any of the input buffers gets empty, fill it with the next 10MB from its chunk.
* Continue until all chunks are processed.
* Total runtime is `nlog(n)`.
* If we have a lot of data and limited RAM, we can run the whole process in two passes: split to chunks \(500 files\), combine 25 chunks at a time resulting in 20 larger chunks, run second merge pass to merge the 20 larger sorted chunks.

##### Counting / Radix Sort \(Linear\)

```java
public int[] sort(int[] a) {
    int max = findMax(a);
    int[] sorted = new int[a.length];
    int[] counts = new int[max + 1];

    for (int i = 0; i < a.length; i++)
        counts[a[i]]++;

    for (int i = 1; i < counts.length; i++)
        counts[i] += counts[i - 1];

    for (int i = 0; i < a.length; i++) {
        sorted[counts[a[i]] - 1] = a[i];
        counts[a[i]]--;
    }

    return sorted;
}

private int findMax(int[] a) {
    if (a.length == 0) return 0;

    int max = Integer.MIN_VALUE;
    for (int i = 0; i < a.length; i++) {
        if (a[i] > max)
            max = a[i];
    }
    return max;
}
```

* Counting sort – `O(n)`, stable, assuming input is integers between `0...k` and that `k = O(n)` – create a new array that first stores the number of appearances for each index, and then accumulate each of the cells to know how many total elements were before each index.
* Radix sort – use counting sort multiple times to sort on the least significant digit, then the next one, etc.

## Searching

##### Binary Search

```java
public int search(int[] a, int x) {
    return search(a, x, 0, a.length - 1);
}

private int search(int[] a, int x, int low, int high) {
    if (low > high) return -1;
    int mid = (low + high) / 2;

    if (a[mid] == x) return mid;
    else if (a[mid] > x) return search(a, x, low, mid - 1);
    else return search(a, x, mid + 1, high);
}
```

* Fast algorithm for searching in a **sorted** array of keys.
* To search for key `q`, we compare `q` to the middle key `S[n/2]`. If `q` appears before `S[n/2]`, it must reside in the top half of `S`; if not, it must reside in the bottom half of `S`. Repeat recursively.
* Efficiency is `O(logn)`.
* Several interesting algorithms follow from simple variants of binary search:
  * Eliminating the "equals check" in the algorithm \(keeping the bigger/smaller checks\) we can find the boundary of a block of identical occurrences of a search term. Repeating the search with the smaller/bigger checks swapped, we find the other boundary of the block.
  * One-sided binary search: if we don't know the size of the array, we can test repeatedly at larger intervals \(`A[1]`, `A[2]`, `A[4]`, `A[8]`, `A[16]`, `...`\) until we find an item larger than our search term, and then narrow in using regular binary search. This results in `2*log(p)` \(where `p` is the index we're after\), regardless how large the array is. This is most useful when `p` is relatively close to our start position.
* Binary search can be used to find the roots of continuous functions, assuming that we have two points where `f(x) > 0` and `f(x) < 0` \(there are better algorithms which use interpolation to find the root faster, but binary search still works well\).

##### Randomization

* Randomize array \(`O(nlogn)`\) – create a new array with "priorities", which are random numbers between `1 - n^3`, then sort the original array based on new priorities array as the keys.
* Randomize array in place \(`O(n)`\) – swap `a[i]` with `a[rand(i, n)]`.

## Selection \(`k`-th smallest element\)

* Sort array and return `k`-th element, `O(nlogn)`.
* Quickselect – `O(n)` expected, `O(n^2)` worst – like quicksort with the partition method, but only recurses into one of the parts \(where `k` is\).

```java
public int quickselect(int[] a, int k) {
    return quickselect(a, k, 0, a.length - 1);
}

private int quickselect(int[] a, int k, int low, int high) {
    int pivot = partition(a, low, high);
    if (pivot == k) return a[pivot];
    else if (k < pivot) return quickselect(a, k, low, pivot - 1);
    else return quickselect(a, k, pivot + 1, high);
}

private int partition(int[] a, int low, int high) {
    int pivot = low;
    int rand = new Random().nextInt(high - low + 1) + low;
    swap(a, low, rand);

    for (int i = low + 1; i <= high; i++) {
        if (a[i] < a[pivot]) {
            swap(a, i, pivot + 1);
            swap(a, pivot, pivot + 1);
            pivot++;
        }
    }

    return pivot;
}
```

* Median of medians select, `O(n)`:
  * Based on quickselect.
  * Finds an approximate median in linear time – this is the _key_ step – which is then used as a pivot in quickselect.
  * It uses an \(asymptotically\) optimal approximate median-selection algorithm to build an \(asymptotically\) optimal general selection algorithm.
  * Can also be used as pivot strategy in with quicksort, yielding an optimal algorithm, with worst-case complexity `O(nlogn)`.
  * In practice, this algorithm is typically outperformed by instead choosing random pivots, which has average linear time for selection and average log-linear time for sorting, and avoids the overhead of computing the pivot.
  * The algorithm divides the array to groups of size 5 \(the last group can be of any size &lt;= 5\) and then calculates the median of each group by sorting and selecting the middle element.
  * It then finds the median of these medians by recursively calling itself, and selects the median of medians as the pivot for partition.

## Graphs

##### Utility Function

```java
private void reset(Graph graph) {
    for (Vertex v : graph.vertices.values()) {
        v.parent = null;
        v.discovered = false;
        v.distance = Integer.MAX_VALUE;
    }
}
```

##### BFS Graph Traversal

```java
public void bfs(Graph graph, Vertex source) {
    reset(graph);
    Queue<Vertex> q = new LinkedList<>();
    q.add(source);
    source.discovered = true;

    while (!q.isEmpty()) {
        Vertex from = q.remove();

        for (Edge e : from.edges) {
            Vertex to = graph.vertices.get(e.to);

            if (!to.discovered) {
                to.parent = from;
                to.discovered = true;
                q.add(to);
            }
        }
    }
}
```

* Breadth-first search usually serves to find shortest-path distances from a given source in terms of _number of edges_ \(not weight\).
* Start exploring the graph from the given root \(can be any vertex\) and slowly progress out, while processing the oldest-encountered vertices first.
* We assign a direction to each edge, from the discoverer `(u)` to the discovered `(v)` \(`u` is the parent of `v`\).
* This defines \(results in\) a tree of the vertices starting with the root \(breadth-first tree\).
* The tree defines the shortest-path from the root to every other node in the tree, making this technique useful in shortest-path problems.
* Once a vertex is discovered, it is placed in a queue. Since we process these vertices in first-in, first-out order, the oldest vertices are expanded first, which are exactly those closest to the root.
* During the traversal, each vertex discovered has a parent that led to it. Because vertices are discovered in order of increasing distance from the root, the unique path from the root to each node uses the smallest number of edges on any root to node path in the graph.
* This path has to be constructed backwards, from the destination node to the root.
* BFS can have any node as the root – depends on what paths we want to find.
* BFS returns the shortest-path in terms of _number of edges_ \(for both directed and undirected graphs\), not in terms of weight \(see shortest-paths below\).
* Traversal is `O(n + m)` where `n` = num of vertex and `m` = total num of edges.
* Applications:
  * Find shortest-path in terms of number of edges
  * Garbage collection scanning
  * Connected components
    * "Connected graph" = there is a path between _any_ two vertices.
    * "Connected component" = set of vertices such that there is a path between every pair of vertices; there must not be a connection between two components \(otherwise they would be the same component\).
    * Many problems can be reduced to finding or counting connected components \(like solving a Rubik's cube – is the graph of legal configurations connected?\).
    * A connected component can be found with BFS – every node found in the tree is in the same connected component. Repeat the search from any undiscovered vertex to define the next component, until all vertices have been found.
  * Vertex-colorings \(testing a graph for bipartite-ness\)
    * Assigning colors to vertices \(as few colors as possible\), such that no edge connects two vertices with the same color.
    * Such problems arise in scheduling applications \(like register allocation in compilers\).
    * A graph is "bipartite" if it can be colored without conflicts with only two colors.
    * Such graphs arise in many applications, like: had-sex-with graph for heterosexuals \(men only have sex with women\), that is male vertices are only connected to female vertices, and vice-versa.
    * The problem – find the separation between the vertices for the two colors.
    * Such a graph can be validated with BFS: start with the root, and keep coloring each new vertex the opposite color of it's parent. Then make sure that no non-discovered edge links two vertices of the same color. If no conflicts are found – problem solved. Otherwise, it isn't a bipartite graph.
    * BFS allows us to separate vertices into two groups after running this algorithm, which we can't do just from the structure of the graph.

##### DFS Graph Traversal

```java
public void dfs(Graph graph) {
    reset(graph);

    for (Vertex v : graph.vertices.values()) {
        if (!v.discovered)
            dfs(graph, v);
    }
}

private void dfs(Graph graph, Vertex v) {
    v.discovered = true;
    // TODO: insert application of DFS here

    for (Edge e : v.edges) {
        Vertex to = graph.vertices.get(e.to);

        if (to.discovered) {
            // cycle found (back edge)! What should we do? depends on the application...
        } else {
            to.parent = v;
            dfs(graph, to);
        }
    }

    // TODO: insert application of DFS here. For example: if we're doing topological sorting
    //       then add v to head of a linked list at this point
}
```

* Depth-first search is often a subroutine in another algorithm.
* Starts exploring the graph from the root, but immediately expanding as far as possible \(in contrast to BFS: breadth vs depth\). Retraction back is only done when all neighboring vertices have already been processed.
* While BFS relied on a queue \(FIFO\) for new discovered vertices to be processed, DFS relies on stack \(LIFO\) \(not really uses a stack but recursion, which functions as a stack\).
* DFS classifies all edges into 2 categories: tree edges and back edges. Tree edges progress you down the tree to next vertices. Back edges take you back into some earlier part on the tree, to some ancestor.
* Start with the root, find the first child, process it, and run DFS on it recursively.
* After processing all reachable vertices, if any undiscovered vertices remain, depth-first search selects one of them as a new root and repeats from that root. The algorithm is repeated until it has discovered every vertex.
* Each vertex is timestamped \(by incremented int\): once when it's discovered and once when a vertex's edges have been examined.
* This defines \(results in\) a depth-first forest, comprising of several depth-first trees.
* Traversal is `O(n + m)` where `n` = num of vertices and `m` = total num of edges.
* Applications:
  * Topological sort – create a linear ordering of a DAG \(directed acyclic graph\), such that for edge `(u, v)` then `u` appears before `v` in the ordering; computed by DFS and adding each "visited" vertex into the _front_ of a linked list.
  * Finding cycles – any back edge means that we've found a cycle. If we only have tree edges then there are no cycles \(to detect a back edge, when you process the edge `(x,y)` check if `parent[x] != y`\).
  * Finding articulation vertices \(articulation or cut-node is a vertex that removing it disconnects a connected component, causing loss of connectivity\). Finding articulation vertices by brute-force is `O(n(n + m))`.
  * Strongly connected components – in a SCC of a directed graph, every pair of vertices `u` and `v` are reachable from each other.

##### Shortest-Paths

* Find the path with minimal edge _weights_ between a source vertex and every other vertex in the graph.
* Similarly, BFS finds a shortest-path for an _unweighted_ graph in terms of number of edges.
* Sub-paths of shortest-paths are also shortest-paths.
* Graphs with negative-weight cycles don't have a shortest-paths solution.
* Some algorithms assume no negative weights.
* Shortest-paths have no cycles.
* We will store the path itself in `vertex.parent` attributes, similar to BFS trees; the result of a shortest-paths algorithm is a _shortest-paths tree_: a rooted tree containing a shortest-path from the source to every vertex that is reachable.
* There may be more than one shortest-path between two vertices and more than one shortest-paths tree for a source vertex.
* Each vertex `v` maintains an attribute `v.distance` which is an upper bound on the weight of a shortest-path \(_"shortest-path estimate"_\) from source `source` to `v`; `v.distance` is initialized to `Integer.MAX_VALUE` and `source.distance` is initialized to `0`.
* The action of _relaxing_ an edge `(from, to)` tests whether we can improve the shortest-path to `to` found so far by going through `from`, and if so updating `to.parent` and `to.distance` \(and the priority queue\).
* **Dijkstra's**

```java
public void dijkstra(Graph graph, Vertex source) {
    reset(graph);
    source.distance = 0;
    PriorityQueue<Vertex> q = new PriorityQueue<>(graph.vertices.values());

    while (!q.isEmpty()) {
        Vertex from = q.remove();

        for (Edge edge : from.edges) {
            Vertex to = graph.vertices.get(edge.to);
            int newDistance = from.distance + edge.weight;

            if (newDistance < to.distance) {
                to.distance = newDistance;
                to.parent = from;
                q.remove(to);
                q.add(to);
            }
        }
    }
}
```

* * Algorithm for finding the shortest-paths between nodes in a graph, which may represent, for example, road networks.
  * Assumes that edge weight is nonnegative.
  * Fixes a single node as the _source_ node and finds shortest-paths from the source to all other nodes in the graph, producing a shortest-path tree.
  * The algorithm uses a min-priority queue for vertices based on their `.distance` value \(shortest-path estimate\)
  * It also maintains a set of vertices whose final shortest-path weights from the source have already been determined.
* **A\***
  * An extension of Dijkstra which achieves better time performance by using heuristics.

##### Minimum Spanning Tree

* Convert a weighted graph to a tree reaching all nodes \(_spanning_\), while having the minimal weight \(_minimum_\).

## Backtracking

* General algorithm for finding all \(or some\) solutions to a problem, that incrementally builds candidates to the solution, and abandons \("backtracks"\) each partial candidate as soon as it determines that it cannot possibly be completed to a valid solution.
* The classic textbook example of the use of backtracking is the eight queens puzzle; another one is the knapsack problem.
* Backtracking can be applied only for problems which admit the concept of a "partial candidate solution" and a relatively quick test of whether it can possibly be completed to a valid solution.
* When it is applicable, however, backtracking is often much faster than brute force enumeration of all complete candidates, since it can eliminate a large number of candidates with a single test.

## Lavenshtein Distance

* A string metric for measuring the difference between two sequences.
* The Levenshtein distance between two words is the minimum number of single-character edits \(i.e. insertions, deletions or substitutions\) required to change one word into the other.

## P, NP, NP-complete

* A problem is in class `P` if its solution may be _found_ in polynomial time.
* A problem is in class `NP` if its solution may be _verified_ in polynomial time.
* A problem in `P` is in `NP` by definition, but the converse may not be the case.
* `NP`-complete is a family of `NP` problems for which you know that if one of them has a polynomial solution then everyone of them has.
* Examples of `NP`-complete problems: traveling salesman, knapsack, graph coloring.
* Once you've reduced a problem to `NP`-complete, you know to give up on an efficient fast algorithm and to start looking at approximations.
* For `NP`-complete problems, no polynomial-time algorithms are known for solving them \(although they can be verified in polynomial time\).
* The most notable characteristic of `NP`-complete problems is that no fast solution to them is known.
* `NP`-hard: non-deterministic polynomial time.

# Algorithms Code Examples

### In-Order

```java
public void inOrder(Tree tree) {
    if (tree == null) return;

    inOrder(tree.left);
    // process tree.value
    inOrder(tree.right);
}
```

Without recursion:

```java
public void inOrder(Tree tree) {
    Stack<Tree> stack = new Stack<>();
    Tree curr = tree;

    while (curr != null || !stack.isEmpty()) {
        while (curr != null) {
            stack.push(curr);
            curr = curr.left;
        }

        curr = stack.pop();
        // process curr.value
        curr = curr.right;
    }
}
```

### Pre-Order

```java
public void preOrder(Tree tree) {
    if (tree == null) return;

    // process tree.value
    preOrder(tree.left);
    preOrder(tree.right);
}
```

Without recursion:

```java
public void preOrder2(Tree tree) {
    Stack<Tree> stack = new Stack<>();
    stack.push(tree);
    Tree curr;

    while (!stack.isEmpty()) {
        curr = stack.pop();

        // process curr.value
        if (curr.right != null) stack.push(curr.right);
        if (curr.left != null) stack.push(curr.left);
    }
}
```

### Post-Order

```java
public void postOrder(Tree tree) {
    if (tree == null) return;

    postOrder(tree.left);
    postOrder(tree.right);
    // process tree.value
}
```

Without recursion:

```java
public void postOrder(Tree tree) {
    Stack<Tree> tmp = new Stack<>();
    Stack<Tree> all = new Stack<>();
    tmp.push(tree);
    Tree curr;

    while (!tmp.isEmpty()) {
        curr = tmp.pop();
        all.push(curr);
        if (curr.left != null) tmp.push(curr.left);
        if (curr.right != null) tmp.push(curr.right);
    }

    while (!all.isEmpty()) {
        curr = all.pop();
        // process curr.value
    }
}
```



