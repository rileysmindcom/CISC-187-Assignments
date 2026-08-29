# Assignment W1-2/2 Arrays
## 1. My Lab Objective
My lab objective is understanding how to use arrays in data structures C++. To create a process of 100 element arrays. As a result for the program to determine the size of the elements that contain the array operations based on how the array's placement in memory location.
## 2. My Assignment W1-2/2 Arrays Code
```
#include <iostream>
using namespace std;

int main()
{
    int numbers[100];

    cout << "Task 1: Array created with 100 elements." << endl;

    for (int i = 0; i < 100; i++)
    {
        numbers[i] = i + 1;
    }

    cout << "First element: " << numbers[0] << endl;
    cout << "Last element: " << numbers[99] << endl;

    cout << "\nTask 2: Size of each element" << endl;
    cout << "Size of one element: " << sizeof(numbers[0])
         << " bytes" << endl;

    cout << "Total size of array: " << sizeof(numbers)
         << " bytes" << endl;

    cout << "\nTask 3: Theoretical number of steps" << endl;

    cout << "Reading: 1 step" << endl;
    cout << "Searching for a value not contained in the array: 100 steps" << endl;
    cout << "Insertion at the beginning: 100 steps" << endl;
    cout << "Insertion at the end: 1 step" << endl;
    cout << "Deletion at the beginning: 100 steps" << endl;
    cout << "Deletion at the end: 1 step" << endl;

    string fruits[100];

    for (int i = 0; i < 100; i++)
    {
        fruits[i] = "orange";
    }

    fruits[10] = "apple";
    fruits[25] = "apple";
    fruits[50] = "apple";
    fruits[75] = "apple";

    int appleCount = 0;

    for (int i = 0; i < 100; i++)
    {
        if (fruits[i] == "apple")
        {
            appleCount++;
        }
    }

    cout << "\nTask 4: Finding all occurrences of apple" << endl;
    cout << "Number of apples found: " << appleCount << endl;
    cout << "Number of steps: N steps" << endl;

    cout << "\nTask 5: Memory address of the array" << endl;

    cout << "Address of the first element: "
         << &numbers[0] << endl;

    cout << "Address of the array: "
         << numbers << endl;

    return 0;
}
```

## 3. Example Steps
### Example 1: Creating an Array of 100 Elements
We first create an integer array that contains 100 elements
'''
int numbers [100];
'''
These indexes range between 0 to 99 because C++ arrays use zero-based indexing

As example:

numbers[0] = 1;
numbers[99] = 100;

### Example 2: Size of Each Element
Using the sizeof() can be used to see how much memory one element occupies

sizeof(numbers[0])

To an int in the system where the program is compiled. This is commonly would be at 4 bytes, and the exact size of an int is implementation dependent

The size total of the array can be found using:

sizeof(numbers)

Using an array with 100 integer. Each integer is 4 bytes:
Which, means

10 x 4 = 400 bytes

### Example 3: What are the Steps to Array Operations?
An array contains 100 elements:
1. The program reads each element as an index
2. We search for a value that is not contained inside the array. Each element must be checked.
3. The insertion begins. This is when existing elements need to be shifted to make room.
4. Insertion is at the end. When a new element can be placed at the end. If that space is available
5. Deletion at the beginning. Remaining elements must shift to the left.
6. Then deletion at the end. The last element can be removed without shifting others.

### Example 4: Finding Every "Apple"
To find every Apple, the program needs to check every element in the array.
This seen using the Lines:

for (int i = 0; i < 100; i++)
{
    if (fruits[i] == "apple")
    {
        appleCount++;
    }
}

If the first element is apple, then the program would need to examine the rest of the array to determine
The array size of N to find every occurrence:
N steps --> O(N)

### Example 5: How do we Find Memory Address of an Apple?
We can allow C++ to find the memory address to an array by using its name.

cout << numbers << endl;

The name of an array represents the address of the first element
Meaning we can obtain the address of the first element
To use the location of the first element of the array.

cout << &numbers[0] << endl;

## 4. W1-2/2 Arrays Flowchart:
<img width="712" height="262" alt="W1-2_2 Arrays" src="https://github.com/user-attachments/assets/4e64238e-25f4-4d1b-aab6-153d84eb083b" />


## Challenges I've Experienced

One of the challenges I've faced was figuring out how to number the steps of changes to depend where an operation takes place for an array. If we decided insertion, or deletion to an element at the beginning requires other elements to be shifted.  Another was viewing the difference between the size of an array element, and the total size. Which, is why I used sizeof() to determine the amount of memory necessary by an individual element for the entire array. And, at last was the memory address of an array. To use numbers, and numbers[0] helped explain the beginning of an array that connects with the address of the first element.

## Here, is my Video Assignment. On W1-2/2 Arrays:


