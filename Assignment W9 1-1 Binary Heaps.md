# Assignment W9 1-1 Binary Heaps

## Lab Objective
The objective of this lab is to understand how binary max-heaps work in C++. Then I discovered how to use trickle-up to add values, and trickle-down to remove the maximum value. Recognizing how heaps facilitate priority queues, and how to use Big-O notation to assess its efficiency.

## Assignment Code
```cpp
#include <iostream>
#include <vector>
#include <stdexcept>
#include <utility>

using namespace std;

class MaxHeap {
private:
    vector<int> heap;

    int parentIndex(int index) const {
        return (index - 1) / 2;
    }

    int leftChildIndex(int index) const {
        return 2 * index + 1;
    }

    int rightChildIndex(int index) const {
        return 2 * index + 2;
    }

    void trickleUp(int index) {
        while (index > 0) {
            int parent = parentIndex(index);

            if (heap[index] <= heap[parent]) {
                break;
            }

            swap(heap[index], heap[parent]);
            index = parent;
        }
    }

    void trickleDown(int index) {
        int count = static_cast<int>(heap.size());

        while (true) {
            int left = leftChildIndex(index);
            int right = rightChildIndex(index);
            int largerChild = index;

            if (left < count && heap[left] > heap[largerChild]) {
                largerChild = left;
            }

            if (right < count && heap[right] > heap[largerChild]) {
                largerChild = right;
            }

            if (largerChild == index) {
                break;
            }

            swap(heap[index], heap[largerChild]);
            index = largerChild;
        }
    }

public:
    bool empty() const {
        return heap.empty();
    }

    int size() const {
        return static_cast<int>(heap.size());
    }

    int maximum() const {
        if (empty()) {
            throw out_of_range("Cannot read the maximum of an empty heap.");
        }

        return heap[0];
    }

    void insert(int value) {
        heap.push_back(value);
        trickleUp(size() - 1);
    }

    bool removeMax() {
        if (empty()) {
            return false;
        }

        if (size() == 1) {
            heap.pop_back();
            return true;
        }

        heap[0] = heap.back();
        heap.pop_back();
        trickleDown(0);
        return true;
    }

    void display() const {
        cout << "[";
        for (size_t i = 0; i < heap.size(); ++i) {
            cout << heap[i];
            if (i + 1 < heap.size()) {
                cout << ", ";
            }
        }
        cout << "]\n";
    }
};

int main() {
    MaxHeap heap;

    cout << "Binary Max-Heap Demonstration\n";
    cout << "-----------------------------\n";

    cout << "\n1. Initially empty heap:\n";
    cout << "Is empty? " << (heap.empty() ? "Yes" : "No") << '\n';
    cout << "Size: " << heap.size() << '\n';
    heap.display();

    cout << "\n2. Insert 90 into the empty heap:\n";
    heap.insert(90);
    heap.display();

    cout << "\n3. Insert additional values: 70, 80, 20, 40, 60, 50:\n";
    for (int value : {70, 80, 20, 40, 60, 50}) {
        heap.insert(value);
        cout << "After inserting " << value << ": ";
        heap.display();
    }

    cout << "\n4. Insert 85 (demonstrates multiple trickle-up swaps):\n";
    heap.insert(85);
    heap.display();

    cout << "\n5. Read the maximum:\n";
    cout << "Maximum value: " << heap.maximum() << '\n';

    cout << "\n6. Remove the maximum:\n";
    if (heap.removeMax()) {
        cout << "Maximum removed. Heap: ";
        heap.display();
    }
    else {
        cout << "Cannot remove from an empty heap.\n";
    }

    cout << "\n7. Demonstrate multiple trickle-down swaps:\n";
    MaxHeap deletionTest;
    for (int value : {100, 88, 75, 60, 70, 40, 50, 20, 30}) {
        deletionTest.insert(value);
    }
    cout << "Before removal: ";
    deletionTest.display();
    deletionTest.removeMax();
    cout << "After removing maximum: ";
    deletionTest.display();

    cout << "\n8. Remove from a one-element heap:\n";
    MaxHeap oneElement;
    oneElement.insert(42);
    cout << "Before removal: ";
    oneElement.display();
    cout << "Removal successful? "
        << (oneElement.removeMax() ? "Yes" : "No") << '\n';
    cout << "After removal: ";
    oneElement.display();

    cout << "\n9. Attempt to remove from an empty heap:\n";
    cout << "Removal successful? "
        << (oneElement.removeMax() ? "Yes" : "No") << '\n';

    cout << "\n10. Empty-heap maximum handling:\n";
    try {
        cout << oneElement.maximum() << '\n';
    }
    catch (const out_of_range& error) {
        cout << error.what() << '\n';
    }

    cout << "\n11. Final summary:\n";
    cout << "Main heap size: " << heap.size() << '\n';
    cout << "Main heap contents: ";
    heap.display();

    return 0;
}
```
## Testing, and Process 

