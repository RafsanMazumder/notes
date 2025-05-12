# C++ Standard Template Library (STL)

The C++ STL is a powerful collection of template classes and functions providing common data structures and algorithms.

## Containers

### Sequence Containers

#### vector

Dynamic array with automatic resizing.

```cpp
#include <vector>

vector<int> vec;                 // Empty vector
vector<int> vec = {1, 2, 3, 4};  // Initialization with values
vector<int> vec(10, 0);          // Vector of size 10 with all values as 0

// Common operations
vec.push_back(5);                // Add element at the end
vec.pop_back();                  // Remove last element
vec.size();                      // Get size
vec.empty();                     // Check if empty
vec.clear();                     // Remove all elements
vec.resize(5);                   // Resize vector
vec[0];                          // Access element (no bounds checking)
vec.at(0);                       // Access element (with bounds checking)
vec.front();                     // Access first element
vec.back();                      // Access last element
```

**Time Complexity:**
- Access: O(1)
- Insert/Erase at end: O(1) amortized
- Insert/Erase at arbitrary position: O(n)

#### deque

Double-ended queue supporting fast insertion and deletion at both ends.

```cpp
#include <deque>

deque<int> dq;                   // Empty deque
deque<int> dq = {1, 2, 3, 4};    // Initialization with values

// Common operations
dq.push_back(5);                 // Add element at the end
dq.push_front(0);                // Add element at the beginning
dq.pop_back();                   // Remove last element
dq.pop_front();                  // Remove first element
dq.size();                       // Get size
dq.empty();                      // Check if empty
dq.clear();                      // Remove all elements
dq[0];                           // Access element (no bounds checking)
dq.at(0);                        // Access element (with bounds checking)
dq.front();                      // Access first element
dq.back();                       // Access last element
```

**Time Complexity:**
- Access: O(1)
- Insert/Erase at beginning/end: O(1) amortized
- Insert/Erase at arbitrary position: O(n)

#### list

Doubly-linked list.

```cpp
#include <list>

list<int> lst;                   // Empty list
list<int> lst = {1, 2, 3, 4};    // Initialization with values

// Common operations
lst.push_back(5);                // Add element at the end
lst.push_front(0);               // Add element at the beginning
lst.pop_back();                  // Remove last element
lst.pop_front();                 // Remove first element
lst.size();                      // Get size
lst.empty();                     // Check if empty
lst.clear();                     // Remove all elements
auto it = lst.begin();           // Iterator to first element
lst.insert(it, 10);              // Insert 10 before position of iterator
lst.erase(it);                   // Erase element at iterator
lst.front();                     // Access first element
lst.back();                      // Access last element
lst.sort();                      // Sort the list
lst.reverse();                   // Reverse the list
lst.merge(list2);                // Merge two sorted lists
lst.splice(it, list2);           // Insert list2 at position it
```

**Time Complexity:**
- Access: O(n)
- Insert/Erase with known position: O(1)
- Search: O(n)

#### forward_list

Singly-linked list.

```cpp
#include <forward_list>

forward_list<int> fl;            // Empty forward_list
forward_list<int> fl = {1, 2, 3};// Initialization with values

// Common operations
fl.push_front(0);                // Add element at the beginning
fl.pop_front();                  // Remove first element
fl.empty();                      // Check if empty
fl.clear();                      // Remove all elements
auto it = fl.begin();            // Iterator to first element
auto it_before = fl.before_begin(); // Iterator before first element
fl.insert_after(it, 10);         // Insert 10 after position of iterator
fl.erase_after(it);              // Erase element after iterator
fl.front();                      // Access first element
fl.sort();                       // Sort the forward_list
fl.reverse();                    // Reverse the forward_list
```

**Time Complexity:**
- Access: O(n)
- Insert/Erase with known position: O(1)
- Search: O(n)

#### array

Fixed-size array.

```cpp
#include <array>

array<int, 5> arr;               // Uninitialized array of size 5
array<int, 5> arr = {1, 2, 3, 4, 5}; // Initialization with values

// Common operations
arr.size();                      // Get size
arr.empty();                     // Check if empty
arr.fill(0);                     // Fill with 0s
arr[0];                          // Access element (no bounds checking)
arr.at(0);                       // Access element (with bounds checking)
arr.front();                     // Access first element
arr.back();                      // Access last element
```

**Time Complexity:**
- Access: O(1)
- Insert/Erase: Not supported (fixed size)

### Associative Containers

#### set

Collection of unique keys, sorted by keys.

