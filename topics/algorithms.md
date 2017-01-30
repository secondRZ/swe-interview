# Algorithms

## Process

1. Confirm your assumptions out loud:
   1. What does the function do exactly? \(Ex: Produce two times the given number.\)
   2. What type of data does the function receive, and what type of data does the function produce. \(Ex: int-&gt; bool\)
   3. What can I assume about: null/empty cases, range cases \(positive/negative\), edge cases.
2. Talk your way to an optimized solution until you both agree that it is a good solution. Don't stop until at least a linear solution is reached, but keep thinking until you can't think of anything better or they tell you to go ahead.
   1. Is this a permutation of an algorithm you already know?
   2. Less looping: Can I loop through my structure only once and get my answer? What about not looping through the entire thing at all? \(Binary search, sqrt for factors and prime, two pointer approach where the inner loop can not be reset every time the outer loop runs and decrease instead of increasing, etc.\)
   3. Did you use the best data structure for the job? What's important/unimportant? Access by value? Access by index? Inserting and removing? Etc.
   4. Can you do it in place?
3. Write the code and optimize the solution with your interviewers.
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

## Math

* **Find All Factors**: If you find that a number `n` % another number `d == 0`, then you have found 2 factors of `n`. `d` and `n / d`. The upper limit to finding all `d` values is `sqrt(n)`, so you loop only has to go that far \(**all the way to** `<=`, not just `<`\), and add both `d` and`n / d` every iteration, unless `d == sqrt(n)`, then you only enter it for itself, not for `n / d`.
* **Verify prime number**: 2 is the lowest prime number. 1 is not prime. Same thing as finding all factors. You only need to go to the square root of the number.
* **Decimal to nary**: Modulo by n. Add the remainder to the vector. Divide by n. Loop again. Loop only while num &gt; 0.
* **Nary to Decimal**: n to the power of the level of the digit you're at \(starting at 0\) \* whatever the digit represents in decimal.
* **Permutations**: For n elements in a fixed set, there are `n!` \(n factorial\) permutations of their arrangement. Unless they can be both upper and lower case, then it's $$$$`2^n * (n!)`. If they can be any character then it's `k^n` where `k` is the amount of available characters, and `n` is the length of the collection. 
* **Euclidean Algo \(Find the greatest common divisor\)**: The GCD of two numbers does not change when one is replaced by the difference between the two. Therefore if you repeatedly replace the larger number with the difference between the two numbers, the numbers will eventually be equal to each other. This is the GCD. Since this can take very long if the larger number is much greater than the smaller, the algo becomes more efficient if you use the modulo of the two numbers until you get 0 \(making the larger number the GCD\), instead of subtraction. If you continuously swap the first number for the second, then you don't have to worry about min and max. Like `tmp = A; A = B % A; B = A;`
* **Reverse and Integer**: To get the last digit of any int, just run mod 10 \(remainder of 10 is always the last digit\). Now the current reversed number is the previous reversed number \(originally set to 0\), times 10 \(to give a blank digit\), plus the last digit you just got. Divide the int by 10, which simply erases the last digit since ints don't have any decimals, and save that to the new original number. This will eventually become 0 \(since ints don't have decimals\). That's when the loop stops.
* **Fibonacci**: `a[i] = a[i - 2] + a[i - 1]`. To find nth element in the sequence,  Always iterative. Never recursively.
* Set an upper limit on an incrementing variable w/ modulo by the upper limit from the variable. So `hour % = 24` will never increment higher than 24.
* Reset to that upper limit with a decrementing variable with `(variable + upper_limit) % upper_limit`. Then if it goes to -1, it'll be 1 minus the upper limit.
* To get the last digit of an int, modulo by 10. To chop off that last digit, divide by 10. To get the first, **divide** by the 1 \* however many digits are in the number. To chop off the first digit, **modulo** by that number. Notice they're reversed.

## Strings

* To get the number of a char in the alphabet, convert it to uppercase, then to an int, then subtract 64 from it \(1 under the char 'A', which is 65\).
* **Largest Possible Number from joining array elements into a string**: Turn them into a string, Then using the `sort` algorithm, pass a comparison function that  appends string `B` to string `A`. Then appends string `A` to string `B`. And finally run `stoi(str1) > stoi(str2)`.

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

Use when you don't need a stable sort \(meaning you don't need mergesort\), you want a sort to use less memory \(meaning you don't **want** mergesort\), but under no circumstances can you have less than `O(n log n)`. \(Meaning quicksort is not an option\).

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

* Efficiency is **O\(logn\)**.

## Randomization

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

## Dynamic Programming

* Dynamic programming is remembering information about past iterations for quicker processing. "Remember your past". Typically accomplished by saving a variable of past results.

## Greedy Algo

* A greedy algorithm is taking the best guess given the information you have at the time.

## Graphs

* Problems usually involve paths, directions, or grids of some sort. Those keywords should signal you should be thinking about graphs.

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



