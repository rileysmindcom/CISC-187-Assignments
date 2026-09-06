# Assignment W2-1/1 Searching
## 1. Lab Objective
The goal of this lab is to understand how searching algorithms work, and how their changes as the size of dataset increase. In this lab I will analyze three searching algorithms.
Starting from Linear search, randomize search, and binary search. Then compare the number of element comparisons by each algorithm. Then understand why binary search requires data as linear search, and randomized search operate without any required data sorted.
And, at last is examining why the random selected elements does not make the algorithm more efficient versus linear search.

## 2. My W2-1/1 Assignment Code

```
#include <vector>
#include <random>
#include <iostream>

using namespace std;

int linearSearch(const vector<int>& data, int target, int& comparisons)
{
    comparisons = 0;

    for (int i = 0; i < data.size(); i++)
    {
        comparisons++;

        if (data[i] == target)
        {
            return i;
        }
    }

    return -1;
}

int binarySearch(const vector<int>& data, int target, int& comparisons)
{
    comparisons = 0;

    int left = 0;
    int right = data.size() - 1;

    while (left <= right)
    {
        int middle = left + (right - left) / 2;

        comparisons++;

        if (data[middle] == target)
        {
            return middle;
        }
        else if (data[middle] < target)
        {
            left = middle + 1;
        }
        else
        {
            right = middle - 1;
        }
    }

    return -1;
}

int randomizedSearch(const vector<int>& data, int target, int& comparisons)
{
    comparisons = 0;

    int n = data.size();

    vector<int> examined(n, 0);

    random_device rd;
    mt19937 generator(rd());
    uniform_int_distribution<int> distribution(0, n - 1);

    int examinedCount = 0;

    while (examinedCount < n)
    {
        int index = distribution(generator);

        if (examined[index] == 1)
        {
            continue;
        }

        examined[index] = 1;
        examinedCount++;

        comparisons++;

        if (data[index] == target)
        {
            return index;
        }
    }

    return -1;
}

int main()
{
    const int SIZE = 100000;

    vector<int> data(SIZE);

    for (int i = 0; i < SIZE; i++)
    {
        data[i] = i + 1;
    }

    int target;

    cout << "Enter a target value from 1 to 100000: ";
    cin >> target;

    int linearComparisons;
    int binaryComparisons;
    int randomizedComparisons;

    int linearResult =
        linearSearch(data, target, linearComparisons);

    int binaryResult =
        binarySearch(data, target, binaryComparisons);

    int randomizedResult =
        randomizedSearch(data, target, randomizedComparisons);

    cout << "\n--- Linear Search ---\n";

    if (linearResult != -1)
        cout << "Target found.\n";
    else
        cout << "Target not found.\n";

    cout << "Comparisons: "
         << linearComparisons << "\n";

    cout << "\n--- Binary Search ---\n";

    if (binaryResult != -1)
        cout << "Target found.\n";
    else
        cout << "Target not found.\n";

    cout << "Comparisons: "
         << binaryComparisons << "\n";

    cout << "\n--- Randomized Search ---\n";

    if (randomizedResult != -1)
        cout << "Target found.\n";
    else
        cout << "Target not found.\n";

    cout << "Comparisons: "
         << randomizedComparisons << "\n";

    return 0;
}
```
## 3. Examples

### 1. Example 1: Linear Search
If were given the array as:
[2, 4, 6, 8, 10, 12, 13]

The target is:
8

This is because Linear search starts from the first element. Checking each element left to right.
As example we can make
Four Comparisons:
1. Comparing 2 with 8 is not equal
2. Comparing 6 with 8 is not equal
3. Comparing 4 with 8 is not equal
4. Comparing 8, with 8 is Found.

This is because linear search requires 4 elements to make a comparison

### 2. Example 2: Binary Search
As we use
[2, 4, 6, 8, 10, 12, 13]

Because the target is 8
We can begin binary search in the middle of the sorted array

The middle element is:
8

If we compare:
 8 = 8

Meaning the target is found.
Which, is why binary search requires 1 element-to-target comparison

### Example 3: Binary Search With 100,000 Elements
Every comparison for a search space that's a binary search is cut in half.

Making it possible for the maximum number of elements to be calculated as

floor(log2(100000)) + 1

Because

log2(100000) = 16.61

However, we receive
floor(16.61) + 1 = 7

This is because the maximum number of elements for comparisons is
17 comparisons
Making binary search effective because of the remaining search space
that starts with:

100,000
50,000
25,000
12,500
6,250
3,125

Instead of constantly checking every element one at a time.

## Example 4: Linear Search vs. Binary Search

### Linear Search:
Is O(N), and needs up for 100,000 comparisons because it examines each element individually

### Binary Search
Is O(log N)
Sorted data is required to check the middle, and divide the search region in half each time.

The size of the binary search space decreases one element after each comparison for a linear search, and binary decreases around a half after each comparison.

## Example 5: Randomized Search
The search checks every comparison randomly selecting an unexamined element as a result in the remaining search space that is available.

And, can examine every element. From its worst-case complexity is O(N)

## 4. Flowchart
<img width="662" height="322" alt="Assignment W2-1_1" src="https://github.com/user-attachments/assets/40bebabb-e07f-43cb-967e-d89c96fe8dc7" />


## 5. Challenges
The first challenge was understanding the difference between binary, and linear search. This was because it was simple assuming both algorithms would look through the components until they located the goal. While, binary search operates differently because it eliminates around half of the remaining search space, and each comparison uses a sorted order of data. Another was ensuring the correction for count element-to-target comparisons. I needed to make sure that the comparison counter increased with an element was compared with the target instead of counting operations performed by the algorithm. Then I used randomized search because the algorithm randomly selects indices to make sure the same index is never examined twice from one search. And at last was ordering the elements. This is because the algorithm needs to examine a large portion dataset of 100,000 elements.

## My Assignment Video W2-1/1 Searching
https://drive.google.com/file/d/1uuBUHulPgh1GOkTWaxLfaUXYLoVyjiMu/view?usp=sharing
