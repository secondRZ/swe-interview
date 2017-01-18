# C++

## Data Types

* **int vs unsigned int**: An unsigned int is good for when you know you won't need negative numbers. Same byte size, but no negatives means that you double the possible positive size \(say from ~-32.5K - ~32.5K to 0 - ~65K\)
* **String**
  * `toupper('c')` and `tolower('C')` return an int representing a character from ascii. You can cast it like `(char)toupper('a')`
  * `replace(pos, num, "str");` works like splice. Or use iterators: `replace(str.begin(), str.begin() + 2, "str");`
  * `str.append("appended string")` appends one piece of a string to another. Changes the actual string.
  * Convert from char to string like this: `std::string str(1, char);`
  * You can initiate a string with repeating characters like this `std::string str(5, 'a'); // aaaaa`
* **Number**

`Print Every Digit:`

```cpp
int num = 1234;
std::string num_str = std::to_string(num);
for(char digit : num_str){
    std::cout << digit-'0' << std::endl;
}
```

`Limit Decimal Places:`

```java
#include <iomanip>
#include <sstream>

stringstream stream;
std::stringstream limited_str;
limited_str << fixed << setprecision(2) << 1.0 / 8.5;
cout << limited_str.str();
```

## STL Classes

* **Array**: C style arrays have their own limitations. They decay to pointers when passing them to functions, which causes them to lose their length information, and dynamic arrays get messy when trying to manage memory on your own.
  * Start with`#include <array>`. Then - after using namespace std - `array<int, 5> my_array = {1, 2, 3, 4, 5};` It is passed to `functions with(const array<int, 5>&arr) {}` \(Notice the pass by const reference. This is to prevent the compiler from making a copy of the array for performance reasons. This is a good rule of thumb for STL arrays.\)
  * Common methods:
    * `array.at(index)` access element at index with bounds checking. Throws and error if out of bounds. Slower but safer.
    * `array.front()` accesses the first element.
    * `array.back()` accesses the last element.
    * `array.size()` gets the length of the array.
    * `array.empty()` tests whether an array is empty.
    * `array.max_size()` returns the maximum size of the array.
    * `sort(array.begin(), array.end())`or`(array.rbegin(), array.rend)`to sort the array forward or backward.
* **Vector**: Like a dynamic array, except it manages its own memory. They also keep track of their own length via the `size()` method. Done like `vector<int> = {1, 2, 3, 4, 5}` after using std namespace and `#include <vector>`. Can't enter with random access unless the vector is already that size.
  * Common methods:
    * `vector.resize(int)` resizes the array to the size int.
    * `vector.insert(it_pos, value)` inserts a value before the iterator position.
    * `vector.erase(it_start_pos, it_end_pos)` removes values between and including the ranges of the iterators. If you only use one argument, then it erases the element at that position. You can also pass one argument which is the position you want to erase.
    * `vector.push_back(value)` adds the value to the end of the vector.
    * `vector.pop_back()` removes the last element of the array.
    * All of the above methods for STL `<array>` also.
* **List**: A doubly linked list. Can only access the front and the back, but inserting and removing \(at any point in the list\) is very fast \(Constant time\). Random access is not supported.`#include <list>` then `list<int> my_list = {1, 2, 3, 4, 5}`
  * Common methods: Same as `vector` with the added methods `pop_front()` and `push_front().`
* **Forward List**: A singly linked list. Exactly like the doubly linked list except it uses less storage, and is slightly faster. But it can't iterate backwards. Has no `pop_back()` or `push_back()` methods.
* **Deque** \(pronounced "deck"\): A double-ended queue class. Same methods and initialization as a list. Unlike a list inserting in the middle is linear time, but accessing is constant because of random access.
* **Map**: Key value pair of data with unique keys. Also called an associative array. All keys must be the same type, and all values must be the same type. Search, removal, and insertion all have logarithmic complexity as they are implemented with red-black trees. `#include <map`&gt; and then `map <string, int> my_map;`Then you create key value pairs with bracket notation.
* **Set**: Container that stores unique elements. Sorted after every entry, not the order that you enter the elements. Logarithmic time for insertion, deletion, and search. A multiset is a set that allows duplicates. Same time complexities. Implemented with a Red Black Tree.
* **Unordered** **Set**: Implemented as a Hashset. Like a map, but no need for a key, only a value. Constant speed for "contains". Best if you need random access for a value instead of an index or a key. Add and remove are also constant. `unordered_set<int> set_name;`
* **Stack**: Implements LIFO \(last in first out\) .
* **Queue**: Implements FIFO \(first in first out\).
* **Algorithms**:
  * `min_element` and `max_element` takes `sequence.begin()` and `sequence.end()`.
  * `sort()`
  * `is_sorted`: Checks if a the elements in range are in **non-descending **order. `is_sorted(v.begin, v.end())`
  * `reverse()`
  * `min()` and `max()` return the smallest and largest value of 2 given values. Ex: `min(5, 10) // 5`
* **Iterators**: Peform on **sorted **collections. Initialized with `vector<int>::iterator low;`

  * `lower_bound`: Returns the first element that is not less than the given argument. Only use on sorted sequences. `lower_bound(v.begin(), v.end(), 20);`
  * `upper_bound`: Returns the first element that is GREATER than the given argument. Only use on sorted sequences. `upper_bound(v.begin(), v.end(), 20);`

## Lambdas

* Like a closure and a callback in JS. \[\] \(\) {} syntax. First `#include <functional>` then other functions can accept and return the "function" type. Include and ampersand or equal sign in the square brackets for variable capture \(either by reference or by copy respectively\) \(Put a variable name in the square brackets to specify whether that one alone will be copied or referenced, except for "this", which binds the keyword\), parameters in the parenthesis, and the body inside of the curly brackets. You can include the optional return type after the parenthesis like \[\] \(\) -&gt; int {}

## Files

* `#include <fstream>`
* Create an output file stream for writing with `ofstream StreamName(file_name, ios:out);`
* Write to that stream with `StreamName <<"Here goes some text for the file.\n";`
* Close the stream with `StreamName.close()`
* Create an input file stream with `ifstream Reader(file_name);`
* Read a line of that file with `getline(Reader, line);afterstring line;`
* Close that stream the same way.`Reader.close();`
* Sometimes you're not reading by lines, you're reading by something else, so check whether you're at the end with `Reader.eof();`



