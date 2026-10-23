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

            // Start with the current node and compare only children that exist.
            if (left < count && heap[left] > heap[largerChild]) {
                largerChild = left;
            }

            if (right < count && heap[right] > heap[largerChild]) {
                largerChild = right;
            }

            // The current node is already >= its children.
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
## Analysis, and Reflection

### Step 1: Understanding a Max-Heap
A complete binary tree where each parent is greater than or equal to its offspring is called a max-heap.

### Step 2: Array Representation
We use a vector to calculate the parent, and child indices, and keep the heap items in level order.

### Step 3: Insertion
When rebuilding the max-heap situation the program utilizes trickle-up after inserting a value at the end of the vector.

### Step 4: Accessing the Maximum
The greatest value is accessible in O(1) time, to keep it at index zero.

### Step 5: Removing the Maximum
The program replaces the root with its last element to use trickle-down restoring the heap condition.

### Step 6: Handling Edge Cases
The program checks, and safely removes a single element looking for empty heaps.

### Step 7: Big-O Analysis
In the worst case reaching the maximum takes O(1) time, meaning insertion, and removal take O(log N) time.

### Step 8: Priority Queues
For priority queue applications, a max-heap effectively handles the highest-priority data first.

### Step 9: Reflection
This gave myself a chance to reflect how binary heaps preserve their structure to enhance program efficiency.

## Flowchart
<img width="672" height="456" alt="W9 1-1 Binary Heaps" src="https://github.com/user-attachments/assets/65a9824f-7267-44ff-8c84-c87af3224a99" />

## Challenges
Some of the challenges I've experienced were understanding how a parent, and child indices function in the program using array-based heap to implement trickle-up, and trickle-down while preserving the max-heap shown was a challenge. But, became easier as I recognized how to properly handle edge cases by testing empty heaps, single element heaps, and many swaps.

## Assignment Video Link
