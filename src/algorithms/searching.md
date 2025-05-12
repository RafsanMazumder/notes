# Searching Algorithms

This document covers various searching algorithms, their implementations, time complexities, and applications.

## Linear Search

Simplest searching algorithm that checks each element in sequence.

```cpp
int linearSearch(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            return i;  // Return index if found
        }
    }
    return -1;  // Not found
}
```

**Time Complexity:**
- Average: O(n)
- Worst: O(n)
- Best: O(1) (if element is at the first position)

**Space Complexity:** O(1)

**Advantages:**
- Simple to implement
- Works on unsorted arrays
- Only option for linked lists when random access isn't available

**Disadvantages:**
- Inefficient for large datasets

## Binary Search

Divide-and-conquer algorithm that works on sorted arrays.

```cpp
int binarySearch(vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Avoid potential overflow
        
        if (arr[mid] == target) {
            return mid;  // Found the target
        } else if (arr[mid] < target) {
            left = mid + 1;  // Search in the right half
        } else {
            right = mid - 1;  // Search in the left half
        }
    }
    
    return -1;  // Target not found
}
```

**Time Complexity:**
- Average: O(log n)
- Worst: O(log n)
- Best: O(1) (if element is at the middle position)

**Space Complexity:** 
- Iterative: O(1)
- Recursive: O(log n) due to call stack

**Advantages:**
- Very efficient for large datasets
- Works well with arrays that support random access

**Disadvantages:**
- Requires sorted array
- Not suitable for data structures that don't allow random access

## Recursive Binary Search

```cpp
int binarySearchRecursive(vector<int>& arr, int target, int left, int right) {
    if (left > right) {
        return -1;  // Base case: target not found
    }
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) {
        return mid;  // Found the target
    } else if (arr[mid] < target) {
        return binarySearchRecursive(arr, target, mid + 1, right);  // Search right half
    } else {
        return binarySearchRecursive(arr, target, left, mid - 1);  // Search left half
    }
}

// Wrapper function
int binarySearchRecursive(vector<int>& arr, int target) {
    return binarySearchRecursive(arr, target, 0, arr.size() - 1);
}
```

## Finding Lower and Upper Bounds

Often used when elements can be repeated.

```cpp
// Find the first occurrence of target
int lowerBound(vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size();  // One past the last element
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return (left < arr.size() && arr[left] == target) ? left : -1;
}

// Find the last occurrence of target
int upperBound(vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size();  // One past the last element
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return (left > 0 && arr[left - 1] == target) ? left - 1 : -1;
}
```

## Binary Search on Answer

Used for optimization problems when we can check if a given value is feasible.

```cpp
// Example: Find the minimum capacity of ships to ship packages within D days
int shipWithinDays(vector<int>& weights, int days) {
    int left = *max_element(weights.begin(), weights.end());  // Minimum possible capacity
    int right = accumulate(weights.begin(), weights.end(), 0);  // Maximum possible capacity
    
    auto feasible = [&](int capacity) {
        int daysNeeded = 1;
        int currentLoad = 0;
        
        for (int weight : weights) {
            if (currentLoad + weight > capacity) {
                daysNeeded++;
                currentLoad = 0;
            }
            currentLoad += weight;
        }
        
        return daysNeeded <= days;
    };
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (feasible(mid)) {
            right = mid;  // Try a smaller capacity
        } else {
            left = mid + 1;  // Need a larger capacity
        }
    }
    
    return left;
}
```

## Jump Search

A middle ground between linear and binary search, especially useful for sorted arrays that are too large to fit in memory.

```cpp
int jumpSearch(vector<int>& arr, int target) {
    int n = arr.size();
    int step = sqrt(n);  // Optimal jump size
    
    // Find the block where the element might be present
    int prev = 0;
    while (arr[min(step, n) - 1] < target) {
        prev = step;
        step += sqrt(n);
        if (prev >= n) {
            return -1;  // Not found
        }
    }
    
    // Linear search in the identified block
    while (arr[prev] < target) {
        prev++;
        if (prev == min(step, n)) {
            return -1;  // Not found
        }
    }
    
    if (arr[prev] == target) {
        return prev;  // Found
    }
    
    return -1;  // Not found
}
```

**Time Complexity:** O(√n)

## Interpolation Search

An improvement over binary search for uniformly distributed sorted arrays.

