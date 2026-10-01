# Assignment W6 > 2/2 Queues

## Lab Objective
The objective of this lab is to create a queue by looking at how effective First-In, First-Out(FIFO) processing is suported by a circular array. As an example we can describe the behavior of an FIFO, then recognize the front, and back of a line, trace queue operations manually, use an array to create a circular queue with a fixed capacity. Put enqueue(), front(), empty(), depqueue(), full(), and size() as practice. For circular wraparound we use modular arithmetic. To identify underflow, and overflow in the queue. We can describ the ineffectiency of elements moving during dequeue(). Examine how complicated queue procedures are used in the terms of time. And, at last use queues to solve real-world computing problems.
## Assignment Code
```cpp 
#include <iostream>
#include <stdexcept>

using namespace std;

class Queue {
private:
    static const int CAPACITY = 10;

    int data[CAPACITY];
    int frontIndex;
    int rearIndex;
    int count;

public:
    Queue();

    bool empty() const;
    bool full() const;
    int size() const;

    void enqueue(int value);
    int dequeue();
    int front() const;
};

Queue::Queue()
    : frontIndex(0), rearIndex(0), count(0) {
}

bool Queue::empty() const {
    return count == 0;
}

bool Queue::full() const {
    return count == CAPACITY;
}

int Queue::size() const {
    return count;
}

void Queue::enqueue(int value) {
    if (full()) {
        throw overflow_error("Queue overflow");
    }

    data[rearIndex] = value;
    rearIndex = (rearIndex + 1) % CAPACITY;
    ++count;
}

int Queue::dequeue() {
    if (empty()) {
        throw underflow_error("Queue underflow");
    }

    int value = data[frontIndex];
    frontIndex = (frontIndex + 1) % CAPACITY;
    --count;

    return value;
}

int Queue::front() const {
    if (empty()) {
        throw underflow_error("Queue underflow");
    }

    return data[frontIndex];
}

int main() {
    Queue q;

    cout << "Initially empty: " << boolalpha << q.empty() << endl;

    q.enqueue(10);
    q.enqueue(20);
    q.enqueue(30);

    cout << "Front: " << q.front() << endl;
    cout << "Size: " << q.size() << endl;

    cout << "Dequeue: " << q.dequeue() << endl;
    cout << "Dequeue: " << q.dequeue() << endl;

    q.enqueue(40);
    q.enqueue(50);
    q.enqueue(60);
    q.enqueue(70);

    cout << "Front after wraparound activity: " << q.front() << endl;
    cout << "Size: " << q.size() << endl;

    cout << "Dequeue: " << q.dequeue() << endl;
    cout << "Dequeue: " << q.dequeue() << endl;
    cout << "Dequeue: " << q.dequeue() << endl;
    cout << "Dequeue: " << q.dequeue() << endl;

    try {
        cout << "Attempting dequeue from empty queue..." << endl;
        q.dequeue();
    }
    catch (const underflow_error& e) {
        cout << e.what() << endl;
    }

    try {
        cout << "Filling queue..." << endl;

        for (int i = 1; i <= 10; ++i) {
            q.enqueue(i);
        }

        cout << "Attempting one additional enqueue..." << endl;
        q.enqueue(11);
    }
    catch (const overflow_error& e) {
        cout << e.what() << endl;
    }

    return 0;
}
```
## Analysis, and Reflection

### Example 1:
### Trace Queue Operations
We can start with an empty queue
If 30 is the last front element. 4 people make up the queue size. The remaining components would be eliminated in the following order:
```
30 -> 40 -> 50 -> 60
```
This is because the elements that entered the queue first are eliminated showing in FIFO. 10 was the first value removed because it was added before 20, 30, 40, 50, and 60. 20 was the next item removed from the queue after 10

### Example 2:
### Why Not Shift The Array?
A shifting solution could need to transfer around N-1 elements during a single dequeue() if a queue has N elements.

We can display this as a result of the complexity to one shifting dequeue is:
```
O(N)
```
This work shows the following if all N elements are removed, and the remaining elements are transferred after each removal:
```
O(N)^2
```
And, can occur because almost all N components may be moved by the initial removal around N-1 by the subsequent removal N02 by the subsequent removal.

This, would be better for advance frontIndex because it doesn't need to move any existing components. The front is simply represented by a different array position in the queue. As a result for dequeue() can function as:
```
O(1)
```
### Example 3, and 4 Circular Queue State:
A circular queue will maintain

frontIndex, rearIndex, and count

