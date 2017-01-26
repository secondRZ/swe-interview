# Algorithms

## Process

1. Confirm your assumptions out loud:
   1. What does the function do exactly? \(Ex: Produce two times the given number.\)
   2. What type of data does the function receive, and what type of data does the function produce. \(Ex: int-&gt; bool\)
   3. What can I assume about: null/empty cases, range cases \(positive/negative\), edge cases
2. "Obviously I could do " {some brute force solution} ". Which is a " {time complexity of solution} " solution."
3. Talk your way to an optimized solution until you both agree that it is a good solution. Don't stop until at least a linear solution is reached, but keep thinking until you can't think of anything better or they tell you to go ahead.
   1. Is this a permutation of an algorithm you already know?
   2. Less looping: Can I loop through my structure only once and get my answer? What about not looping through the entire thing at all? \(Binary search, sqrt for factors and prime, two pointer approach where the inner loop can not be reset every time the outer loop runs and decrease instead of increasing, etc.\)
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
* To get the last digit of an int, modulo by 10. To chop off that last digit, divide by 10. To get the first, **divide** by the 1 \* however many digits are in the number. To chop off the first digit, **modulo** by that number. Notice they're reversed.
* To get the number of a char in the alphabet, convert it to uppercase, then to an int, then subtract 64 from it \(1 under the char 'A', which is 65\).
* **Permutations**: For n elements in a fixed set, there are `n!` \(n factorial\) permutations of their arrangement. Unless they can be both upper and lower case, then it's $$$$`2^n * (n!)`. If they can be any character then it's `k^n` where `k` is the amount of available characters, and `n` is the length of the collection. 
* **Euclidean Algo \(Find the greatest common divisor\)**: The GCD of two numbers does not change when one is replaced by the difference between the two. Therefore if you repeatedly replace the larger number with the difference between the two numbers, the numbers will eventually be equal to each other. This is the GCD. Since this can take very long if the larger number is much greater than the smaller, the algo becomes more efficient if you use the modulo of the two numbers until you get 0 \(making the larger number the GCD\), instead of subtraction. If you continuously swap the first number for the second, then you don't have to worry about min and max. Like `tmp = A; A = B % A; B = A;`
* **Reverse and Integer**: To get the last digit of any int, just run mod 10 \(remainder of 10 is always the last digit\). Now the current reversed number is the previous reversed number \(originally set to 0\), times 10 \(to give a blank digit\), plus the last digit you just got. Divide the int by 10, which simply erases the last digit since ints don't have any decimals, and save that to the new original number. This will eventually become 0 \(since ints don't have decimals\). That's when the loop stops.
* **Largest Possible Number from joining array elements into a string**: Turn them into a string, Then using the `sort` algorithm, pass a comparison function that  appends string `B` to string `A`. Then appends string `A` to string `B`. And finally run `stoi(str1) > stoi(str2)`.
* **Fibonacci**: `a[i] = a[i - 2] + a[i - 1]`. To find nth element in the sequence,  Always iterative. Never recursively.

## Math

* **Find All Factors**: If you find that a number `n` % another number `d == 0`, then you have found 2 factors of `n`. `d` and `n / d`. The upper limit to finding all `d` values is `sqrt(n)`, so you loop only has to go that far \(**all the way to** `<=`, not just `<`\), and add both `d` and`n / d` every iteration, unless `d == sqrt(n)`, then you only enter it for itself, not for `n / d`.
* **Verify prime number**: 2 is the lowest prime number. 1 is not prime. Same thing as finding all factors. You only need to go to the square root of the number.
* **Decimal to nary**: Modulo by n. Add the remainder to the vector. Divide by n. Loop again. Loop only while num &gt; 0.
* **Nary to Decimal**: n to the power of the level of the digit you're at \(starting at 0\) \* whatever the digit represents in decimal.

## Sorting

* "Naive" sorting algorithms run in `O(n^2)` while enumerating all pairs.
* "Sophisticated" sorting algorithms run on `O(nlog(n))`.
* **Stable** sorting algorithms keep the same order for secondary properties of elements as the were before the sort. 

##### Insertion Sort `O(n^2)`

```cpp
void InsertionSort(array<int, 10> &arr) {
    for (int i = 1; i < arr.size(); i++) {
        int value = arr[i],
            hole = i;

        while (hole > 0 && arr[hole - 1] > value) {
            arr[hole] = arr[hole - 1];
            hole--;
        }
        arr[hole] = value;
    }
}
```