### Step 1: Is It a Valid Max-Heap?
Each tree is completed with a parent greater than or equal to its offspring, Heap A becomes a valid max-heap. While Heap B lacks a child position before another position is filled, making it an invalid max-heap because the value of 95 is higher than its parent 90, Heap C is invalid. This explains that a max-heap needs to meet both heap order- and a fully completed tree.

### Step 2: Heap vs. Binary Search Tree
Every parent must be greater than or equal to its children in a max-heap. To maintain the greatest value at the root. A heap doesn't arrange all of its values especially during a left-to-right order, when comparing to Binary Search Tree. Such as its worst-case search time is O(N) because searching for an arbitrary value as 40, can be necessary to checking several nodes. But, because the maximum value is kept at its root, retrieving would require O(1) time.

### Step 3: Represent a Heap Using an Array
I learned that the level of ordering the traversal can be used for storing a binary heap in a vector. So each level of the values can be stored from left to right, starting at the root.
*By Using This Chart:*
```
Index:       Value:
0             90
1             70
2             80
3             20
4             40
5             60
6             50

```
Based on this result helped me determine by the node's placement from using the parent, and child formulas in the program. Mostly because when a child index is smaller than the heap's size becomes valid.

### Step 4: Why Use an Array?
We use an array to look at the last node is placed at the last array index by using an array to find it. So then the program can utilize trickle-up to restore the max-heap property after appending this value at the end. Then the program needs to eliminate the final array position by substituting the last element for the root when eliminating the maximum. We can do this by looking at the entire tree structure preserved without having to find the final node.

### Step 5: Implement a MaxHeap Class
We can create a *MaxHeap* class to store the heap values inside a vector. To look at the operations that enter data, accessing the maximum, removing the maximum, and then displaying the heap so we can take these functions for computing the parent, and child indices. Then I preserved the max-heap property by following insertion, and removal to employ trickle-up, and trickle-down.

### Step 6: Implement the Index Functions
The root is kept at index 0. So it lacks a parent. Then I can check if a child's computed index is less than the size of the heap making sure that it exists. These calculations function because the program can determine the parent, and child positions without the need for linked nodes as the entire binary tree is kept in level order.

### Step 7: Heap Insertion
Then I decided to experiment with adding 85 into another chart by adding a new value positioned at index 7. So we can compare with its parent at 20. Then the value 85 reaches index 1 after we switch with 20, and then 70. However, this stops because its parent 90 is greater.

```
Current Index    Parent Index    Current Value    Parent Value    Action
7                    3                85               20      Swap 85 and 20.
3                    1                85               70      Swap 85 and 70.
1                    0                85               90          Stop.
```
This helped me learn how to use trickle-up to look at the tree whether its complete. By restoring the *heap property* through testing.

### Step 8: Accessing the Maximum
Because it takes O(1) time for reaching the maximum value since its stored at index 0. The program has to restore the heap after replacing its root. From removing the maximum required O(log N) time. Especially when the maximum() is invoked without any elements. This method helped me handle an empty heap by raising an *out_of_range exception*.

### Step 9: Removing the Root
I use an elimination process to determine whether the heap was empty or only had one element. The last value is moved to the root, and the last array element is removed. If the heap has more than one element. The max-heap is then restored by the application by re-calling trickle-down

### Step 10: Trickle-Down
I can test by using a chart to experiment taking 100 out of the heap. So we can compare the final element, and 30 with its result by substituting it for the root. The result node would have no children. Then the program stops after swapping 88, and then 70.

```
Current Index          Left Child           Right Child        Larger Child           Action
0                    88 at index 1          75 at index 2       88 at index 1    Swap 30 and 88.
1                    60 at index 3           70 at index 4      70 at index 4    Swap 30 and 70.
4                        No child               No child            None                Stop.
```
This shows how we use the trickle-down method to restore the max-heap property as a result of removing the root.

### Step 11: Why the Larger Child?
When trickling a value downhill, the larger child must be chosen in the program. So the other child can grow larger than its parent, and break the max-heap for the program to chose the smaller child instead. To maintain the proper ordering across the heap as an aid by choosing the larger child.