```cpp
int interpolationSearch(vector<int>& arr, int target) {
    int left = 0;
    int right = arr.size() - 1;
    
    while (left <= right && target >= arr[left] && target <= arr[right]) {
        // Formula for probing position
        int pos = left + ((double)(right - left) / (arr[right] - arr[left])) * (target - arr[left]);
        
        if (arr[pos] == target) {
            return pos;  // Found
        }
        
        if (arr[pos] < target) {
            left = pos + 1;  // Search in right half
        } else {
            right = pos - 1;  // Search in left half
        }
    }
    
    return -1;  // Not found
}
```

**Time Complexity:**
- Average: O(log log n) for uniformly distributed data
- Worst: O(n) when data is not uniformly distributed

## Exponential Search

Useful for unbounded searches and better than binary search for bounded arrays when the element is near the beginning.

```cpp
int exponentialSearch(vector<int>& arr, int target) {
    int n = arr.size();
    
    // If target is at first position
    if (arr[0] == target) {
        return 0;
    }
    
    // Find range for binary search
    int i = 1;
    while (i < n && arr[i] <= target) {
        i *= 2;
    }
    
    // Perform binary search on the range [i/2, min(i, n-1)]
    return binarySearchRecursive(arr, target, i / 2, min(i, n - 1));
}
```

**Time Complexity:** O(log n)

## Fibonacci Search

Uses Fibonacci numbers to divide the array, useful when binary search's uniform division is not optimal.

```cpp
int fibonacciSearch(vector<int>& arr, int target) {
    int n = arr.size();
    
    // Initialize Fibonacci numbers
    int fibM2 = 0;  // (m-2)'th Fibonacci number
    int fibM1 = 1;  // (m-1)'th Fibonacci number
    int fibM = fibM1 + fibM2;  // m'th Fibonacci number
    
    // Find smallest Fibonacci number >= n
    while (fibM < n) {
        fibM2 = fibM1;
        fibM1 = fibM;
        fibM = fibM1 + fibM2;
    }
    
    // Marks the eliminated range from front
    int offset = -1;
    
    // Search in the array
    while (fibM > 1) {
        // Check if fibM2 is a valid index
        int i = min(offset + fibM2, n - 1);
        
        if (arr[i] < target) {
            fibM = fibM1;
            fibM1 = fibM2;
            fibM2 = fibM - fibM1;
            offset = i;
        } else if (arr[i] > target) {
            fibM = fibM2;
            fibM1 = fibM1 - fibM2;
            fibM2 = fibM - fibM1;
        } else {
            return i;  // Found
        }
    }
    
    // Check the last element
    if (fibM1 && arr[offset + 1] == target) {
        return offset + 1;
    }
    
    return -1;  // Not found
}
```

**Time Complexity:** O(log n)

## Searching in 2D Arrays

### Row-wise and Column-wise Sorted Matrix

```cpp
pair<int, int> searchMatrix(vector<vector<int>>& matrix, int target) {
    int rows = matrix.size();
    if (rows == 0) return {-1, -1};
    
    int cols = matrix[0].size();
    if (cols == 0) return {-1, -1};
    
    // Start from top-right corner
    int row = 0;
    int col = cols - 1;
    
    while (row < rows && col >= 0) {
        if (matrix[row][col] == target) {
            return {row, col};  // Found
        } else if (matrix[row][col] > target) {
            col--;  // Move left
        } else {
            row++;  // Move down
        }
    }
    
    return {-1, -1};  // Not found
}
```

**Time Complexity:** O(rows + cols)

## Applications

1. **Database Systems**: Indexing and retrieval operations
2. **Compiler Design**: Symbol table lookups
3. **Information Retrieval**: Searching for documents
4. **Machine Learning**: k-nearest neighbors algorithm
5. **Geographic Information Systems**: Spatial indexing

## Choosing the Right Search Algorithm

| Algorithm | When to Use |
|-----------|-------------|
| Linear Search | Small datasets, unsorted arrays, linked lists |
| Binary Search | Large sorted arrays, log-time requirement |
| Jump Search | Large sorted arrays that don't fit in memory |
| Interpolation Search | Uniformly distributed sorted arrays |
| Exponential Search | Unbounded arrays, element near beginning |
| Fibonacci Search | When division points need to be biased |

## Practice Problems

1. Find first and last occurrence of an element in a sorted array
2. Search in a rotated sorted array
3. Find the peak element in a bitonic array
4. Find the missing number in an array of 1 to n
5. Search in a nearly sorted array
6. Find the smallest letter greater than target