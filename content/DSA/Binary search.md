A general template I told Gemini to generate, will fix if there is anything wrong.
```python
def binary_search(arr, target):
    """
    Performs a binary search on a sorted array.

    Args:
        arr: The sorted array (list or tuple).
        target: The value to search for.

    Returns:
        The index of the target if found, otherwise -1.
    """
    low = 0
    high = len(arr) - 1

    while low <= high:  # Crucial: <= to include single-element case
        mid = (low + high) // 2  # Integer division

        if arr[mid] == target:
            return mid  # Target found
        elif arr[mid] < target:
            low = mid + 1  # Search in the right half
        else:
            high = mid - 1  # Search in the left half

    return -1  # Target not found


# Variations:

def binary_search_leftmost(arr, target):
    """Finds the leftmost occurrence of the target."""
    low = 0
    high = len(arr) - 1
    result = -1  # Initialize result to -1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            result = mid  # Store the index and keep searching left
            high = mid - 1
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return result



def binary_search_rightmost(arr, target):
    """Finds the rightmost occurrence of the target."""
    low = 0
    high = len(arr) - 1
    result = -1

    while low <= high:
        mid = (low + high) // 2
        if arr[mid] == target:
            result = mid # Store the index and keep searching right
            low = mid + 1
        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    return result




def binary_search_closest(arr, target):
    """Finds the value in arr closest to target."""
    low = 0
    high = len(arr) - 1

    if target <= arr[low]: return arr[low]
    if target >= arr[high]: return arr[high]


    while low <= high:
        mid = (low + high) // 2

        if arr[mid] == target:
            return arr[mid]  # Exact match

        elif arr[mid] < target:
            low = mid + 1
        else:
            high = mid - 1

    # At this point, 'low' and 'high' are the closest indices
    if abs(arr[low] - target) < abs(arr[high] - target):
        return arr[low]
    else:
        return arr[high]



# Example Usage
arr = [2, 5, 7, 8, 11, 12]
target = 13

print(f"Index of {target}: {binary_search(arr, target)}")  # Output: -1 (not found)
print(f"Closest Value to {target}: {binary_search_closest(arr, target)}")
target = 12
print(f"Index of {target}: {binary_search(arr, target)}")


arr_with_duplicates = [2, 5, 7, 7, 7, 8, 11, 12]
target = 7
print(f"Leftmost index of {target}: {binary_search_leftmost(arr_with_duplicates, target)}")
print(f"Rightmost index of {target}: {binary_search_rightmost(arr_with_duplicates, target)}")
```


**Key points and explanations:**

* **Sorted Input:** Binary search *requires* a sorted array.
* **`low <= high`:**  The `<=` in the `while` loop condition is essential.  If you use just `<`, you'll miss the case where `low` and `high` converge on the target element.
* **Integer Division:**  `mid = (low + high) // 2` ensures integer division.
* **Updating `low` and `high`:** Carefully update `low = mid + 1` and `high = mid - 1` to avoid infinite loops.
* **Return -1:** Return -1 to indicate that the target was not found.
* **Variations:** The included variations demonstrate how to find the leftmost or rightmost occurrence of a target (useful when duplicates are present) or the closest element if the target isn't present.  These variations require careful adjustment of the `low` and `high` pointers and sometimes a separate variable to track the result.



This template and its variations provide a solid foundation for applying binary search to a variety of problems. Remember to adapt the comparison logic (`arr[mid] == target`, `arr[mid] < target`, etc.) as needed for your specific problem.