Because there are no elements stored, count == 0 can indicate an empty queue.

We can look at the number of stored elements that have surpassed the maximum capacity count == CAPACITY to represent a full queue.

In this architecture frontIndex == rearIndex can show an empty or full queue. This uncertainty is elemeninated by the extra count variable:

```
count == 0 --> empty count == CAPACITY --> full
```

Which, is a result that the queue does not need to rely mostly on the two indexes to identify its status.

### Example 5 enqueue()
The first enqueue() function determines if the queue is full. The value is stored at rearIndex. If it isn't full

Then,

```
rearIndex = (rearIndex + 1) % CAPACITY;
```
Reveals the rear position
The count is raised.
By Using 

```
++rearIndex;
```

And, can be insufficient because the index would eventually exceed or equal to the array's capacity. The index wraps back to zero becoming a result to the modulo operation.

As an example, take a five-person capacity:
rearIndex is 4:

```
(4 + 1) % 5 = 0
```
As a result, index 0 comes after index 4.

### Example: 6 dequeue()
The operation of dequeue() function determines if the queue is empty. When its empty throws:

Underflow of the queue

If not, the value at frontIndex is saved the front index is advanced , the count is decreased, and saved value is returned.

The front is advanced shown as:
```
frontIndex = (frontIndex + 1) % CAPACITY;
```
Because no elements need to be physically moved this makes it efficient instead of moving the remaining elements.

### Example 7: front() vs dequeue()
The oldest element is returned by the front() function without being deleted. And, returned by the dequeue() function, which removes from the logical queue

As an example,
If the line is:
```
10 -> 20 -> 30
```
We call 
front()

Returning
10, yet the line is still there

We call again..
The dequeue yields:
And, 10 the line turns into:
```
20 -> 30
```

### Example: 8 Circular Wraparound
One sequence could indicate wraparound using a capacity of 5
However, when the wraparound occurs as rearIndex shifts from 4 to 0
We can use the modulo calculation by
```
(4 + 1) % 5 = 0
```
Because the queue may cross the physical end of the array, and the physical order of the values in the array may njot match the logical order of the queue. Although, it is possible to keep using the array at the beginning without relocating the current elements because of the modular arithmetic.

### Example: 9 Logical Position vs. Physical Position
Given:
CAPACITY = 8
frontIndex = 6
count = 4

The physical index is:
```
(frontIndex + i) % CAPACITY
```
```
Logical Position  Calculation  Physical Index
0                 6 + 0) % 8             6

1                 (6 + 1) % 8            7

2                 (6 + 2) % 8            0

3                 (6 + 3) % 8            1
```

This shows how a logical queue can traverse the array's physical end. The Modular arithmetic returns to index 0 after physical index 7. It is not necessary to relocate existing elements.

### Example 10: Complete Circular Queue Testing
The software can evalue
a queue that is originally empty
Several enqueuing processes
FIFO eliminates
front(), size() and several dequeuing processes

Just as a wraparound in a circle

Reuse of jobs that are previously empty
Such as:

Underflow, and overflow of queue.

Then the overflow test tries to insert an eleventh value after filling all ten places. An empty queue is removed by the underflow test.



## Flowchart for enqueue() Process
<img width="692" height="282" alt="W6_2_2 Queues" src="https://github.com/user-attachments/assets/8dc83aa1-bc8a-41e7-9501-5bc2aae6c179" />


## Flowchart for dequeue() Process
<img width="612" height="306" alt="W6 _ 2_2 Queues dequeue()" src="https://github.com/user-attachments/assets/228a60e8-9774-44e7-a75c-9aed79175253" />


## Challenges
Some of the challenges I've experienced were how frontIndex, and rearIndex move independently. Because a circular queue enables indexes to travel throughout the array it may be initially simpler to visualize a queue as a typical row of data. To understand the modulo operator's purpose presesented as (index + 1) % CAPACITY. This allows the index to reach the final array position and then return to 0. To recognize how the queue can reuse previously held spots was important. Then realizing why elements do not need to be moved following the dequeue presented. The program execution advances the frontIndex instead of shifting all other values the operation becomes effective. Then I needed to test the overflow, and underflow circumstances because they explain how the queue must safeguard itself against adding more values than it can hold or remove values when it is empty. The lesson I learned from this was that the values from logical order in the queue is not always reflected in the array's physical structure. All together frontIndex, rearIndex, and count all allow the physical array positions to wrap around while maintaining track of the logical queue. 



