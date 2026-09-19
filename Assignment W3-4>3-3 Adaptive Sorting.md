# Assignment W3-4>3/3 Adaptive Sorting

## Lab Objective
The goal of this lab is comparing the Selection Sort with Insertion Sort to determine which sorting algorithm is effective based on the data’s original order. Before sorting we first examine neighboring pairs so the program can define the array of fifty integers. The program can categorize the arrays as Best, Nearly Sorted, Average, and Partially Sorted or Worst Highly Reverse-Ordered based on the quantity of  the sorted data. The program then selects between both Insertion, or Selection Sort. Observing why the sorting algorithm requires O(N^2) time, and input analysis that takes O(N) time.

## Assignment Code W3-4>3/3 Adaptive sorting 
```c++
#include <iostream>
using namespace std;

void selectionSort(int array[], int size)
{
    for (int i = 0; i < size - 1; i++)
    {
        int minIndex = i;

        for (int j = i + 1; j < size; j++)
        {
            if (array[j] < array[minIndex])
            {
                minIndex = j;
            }
        }

        int temp = array[i];
        array[i] = array[minIndex];
        array[minIndex] = temp;
    }
}

void insertionSort(int array[], int size)
{
    for (int i = 1; i < size; i++)
    {
        int key = array[i];
        int j = i - 1;

        while (j >= 0 && array[j] > key)
        {
            array[j + 1] = array[j];
            j--;
        }

        array[j + 1] = key;
    }
}

int countOrderedPairs(int array[], int size)
{
    int orderedPairs = 0;

    for (int i = 0; i < size - 1; i++)
    {
        if (array[i] <= array[i + 1])
        {
            orderedPairs++;
        }
    }

    return orderedPairs;
}

string classifyArray(int orderedPairs, int size)
{
    int totalPairs = size - 1;

    double percentage =
        (static_cast<double>(orderedPairs) / totalPairs) * 100;

    if (percentage >= 90)
    {
        return "Best/Nearly Sorted";
    }
    else if (percentage >= 40)
    {
        return "Average/Partially Ordered";
    }
    else
    {
        return "Worst/Highly Reverse-Ordered";
    }
}

void printArray(int array[], int size)
{
    for (int i = 0; i < size; i++)
    {
        cout << array[i] << " ";
    }

    cout << endl;
}

int main()
{
    const int SIZE = 50;

    int array[SIZE] =
    {
        1, 2, 3, 4, 5,
        7, 6, 8, 9, 10,
        11, 12, 13, 15, 14,
        16, 17, 18, 19, 20,
        21, 22, 24, 23, 25,
        26, 27, 28, 29, 30,
        31, 33, 32, 34, 35,
        36, 37, 38, 40, 39,
        41, 42, 43, 44, 45,
        46, 47, 48, 49, 50
    };

    cout << "Array before sorting:" << endl;
    printArray(array, SIZE);

    int orderedPairs = countOrderedPairs(array, SIZE);

    string classification =
        classifyArray(orderedPairs, SIZE);

    cout << "\nInput classification: "
        << classification << endl;

    if (classification == "Worst/Highly Reverse-Ordered")
    {
        cout << "Sorting algorithm selected: Selection Sort"
            << endl;

        selectionSort(array, SIZE);
    }
    else
    {
        cout << "Sorting algorithm selected: Insertion Sort"
            << endl;

        insertionSort(array, SIZE);
    }

    cout << "\nArray after sorting:" << endl;
    printArray(array, SIZE);

    return 0;
}
```

## W3-4>3/3 Analysis and Reflection

### Analysis
The program determines the original array’s original order using neighboring pairs. The array of N elements containing N-1 adjacent pairings. There are 49 adjacent pairs that look for an array of 50 integers.
These are my thresholds 45 to 49 pairs arranged together from best/nearly sorted.
Average/Partially Ordered: 20-44 neighboring pairs in order.
0-19 sorted adjacent pairs are the worst/most reverse-ordered
Then we can take these pairs in order using
Array[i] <= array[i +1]
This tells the program whether to choose Insertion sort for both Nearly Sorted, and Average Sorted arrays because there’s a benefit from partially sorted data.
Showing how the program defines Selection Sort for Worst/Highly Reverse-Ordered arrays.

### Complexity
We can use the categorization method since O(N) takes time checking each neighboring pair checked once.

### Selection Sort Uses:
O(N^2) as best case
O(N^2) as average case
O(N^2) as worst case

### Insertion Sort Uses:
O(N) as best case
O(N^2) as average case
O(N^2) as worst case
In this scenario adding O(N) can be classified before an O(N^2) sorting algorithm as it yields an O(N^2) as total because of the quadratic term leads.

## Reflection

### 1. What information can the program determine about the input without actually sorting it?
We can determine by looking at the neighboring pairs the algorithm  may examine the original data’s order. Without altering the array we can determine if it is strongly reverse-ordered, highly reverse-ordered, and nearly sorted.

### 2. What is the cost of performing this pre-analysis?
O(N) takes time. Which is why the cost of performing the pre-analysis is that the software only needs to look at each adjacent pair once.

### 3. Is adaptive sorting always preferable?
Not always. An extra analysis phase is added before sorting in adaptive sorting. The difference isn't as significant compared to a small dataset. However, adaptive sorting is helpful when the order of the input data alters overtime.

### 4. How does dataset size and input order affect the choice?
The amount of effort for a sorting algorithm increases with the size of the dataset. When the data is already almost sorted, Insertion Sort can be effective in less ideal circumstances since both algorithms can take O(N^2) time.

### 5. What are the limitations of Big-O analysis?
The limitations of Big-O analysis describe how an algorithm runs as time increases to the size of the input over time. The program focuses on constant factors, and the actual distribution of the input data such as factors, hardware, and implementation details are not fully taken into consideration.

## Flowchart
<img width="707" height="641" alt="W3-4_3_3 Adaptive Sorting" src="https://github.com/user-attachments/assets/06ec3d53-97c1-43e9-b1e6-f9735a44a339" />

## Challenges
My first challenge was understanding how to categorize the initial array without first sorting it. Then determining how much of the array was already in order. This helped me understand using neighboring pairs. Selecting the thresholds needed for three classes presented. Creating categories for the program to assess the percentage of ordered adjacent pairs. I had to ensure that the software was able to show the sorted array after sorting, and the original array before sorting. Making it easier to confirm that the adaptive algorithm is operating. And, the last was to clarify why adding the pre-analysis did not alter the worst-case Big-O complexity, which was important to compare between O(N) classification, and the O(N^2) sorting step.

## Assignment W3-4>3/3 Adaptive Sorting Video
https://drive.google.com/file/d/1GvfeH_be1aT26P58cd3wF9oQcDBD2gvo/view?usp=sharing
