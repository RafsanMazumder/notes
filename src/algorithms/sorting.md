# Sorting Algorithms

This document covers common sorting algorithms, their implementations, time and space complexities, and practical use cases.

## Bubble Sort

A simple comparison-based algorithm that repeatedly steps through the list, compares adjacent elements, and swaps them if they're in the wrong order.

```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    bool swapped;
    
    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        
        // If no swapping occurred in this pass, array is sorted
        if (!swapped) {
            break;
        }
    }
}
```

**Time Complexity:**
- Best: O(n) when array is already sorted
- Average: O(n²)
- Worst: O(n²)

**Space Complexity:** O(1)

**Advantages:**
- Simple to understand and implement
- Works well for small datasets
- Stable sort (doesn't change relative order of equal elements)

**Disadvantages:**
- Inefficient for large datasets
- Poor performance compared to other algorithms

## Selection Sort

Repeatedly selects the smallest (or largest) element from the unsorted portion and puts it at the beginning (or end).

```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n - 1; i++) {
        int min_idx = i;
        
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }
        
        // Swap the found minimum element with the element at index i
        if (min_idx != i) {
            swap(arr[i], arr[min_idx]);
        }
    }
}
```

**Time Complexity:**
- Best: O(n²)
- Average: O(n²)
- Worst: O(n²)

**Space Complexity:** O(1)

**Advantages:**
- Simple implementation
- Performs well on small datasets
- Minimizes the number of swaps (O(n) swaps)

**Disadvantages:**
- Inefficient for large datasets
- Not stable

## Insertion Sort

Builds the sorted array one element at a time by repeatedly taking the next element and inserting it into the already-sorted part of the array.

```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        // Move elements greater than key to one position ahead
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}
```

**Time Complexity:**
- Best: O(n) when array is already sorted
- Average: O(n²)
- Worst: O(n²)

**Space Complexity:** O(1)

**Advantages:**
- Simple implementation
- Efficient for small datasets
- Stable sort
- Adaptive (efficient for partially sorted arrays)
- Works well for arrays that are almost sorted
- Online algorithm (can sort as data arrives)

**Disadvantages:**
- Inefficient for large datasets

## Merge Sort

A divide-and-conquer algorithm that divides the input array into two halves, recursively sorts them, and then merges the sorted halves.

```cpp
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temporary arrays
    vector<int> L(n1), R(n2);
    
    // Copy data to temporary arrays
    for (int i = 0; i < n1; i++) {
        L[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        R[j] = arr[mid + 1 + j];
    }
    
    // Merge the temporary arrays back into arr[left..right]
    int i = 0, j = 0, k = left;
    
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    // Copy the remaining elements of L[], if any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    
    // Copy the remaining elements of R[], if any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        
        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
}

// Wrapper function
void mergeSort(vector<int>& arr) {
    mergeSort(arr, 0, arr.size() - 1);
}
```

**Time Complexity:**
- Best: O(n log n)
- Average: O(n log n)
- Worst: O(n log n)

**Space Complexity:** O(n)

**Advantages:**
- Guaranteed O(n log n) performance
- Stable sort
- Works well for linked lists
- External sorting (when data doesn't fit in memory)

**Disadvantages:**
- Requires extra space
- Slower for small datasets compared to insertion sort
- Not in-place (although there are in-place variations)

## Quick Sort

Another divide-and-conquer algorithm that selects a 'pivot' element and partitions the array around the pivot.

```cpp
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];  // Choose the rightmost element as pivot
    int i = low - 1;  // Index of smaller element
    
    for (int j = low; j < high; j++) {
        // If current element is smaller than the pivot
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    
    // Place pivot in its correct position
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        // Partition the array
        int pi = partition(arr, low, high);
        
        // Sort elements before and after partition
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// Wrapper function
void quickSort(vector<int>& arr) {
    quickSort(arr, 0, arr.size() - 1);
}
```

**Time Complexity:**
- Best: O(n log n)
- Average: O(n log n)
- Worst: O(n²) when array is already sorted and pivot is always the smallest/largest element

**Space Complexity:** O(log n) for the recursive call stack

**Advantages:**
- Typically faster in practice than other O(n log n) algorithms
- In-place sorting
- Cache-friendly
- Tail-recursive, which can be optimized

**Disadvantages:**
- Worst-case O(n²) performance
- Not stable
- Poor pivot selection can lead to inefficient sorting

### Randomized Quick Sort

To avoid worst-case scenarios, we can choose a random pivot:

```cpp
int randomPartition(vector<int>& arr, int low, int high) {
    // Generate a random index between low and high
    int random = low + rand() % (high - low + 1);
    
    // Swap the random element with the last element
    swap(arr[random], arr[high]);
    
    // Now use the standard partition method
    return partition(arr, low, high);
}

void randomizedQuickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = randomPartition(arr, low, high);
        randomizedQuickSort(arr, low, pi - 1);
        randomizedQuickSort(arr, pi + 1, high);
    }
}
```