### Step 12: One-Child Edge Case
I learned that the program can't always assume that each node would have two children. So this method can compare the current value with that child, and does any necessary swaps if a node has only one left child. The process finishes because it has reached a leaf if the node has no children, the algorithm stops so the invalid array can access checking if child indices exist.

### Step 13: Test the Complete Heap
I begin testing the functions maximum access, insertion, removal, and displaying the heap by using the *main() method*. So this test can remove the maximum that is necessary for numerous trickle-down swaps in this situation. To introduce values that require multiple trickle-up swaps. Ensuring that these edge cases are handled. I then include tests for an empty heap, and one-element heap.

### Step 14: The Complexity Analysis Chart
I learned that insertion, and removal are important because the height of a full binary heap is O(log N). We observe the root is accessible to reaching the maximum can take constant time. Because the heap doesn't completely sort these elements. Also, when searching for a value that is less effective.
Here, is my Analysis Chart:
```
Operation:                       Big-O:        Explanation:
Read maximum                     O(1)      We store the maximum at index 0.
Insert                        O(log N)    The height of the heap can be affect by a new value.
Remove maximum                O(log N)   We make a replacement value to descend through the heap.
Search for arbitrary value        O(N)     Each component needs to be examined by the program.
Access last array node           O(1) The last array index is used to access the last element.
```
### Step 15: Heap vs. Ordered Array
When the highest value is stored at the end. An ordered array removes it through O(1) time. This is necessary to relocate the other elements in order to insert values, which can make insertion at O(N). Because a queue can add, and remove tasks. So a binary heap can be helpful because it allows both insertion, and removing O(log N) time.

### Step 16: Queue Reasoning
The task of a queue must follow an order of:
```
A = 4, B = 9, C = 2, D = 7, and E = 10.
```
This is after we use insertion, and trickle-up for the array as a result:
```
[10, 9, 2, 4, 7]
```
Because the highest is 10, Take E must be completed. Then the final array would give us a result as Task E is eliminated, resulting Task B can be handled next:
```
[9, 7, 2, 4]
```
So the job stays at the root, and can be accessed through that example, which explains why a max-heap became helpful for queues.

## Analysis, and Reflection

### Step 1: Understanding a Max-Heap
A complete binary tree in which every parent is greater than or equal to its offspring is called a binary max-heap. So the larger value is kept at the bottom of the heap because of this structure.

### Step 2: Array Representation
Heap elements are stored in level order by the application using a vector. To explore the heap, parent, and child indices are computed using mathematical formulas.
```
Parent index = (i - 1) / 2
Left child   = 2 * i + 1
Right child  = 2 * i + 2
```

### Step 3: Insertion
The program adds a new value to the vector's end when it is inserted. Until the max-heap property is restored, the trickle-up process then compares a new value with its parent, and switches them.

### Step 4: Accessing the Maximum
The program receives the maximum value through O(1) time because it is stored at index zero. Which, eliminates the need to rearranging the heap, and increase the performance of accessing the highest-priority value.

### Step 5: Removing the Maximum
We can substitute the final vector element for the root as the software eliminates the maximum. Then it moves the replacement value downward such as trickle-down to restore the max-heap trait

### Step 6: Handling Edge Cases
Before the program checks whether to attempt removing the heap's maximum value, the program has to verify whether its empty. So we can safely manage one-element heap to prevent it accessing to an empty heap's limit by using an exception.

### Step 7: Big-O Analysis
Since the value is kept at the root. When we access the maximum requires O(1) time. Because values could move through the heap's height, insertion to remove, and take O(log N) time in its worst case.

### Step 8: Priority Queues
We can observe that the highest-priority value maintains at the root, a max-heap becomes helpful to creating a priority queue. This enables the highest-priority item to be processed through the program before lower-priority items.

### Step 9: Reflection
This gave myself a chance to learn how binary heaps facilitate insertion, and removes throughout the program during this experiment. To discover how to test different edge situations making sure the program functions using trickle-up, and trickle-down to restore the max-heap property.

## Flowchart
<img width="672" height="456" alt="W9 1-1 Binary Heaps" src="https://github.com/user-attachments/assets/65a9824f-7267-44ff-8c84-c87af3224a99" />

## Challenges
Some of the challenges I've experienced were understanding how a parent, and child indices function in the program using array-based heap to implement trickle-up, and trickle-down while preserving the max-heap shown was a challenge. But, became easier as I recognized how to properly handle edge cases by testing empty heaps, single element heaps, and many swaps.

## Assignment Video Link
