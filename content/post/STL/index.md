---
title: "STL"
date: 2022-01-30T10:17:40+08:00
draft: true
---

# C++ STL

|  | queue | priority_queue | stack | vector |
| --- | --- | --- | --- | --- |
| insert back | N/A | N/A | N/A | push_back O(1) amortized |
| Insertion | push O(1) | push O(logN) | push O(1) | insert O(#inserted + #after) |
| Emplace | emplace O(1) | emplace O(logN) | emplace O(1) | emplace O(#after) |
| Deletion | pop O(1) | pop O(logN) | pop O(1) | erase O(#erased + #after) |
| Get Frist | front O(1) | top O(1) | N/A | front O(1) |
| Get Last | back O(1) | N/A | top O(1) | back O(1) |
| Is Empty | empty O(1) | empty O(1) | empty O(1) | empty O(1) |
| Size | size O(1) | size O(1) | size O(1) | size O(1) |
| Clear | N/A | N/A | N/A | clear O(N) |

|  | set | unordered_set |
| --- | --- | --- |
| Count | count O(logN) | count O(1) avg
O(N) worst |
| Insert | insert O(logN) | insert O(1) avg
O(N) worst
May rehash |
| Emplace | emplace O(logN) | emplace O(1) avg
O(N) worst
May rehash |
| Deletion (val) | erase O(logN) | erase O(1) avg
O(N) worst |
| Deletion (iterator) | erase O(1) amortized | erase O(1) |
| Deletion (range) | erase O(#erased) | erase O(#erased) |
| Find | find O(logN) | find O(1) avg
O(N) worst |
| Lower_bound | lower_bound O(logN) | N/A |
| Upper_bound | upper_bound O(logN) | N/A |
| Clear | clear O(N) | clear O(N) |

## deque

- **Implementation**: matains a double-ended queue of **chunks** of fixed size.
    
- provide a functionality similar to `vector`, but with efficient insertion and deletion of elements **also at the beginning** of the sequence.

## queue

- Default underlying container: `deque`, queue is not container, it just a `container adapter`.
- `push`: Insert a new element at the end of the queue. (**push_back** of deque)
- `emplace`: It can call the constructor by itself (more efficient). (**emplace_back** of deque)
- `pop`: Removes the next element in the queue. (**pop_front** of deque)
- `front`: Returns a **reference** to the next element in the queue. (**front** of deque)
- `back`: Returns a **reference** to the last element in the queue. (**back** of deque)
- `empty`: Returns whether the queue is empty. (**empty** of deque)
- `size`: Returns the number of elements in the queue. (**size** of deque)
    
    ```cpp
    class data {
    		int a, b;
    public:
    		data(int x, int y): a(x), b(y) {}
    };
    
    queue<data> q;
    q.push(data(1, 2));
    q.emplace(1, 2); // call constructor in place.
    ```
    

## priority_queue

- Default underlying container: `vector`
    - Requires indexing heavily. So choose the vector rather than the deque.
- `push`: Insert a new element in the priority_queue. (**push_back** of vector, **push_heap**)
    - `vector<int> arr`, `arr.push_back(3)`, `push_heap(arr.begin(), arr.end())`
- `emplace`: **emplace_back** of vector, **push_heap**
- `pop`: Remove element on top of the priority queue. (**pop_heap**, **pop_back** of vector)
    - `pop_heap(arr.begin(), arr.end())`, `arr.pop_back()`
    - **pop_heap**: swap the begin and end value, and makes the subrange [first, end-1) into a heap.
- `top`: Return a **constant reference** to the top element. (**front** of vector)
- `empty`: Returns whether the priority_queue is empty.
- `size`: Returns the number of elements in the priority_queue.

## stack

- Default underlying container: `deque`, stack is not container, just a `container adapter`.
    - deque can grow faster than a vector (which requires reallocation and copying).
    - stack will call push pop frequently, so choose the deque
    - Reference: **[Why does stack use deque by default?](https://stackoverflow.com/questions/102459/why-does-stdstack-use-stddeque-by-default)**
- `push`: Insert a new element at the top of the stack. (**push_back** of deque)
- `emplace`: It can call the constructor by itself (more efficient). (**emplace_back** of deque)
- `pop`: Removes the element on top of the stack. (**pop_back** of deque)
- `top`: Returns a reference to the top element in the stack. (**back** of deque)
- `empty`: Returns whether the stack is empty.
- `size`: Returns the number of elements in the stack.

## vector

- `back`: Returns a reference to the last element in the vector.
- `capacity`: Return size of allocated storage capacity.
- `emplace(iterator pos, element)`: Insert a new element at position.
- `emplace_back(element)`: Construct and insert element at the end.
- `push_back`: Insert element at the end.
- `insert`: Insert an element at position.
    - `insert(iterator pos, element)`:
    - `insert(iterator pos, iterator first, iterator last)`:
        - insert a range of element at the position.
- `erase`: remove from the vector either a single element or a range.
    - `erase(iterator pos)`: erase a single element from the position
    - `erase(iterator first, iterator last)`: erase a range of element [first, last)
- `pop_back`: Removes the last element in the vector
- `front`: Returns a reference to the first element in the vector
- `operator[]`: Returns a reference to the element at position n in the vector container.
- `begin`, `end`: iterator point to before the first and after the last.
- `rbegin`, `rend`: reverse iterator point to before the last and one element before the first.
    - `for (auto it=vector.rbegin(); it!=vector.rend(); ++it)`
- `reserve`: Request the vector capacity be at least enough to contain n elements
    - To prevent the frequent reallocation.
- `resize`: Resizes the container so that it contains n elements.
    - `resize(int n)`: If n is greater than current, call the default constructor.
    - `resize(int n, int val)`: If n is greater than current, the extended element is initialized with the provided value.

## set

- Sets are typically implemented as binary search tree to allow the ***direct iteration on subsets*** based on their ***order***.
    - Typically use the `red-black tree`.
    - Some folks over at Google actucally builts a B-tree based containers.
        - **Reference**: **[Does any set implementation not use a red-black tree?](https://stackoverflow.com/questions/26550276/does-any-stdset-implementation-not-use-a-red-black-tree)**
- `value_type` is the first template parameter. (range based for loop)
- `count`: 1 if the container contains an element, 0 otherwise
- `erase`: Removes from the set container
    - `erase(iterator position)`: **amortized O(1)**
    - `erase(val)`: **O(logN)**
    - `erase(iterator first, iterator last)`: **O(last-first)**
- `insert`: Insert the new elements to the set container
    - `insert(val)`: **O(logN)**
    - `insert(iterator first, iterator last)`: **O(#inserted * logN)**
- `find(val)`: search the container for an element equivalent to val.
    - if found: Return the iterator to the element
    - else: return the iterator set::end
- `lower_bound(val)`: Returns an iterator point to the first **element ≥ val**.
- `upper_bound(val)`: Returns an iterator point to the first **element > val**.
- Traversal example:
    
    ```cpp
    set<int> data;
    
    // range based traversal
    for (int num: data)
    		cout << data << endl;
    
    // Iterator traversal
    for (auto it=data.begin(); it!=data.end(); ++it)
    		cout << *it << endl;
    ```
    

## unordered_set

- Unordered sets are containers that store unique elements in **no particular order**.
    - Use the hash table with **bucket** as the implementation.
    - When the container is full may trigger **rehash**.
- `value_type` is the first template parameter. (range based for loop)
- `count(k)`: 1 if an element with a value equivalent to k is found, or zero otherwise.
- `erase`: Removes from the unordered_set container.
    - `erase(iterator position)`: **O(1)**
    - `erase(val)`: **O(1) for avg, O(N) for worst**
    - `erase(iterator first, iterator last)`: **O(#erased)**
- `insert`: Inserts new elements in the unordered_set
    - `insert(val)`: **O(1)** for avg, **O(N)** for worst
    - `insert(iterator first, iterator last)`: **O(inserted)** for avg, **O(N*(size+1))** for worst
- `find(k)`: Searches the container for an element with k as value and returns an iterator.
    - **O(1)** for constant, **O(N)** for worst
- Traversal example
    
    ```cpp
    unordered_set<int> data;
    
    // range based traversal
    for (int num: data)
    		cout << data << endl;
    
    // Iterator traversal
    for (auto it=data.begin(); it!=data.end(); ++it)
    		cout << *it << endl;
    ```
    

## map

- Maps are associative containers that store elements formed by a combination of a **key value** and a **mapped value**, following a **specific order**.
- `value_type` is pair<const key_type, mapped_type> (range-based for loop)
- `operator[k]`:
    - If k matches the key of an element in the container, the function **returns a reference to its mapped value**.
    - If k does not match the key of any element in the container, the function inserts a new element with that key (the **element is constructed using its default constructor**). and returns a reference to its mapped value.
- Traversal example
    
    ```cpp
    map<string, int> data;
    
    // range based traversal, (copy by value)
    for (auto mapPair: data) // for (pair<const string, int> mapPair: data)
    		printf("%s, %d\n", mapPair.first, mapPair.second);
    
    // range based traversal (Use refernce directly)
    for (auto &mapPair: data) // for (pair<const string, int> &mapPair: data)
    		printf("%s, %d\n", mapPair.first, mapPair.second);
    
    // Iterator traversal
    for (auto it=data.begin(); it!=data.end(); ++it)
    		printf("%s, %d\n", it->first, it->second);
    ```
    

## unordered_map

- Unordered maps are associative containers that store elements formed by the combination of a key value and a mapped value, and which allows for **fast retrieval of indiviaudl elements based on their keys**.
- `value_type` is pair<const key_type, mapped_type> (range-based for loop)
- `operator[k]`:
    - If k matches the key of an element in the container, the function **returns a reference to its mapped value**.
    - If k does not match the key of any element in the container, the function inserts a new element with that key (the **element is constructed using its default constructor**). and returns a reference to its mapped value.
- Traversal example
    
    ```cpp
    unordered_map<string, int> data;
    
    // range based traversal, (copy by value)
    for (auto mapPair: data) // for (pair<const string, int> mapPair: data)
    		printf("%s, %d\n", mapPair.first, mapPair.second);
    
    // range based traversal (Use refernce directly)
    for (auto &mapPair: data) // for (pair<const string, int> &mapPair: data)
    		printf("%s, %d\n", mapPair.first, mapPair.second);
    
    // Iterator traversal
    for (auto it=data.begin(); it!=data.end(); ++it)
    		printf("%s, %d\n", it->first, it->second);
    ```
    

## list

- `Get Reference`:
    - **front**(), **back**()
- `Get Iterator`:
    - **begin**(), **end**()
- `Deletion`:
    - **erase**(iterator position)
    - **erase**(iterator first, iterator last)
    - Iterators, pointers and references referring to elements removed by the function are invalidated.
    - **All other iterators**, pointers and reference **keep their validity**.
- `Insertion`:
    - **insert**(iterator position, value)
    - **insert**(iterator position, size_type n, val)
    - **insert**(iterator position, iterator first, iterator last)
- `pop_back`(), `pop_front`(), `push_back`(), `push_front`()
- `reverse`