```cpp
#include <set>

set<int> s;                      // Empty set
set<int> s = {1, 2, 3, 4};       // Initialization with values

// Common operations
s.insert(5);                     // Insert element
s.erase(3);                      // Remove element with value 3
s.size();                        // Get size
s.empty();                       // Check if empty
s.clear();                       // Remove all elements
s.find(2);                       // Find element with value 2
s.count(2);                      // Count elements with value 2 (0 or 1)
s.lower_bound(3);                // Iterator to first element >= 3
s.upper_bound(3);                // Iterator to first element > 3
```

**Time Complexity:**
- Insert/Erase/Find: O(log n)

#### multiset

Collection of keys, sorted by keys (allows duplicates).

```cpp
#include <set>

multiset<int> ms;                // Empty multiset
multiset<int> ms = {1, 2, 2, 3}; // Initialization with values

// Common operations (similar to set)
ms.insert(5);                    // Insert element
ms.erase(3);                     // Remove all elements with value 3
auto it = ms.find(2);            // Find first element with value 2
ms.erase(it);                    // Remove specific occurrence of 2
ms.count(2);                     // Count elements with value 2
```

**Time Complexity:**
- Insert/Erase/Find: O(log n)

#### map

Collection of key-value pairs, sorted by keys, keys are unique.

```cpp
#include <map>

map<string, int> mp;             // Empty map
{% raw %}
map<string, int> mp = {{"apple", 5}, {"banana", 3}}; // Initialization with values
{% endraw %}

// Common operations
mp["orange"] = 2;                // Insert or update key-value pair
mp.insert({"pear", 4});          // Insert key-value pair
mp.insert(make_pair("grape", 6));// Another way to insert
mp.erase("apple");               // Remove element with key "apple"
mp.size();                       // Get size
mp.empty();                      // Check if empty
mp.clear();                      // Remove all elements
mp.find("banana");               // Find element with key "banana"
mp.count("banana");              // Count elements with key "banana" (0 or 1)
```

**Time Complexity:**
- Insert/Erase/Find: O(log n)

#### multimap

Collection of key-value pairs, sorted by keys (allows duplicate keys).

```cpp
#include <map>

multimap<string, int> mm;        // Empty multimap
mm.insert({"apple", 5});         // Insert key-value pair
mm.insert({"apple", 3});         // Insert another with same key
```

**Time Complexity:**
- Insert/Erase/Find: O(log n)

### Unordered Associative Containers

#### unordered_set

Collection of unique keys, hashed by keys.

```cpp
#include <unordered_set>

unordered_set<int> us;           // Empty unordered_set
unordered_set<int> us = {1, 2, 3, 4}; // Initialization with values

// Common operations (similar to set, but unordered)
us.insert(5);                    // Insert element
us.erase(3);                     // Remove element with value 3
us.find(2);                      // Find element with value 2
us.count(2);                     // Count elements with value 2 (0 or 1)
```

**Time Complexity:**
- Insert/Erase/Find: O(1) average, O(n) worst case

#### unordered_multiset

Collection of keys, hashed by keys (allows duplicates).

```cpp
#include <unordered_set>

unordered_multiset<int> ums;     // Empty unordered_multiset
ums.insert(2);                   // Insert element
ums.insert(2);                   // Insert duplicate
```

**Time Complexity:**
- Insert/Erase/Find: O(1) average, O(n) worst case

#### unordered_map

Collection of key-value pairs, hashed by keys, keys are unique.

```cpp
#include <unordered_map>

unordered_map<string, int> um;   // Empty unordered_map
um["apple"] = 5;                 // Insert or update key-value pair
um.insert({"banana", 3});        // Insert key-value pair
```

**Time Complexity:**
- Insert/Erase/Find: O(1) average, O(n) worst case

#### unordered_multimap

Collection of key-value pairs, hashed by keys (allows duplicate keys).

```cpp
#include <unordered_map>

unordered_multimap<string, int> umm; // Empty unordered_multimap
umm.insert({"apple", 5});        // Insert key-value pair
umm.insert({"apple", 3});        // Insert another with same key
```

**Time Complexity:**
- Insert/Erase/Find: O(1) average, O(n) worst case

### Container Adaptors

#### stack

LIFO (Last In, First Out) data structure.

```cpp
#include <stack>

stack<int> stk;                  // Empty stack
stk.push(1);                     // Add element to top
stk.push(2);                     // Add element to top
int top = stk.top();             // Access top element (2)
stk.pop();                       // Remove top element
stk.size();                      // Get size
stk.empty();                     // Check if empty
```

**Time Complexity:**
- Push/Pop/Top: O(1)

#### queue

FIFO (First In, First Out) data structure.

