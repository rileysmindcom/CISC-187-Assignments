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
