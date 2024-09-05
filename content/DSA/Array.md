## Cracking Array Interview Questions: A Comprehensive Guide

This guide covers essential array concepts and strategies for acing those big tech interviews.

### I. Array Fundamentals

* Definition: A contiguous block of memory storing a collection of elements with the same data type.
* Key Properties:
    * Fixed Size: Declared with a specific size (cannot change during runtime).
    * Index-Based Access: Elements accessed using their index (0-based).
    * Contiguous Memory: Elements stored next to each other for efficient access.
* Declaration:
   ```java
   int[] numbers = new int[5]; // Java
   int numbers[5]; // C++
   numbers = [0] * 5 // Python (creates a list with 5 zeros, lists are dynamic arrays in Python)
   ```

### II. Essential Array Operations

* Traversal:
   ```java
   for (int i = 0; i < numbers.length; i++) {
       System.out.println(numbers[i]); 
   }
   ```
* Insertion:
   * At the end: Efficient for dynamic arrays.
   * At a specific index: Less efficient, requires shifting elements.
* Deletion: Similar efficiency considerations as insertion.
* Searching: Linear search, binary search.
* Sorting: Bubble sort, insertion sort, merge sort, quicksort.

### III. Common Array Interview Questions

1. Two Sum: (Sorted/unsorted array, target sum).
   * Solutions: Two pointers, hash table.
2. Best Time to Buy and Sell Stock: (Array representing stock prices).
   * Solution: Track min price and max profit in one pass.
3. Longest Consecutive Sequence: (Unsorted array).
   * Solution: Sorting or using a set/hash table.
4. Product of Array Except Self:
   * Solution: Two passes (left products, then right products).
5. Merge Sorted Arrays: (Two sorted arrays).
   * Solution: Two pointers, compare and place elements.

### IV. Array Concepts

* Two-Dimensional Arrays (Matrices): Arrays of arrays (grids, tables), just array pointing to other arrays
    * Operations: Traversal, searching, rotation.
* Dynamic Arrays (Resizable Arrays):
    * Examples: ArrayList (Java), Vector (C++), lists (Python)
    * Advantages: Flexible size, easier insertion/deletion.
    * Trade-offs: Occasional resizing (amortized O(1) complexity).
- Array of Objects: Store references to Objects so similar complexity for operations
### V. Big O Notation and Arrays

Big O notation describes how runtime or space usage grows with input size (n).

* Common Time Complexities:

| Operation         | Description                                | Time Complexity | Example                | Why O(1) or O(n)?                                                                                    |
| ----------------- | ------------------------------------------ | --------------- | ---------------------- | ---------------------------------------------------------------------------------------------------- |
| Accessing         | Get element at a specific index            | **O(1)**        | `my_list[3]`           | **Direct memory access:**  Index acts as an offset to calculate the element's exact memory location. |
| Searching (in)    | Check if an element is present             | O(n)            | `5 in my_list`         | Needs to potentially check all elements.                                                             |
| Searching (index) | Get index of an element (first occurrence) | O(n)            | `my_list.index(5)`     | Linear search required.                                                                              |
| Appending         | Add element to the end                     | O(1)*           | `my_list.append(10)`   | Amortized O(1) - usually constant, but occasional resizing can take O(n).                            |
| Inserting         | Insert at a specific index                 | O(n)            | `my_list.insert(2, 7)` | Shifting elements might be needed.                                                                   |
| Deleting (pop)    | Remove and return element at given index   | O(1) or O(n)**  | `my_list.pop(1)`       | O(1) from end, O(n) from middle (shifting).                                                          |
| Removing          | Remove first occurrence of a value         | O(n)            | `my_list.remove(5)`    | Linear search needed to find the element.                                                            |
| Length            | Get the number of elements                 | O(1)            | `len(my_list)`         | Python lists track their size internally.                                                            |
| Traversal         | Visit every element                        | O(n)            | `for item in my_list:` | Has to visit each element once.                                                                      |


\* **Amortized O(1):**  For operations like insertion and deletion at the end of dynamic arrays, the time complexity is *amortized* O(1). This means that while a single insertion/deletion *might* take O(n) time (if resizing occurs, about 1.5 or 2 times the original array), the average time complexity over many operations is still constant time (O(1)).
\** **O(1) or O(n):** `pop()` is O(1) when removing from the end, O(n) when removing from the middle (requires shifting elements).

* Space Complexity:
	- Creating an array: O(n) - Linear space growth.
	- In-place algorithms: O(1) - Modify the array directly without extra space.
### Side note:
- n = 1000 => <=O(n^2)
- n = 10000 => O(n^2), O(nlogn)
- n = 100000 => O(n), O(nlogn)
- n = 1000000 => O(n)
- n = 100 or 200 or 500 => O(n^3)

Prioritize time first, space second