Used when you have limited time to implement a sort. Simply the easiest to write.

1. Start looping through the array. i will serve as a `marker` to the elements already sorted, so start the loop at 1.
2. Set the comparison `value` to a variable \(`array[i]`\).
3. Set `hole` to `i` \(It resets automatically at every stage at every stage\).
4. Start the comparison `while` loop, which continues only while you haven't shifted all of the sorted elements \(`marker > 0`\), **and** the next sorted element is still smaller than the comparison variable.
5. Within the loop, set `array[hole]` to the next comparison element \(`[hole - 1]`\) \(the one that was checked in the while condition\), and decrement `hole`.
6. Once the `while` loop is over \(you've shifted all of the elements, or the next element that you would have shifted is not smaller than the comparison value\) set `array[hole]` to the value.

##### Mergesort `O(n log n)`

```cpp
void Merge(vector<int> &base_array, vector<int> left, vector<int> right) {
    int i = 0, j = 0;
    while (i < left.size() && j < right.size()) {
        if (left[i] < right[j]) {
            base_array[i + j] = left[i];
            i++;
        } else {
            base_array[i + j] = right[j];
            j++;
        }
    }

    while (i < left.size()) {
        base_array[i + j] = left[i];
        i++;
    }
    while (j < right.size()) {
        base_array[i + j] = right[j];
        j++;
    }
}

void MergeSort(vector<int> &arr) {
    if (arr.size() == 1)
        return;

    int half = arr.size() / 2;
    vector<int> left, right;

    for (int i = 0; i < half; i++)
        left.push_back(arr[i]);
    for (int i = half; i < arr.size(); i++)
        right.push_back(arr[i]);

    MergeSort(left);
    MergeSort(right);
    Merge(arr, left, right);
}
```

Used when your sort must be stable \(can't use quicksort or heapsort\), or must use a linked-list due to interview constraints \(doesn't need random access like heapsort and quicksort\), and memory is not an issue \(this sort uses the worst memory\).

1. Make sure you pass the array as a **reference** in both functions.
2. If the array only has one element then it is already sorted. Exit the function. This is the stop condition for the recursion.
3. Split the list in half. \(If the list has an odd number of elements, one side just has one more\) by dividing the length by 2 \(in an `int` so we get no floating numbers\). `left` will be of size half, `right` will be of size `length - half`.
4. Then run two separate `for` loops. One is`i = 0; i < half;` setting `left[i] = base_array[i]` and the other is`i = half; i < length;` setting `right[i] = base_array[i]`
5. Now call the function from within itself for **both** the left **and** the right. \(The entire left sub-arrays will finish first if called first, then the right\)
6. Because this is a recursive call, it will continue until there is only one element in a sub-array \(when a split hits the stop condition.\).
7. At the end of the recursion, the sub-arrays are sorted \(an array with one element is already sorted\), call the merge function and pass `left`, `right`, and the `base_array`. The base\_array will be overwritten, so pass it as a **reference**.
   1. Create two `markers`. One for your position in both sub-arrays. `i` and `j`
   2. Enter into a loop that goes `while` you haven't run out of elements to check in either array.
   3. Compare the two smallest elements of each sub-array, and overwrite the next element of the base array with the smaller of the two. \(Which is `array[i + j]`\)
   4. Move the `marker` up on the sub-array that you just used an element from.
   5. One of the sub-arrays will finish before the other. So you enter a `while` loop for the unfinished one. While there are still elements to check, overwrite the next element of the base array, which is still `array[i + j]`. Increment the `marker` every time.

##### Quicksort: Average: `O(n log n)`, worst `O(n^2)`

```cpp
int partition(int * array, int start, int end) {
    int pivot = array[end];
    int p_index = start;
    for (int i = start; i < end; i++) {
        if (array[i] <= pivot) {
            swap(array[i], array[p_index]);
            p_index++;
        }
    }
    swap(array[p_index], array[end]);
    return p_index;
}

void QuickSort(int * array, int start, int end) {
    if (start >= end)
        return;

    int p_index = partition(array, start, end);
    QuickSort(array, start, p_index - 1);
    QuickSort(array, p_index + 1, end);
}
```

Used when you don't need a stable sort \(meaning you don't need mergesort\), and you care more about average case than worst case \(meaning you don't need heapsort\).

Almost always faster than mergesort and heapsort, even with its `n^2` worst case \(which rarely happens\). This is because it uses much less swaps than both of them, which are costly. Randomized version makes `n log n` highly probably. It also uses much less **space** `O(log n)` than mergesort `O(n)`.

1. The function takes an `array`, a `start` index, and an `end` index that needs to be sorted. These mark the boundaries of the segment you're sorting. \(Initial parameters are `start = 0`, and `end = array.length - 1`\).
2. If `start >= end`, then the segment is already sorted. This is the stop condition. 
3. Call the `partition` function, which returns the partition index.  `p_index = partition(array, start, end)`. and places all elements less than the pivot to the left of the index, and all elements greater to the right, modifying the actual array. Partition:
   1. Select a `pivot`. One implementation always selects the last element of the segment. So `pivot = array[end]`.
   2. Set the `p_index` initially to `start` . 
   3. Run a loop from `start` to `< end`.  Inside the loop, if the current element is less than **or** equal to your pivot \(currently the last element\), then swap the current element with the element at `p_index`,  currently the 0. Then `p_index++`.
   4. After the loop, swap the element at `p_index` with `array[end]`, which was you comparative `pivot`.
4. Now call the sort function from within itself **twice**. Once for the left side `QuickSort(array, start, p_index - 1)`and the other for the right side `QuickSort(array, p_index + 1, end)`.

##### Heapsort `O(n log n)`

Use when you don't need a stable sort \(meaning you don't need mergesort\), but under no circumstances can you have less than `O(n log n)`. \(Meaning quicksort is not an option\).