```cpp
#include <queue>

queue<int> q;                    // Empty queue
q.push(1);                       // Add element to back
q.push(2);                       // Add element to back
int front = q.front();           // Access front element (1)
int back = q.back();             // Access back element (2)
q.pop();                         // Remove front element
q.size();                        // Get size
q.empty();                       // Check if empty
```

**Time Complexity:**
- Push/Pop/Front/Back: O(1)

#### priority_queue

Provides constant time lookup of the largest (by default) element.

```cpp
#include <queue>

// Max heap (default)
priority_queue<int> pq;          // Empty priority queue
pq.push(3);                      // Add element
pq.push(5);                      // Add element
pq.push(1);                      // Add element
int top = pq.top();              // Access largest element (5)
pq.pop();                        // Remove largest element
pq.size();                       // Get size
pq.empty();                      // Check if empty

// Min heap
priority_queue<int, vector<int>, greater<int>> min_pq;
min_pq.push(3);                  // Add element
min_pq.push(5);                  // Add element
min_pq.push(1);                  // Add element
int min_top = min_pq.top();      // Access smallest element (1)
```

**Time Complexity:**
- Push/Pop: O(log n)
- Top: O(1)

## Algorithms

STL provides various algorithms for common operations on containers.

```cpp
#include <algorithm>

vector<int> v = {5, 2, 8, 1, 3};

// Sorting
sort(v.begin(), v.end());        // Sort in ascending order
sort(v.begin(), v.end(), greater<int>()); // Sort in descending order

// Binary search (on sorted range)
binary_search(v.begin(), v.end(), 3); // Returns true if 3 is in vector
lower_bound(v.begin(), v.end(), 3);   // Iterator to first element >= 3
upper_bound(v.begin(), v.end(), 3);   // Iterator to first element > 3

// Min/Max
*min_element(v.begin(), v.end()); // Smallest element
*max_element(v.begin(), v.end()); // Largest element

// Finding
find(v.begin(), v.end(), 3);     // Iterator to first occurrence of 3
count(v.begin(), v.end(), 3);    // Count occurrences of 3

// Manipulation
reverse(v.begin(), v.end());     // Reverse the vector
rotate(v.begin(), v.begin() + 2, v.end()); // Rotate vector left by 2 positions
random_shuffle(v.begin(), v.end()); // Randomly shuffle elements

// Heap operations
make_heap(v.begin(), v.end());   // Convert range to a heap
push_heap(v.begin(), v.end());   // Add element to heap
pop_heap(v.begin(), v.end());    // Move largest element to end and reconstitute heap

// Numeric
#include <numeric>
accumulate(v.begin(), v.end(), 0); // Sum of elements (starting with 0)
partial_sum(v.begin(), v.end(), result.begin()); // Partial sums
```

## Iterators

Iterators are used to access and traverse container elements.

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Types of iterators
auto it = v.begin();             // Regular iterator
auto rit = v.rbegin();           // Reverse iterator
auto cit = v.cbegin();           // Constant iterator
auto crit = v.crbegin();         // Constant reverse iterator

// Iterator operations
it++;                            // Move to next element
it--;                            // Move to previous element
it += 2;                         // Move forward by 2 (random access)
*it;                             // Access element
it = v.end();                    // Iterator to one past the last element
```

## Utility Classes

### pair

Holds two values of possibly different types.

```cpp
#include <utility>

pair<string, int> p("apple", 5); // Create pair
auto p = make_pair("apple", 5);  // Alternative creation
string first = p.first;          // Access first element
int second = p.second;           // Access second element
```

### tuple

Holds a fixed-size collection of elements of different types.

```cpp
#include <tuple>

tuple<string, int, double> t("apple", 5, 3.14); // Create tuple
auto t = make_tuple("apple", 5, 3.14);          // Alternative creation
string first = get<0>(t);        // Access first element
int second = get<1>(t);          // Access second element
double third = get<2>(t);        // Access third element
```

## Best Practices

1. **Use the Right Container**: Choose based on your access patterns and required operations.
2. **Avoid Premature Optimization**: Start with a straightforward solution, then optimize if needed.
3. **Range-based for Loop**: Prefer range-based for loops for cleaner code.
   ```cpp
   for (const auto& elem : container) {
       // Use elem
   }
   ```
4. **Use auto for Iterator Types**: Makes code more readable and maintainable.
5. **Leverage STL Algorithms**: They're well-tested and often more efficient than manual implementations.
6. **Pass by Reference**: For large containers, pass by reference to avoid copying.
7. **Reserve Vector Capacity**: If you know the size in advance, use `vector::reserve()` to avoid reallocations.
8. **Use emplace_back() Instead of push_back()**: For objects that require construction, `emplace_back()` constructs in-place.