##### External Sort

* Used when we need to sort more data then can fit into memory, usually by using disk.
* For example, assume we have 900MB file, 100MB RAM.
* Read 100MB chunks of the data to memory and sort \(mergesort\) then save to file, until all 900MB is sorted in chunks.
* Read the first 10MB of each sorted chunk to input buffers \(=90MB\) + allocate an output buffer \(10MB\) – sizes can be adjusted.
* Perform a 9-way merge \(mergesort is 2-way by default\) and store the result in the output buffer.
* Once the output buffer is full, write it to disk to the sorted destination file, empty it, and continue merging the input buffers.
* If any of the input buffers gets empty, fill it with the next 10MB from its chunk.
* Continue until all chunks are processed.
* Total runtime is nlog\(n\).
* If we have a lot of data and limited RAM, we can run the whole process in two passes: split to chunks \(500 files\), combine 25 chunks at a time resulting in 20 larger chunks, run second merge pass to merge the 20 larger sorted chunks.

##### Counting Sort `O(n)`

Used when you have a lot of elements, but a limited range of values. \(Ex: A university has 200K students, all between the age of 18 and 22. Sort them by age.

## Searching

##### Binary Search

```java
int search(const vector<int> &haystack, const int &needle, const int &low, const int &high) {
    if (low > high)
        return -1;

    int mid = (low + high)/2;
    if (haystack[mid] == needle)
        return mid;
    else if (haystack[mid] < needle)
        return search(haystack, needle, mid + 1, high);
    else
        return search(haystack, needle, low, mid - 1);

}

int b_search(const vector<int> &haystack, const int &needle) {
    return search(haystack, needle, 0, haystack.size() - 1);
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

* Randomize array in place \(`O(n)`\) – swap `a[i]` with `a[rand(i, n)]`.

## Backtracking

* Backtracking is trying out all possibilities using recursion \(think brute force\). Example: Given a maze, determine whether there is a path from start to finish. At each intersection you can go straight, left, or right. 
* Characterized by three traits:
  * You don't have enough info in the beginning, 
  * Each decision leads to another set of decisions.
  * Any path of choices may lead to a solution.

```cpp
// pseudocode for the maze question

boolean pathFound(Position p) {
    if (p is finish) 
        return true;

    foreach option O from p {
        boolean isThereAPath = pathFound(O);
        if (isThereAPath) return true; // We found a path using option O
    }
    // We have tried all options from this position and none of the options lead to finish. 
    // Hence there is no solution possible to finish
    return false;
}
```

## Hashing

* The process of placing items into their own buckets, being sure to deal with collisions. You want the hash function to spread entries as evenly as possible among the buckets.

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



