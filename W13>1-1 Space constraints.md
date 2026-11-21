# Assignment W13 > 1/1 Space Constraints
## Lab Objective
The goal of my lab is to learn, and understand how to evaluate trade-offs between time complexity, and space complexity to the memory requirements. Then looking at the difference between space complexity, and time complexity. I begin using Big O notation, and auxiliary space. Which, determines the variables, and data structures used through memory usage. We Identify between algorithms that create extra storage for those that are in place. Then both time, and space complexity to compare algorithms. And, then analyze trade-offs between time, and space under system restrictions. To recognize the recursive hidden memory cost by examining the recursive function's maximum call-stack depth.

## Assignment Code W13 > 1/1 Space Constraints
```cpp
#include <algorithm>
#include <cctype>
#include <iostream>
#include <string>
#include <unordered_set>
#include <vector>

using namespace std;

vector<string> makeUppercaseCopy(const vector<string>& words) {
    vector<string> result;

    for (const string& word : words) {
        string upper = word;

        for (char& ch : upper) {
            ch = static_cast<char>(
                toupper(static_cast<unsigned char>(ch))
                );
        }

        result.push_back(upper);
    }

    return result;
}

void makeUppercaseInPlace(vector<string>& words) {
    for (string& word : words) {
        for (char& ch : word) {
            ch = static_cast<char>(
                toupper(static_cast<unsigned char>(ch))
                );
        }
    }
}

bool hasDuplicateNestedLoops(const vector<int>& values) {
    for (size_t i = 0; i < values.size(); ++i) {
        for (size_t j = i + 1; j < values.size(); ++j) {
            if (values[i] == values[j]) {
                return true;
            }
        }
    }

    return false;
}

bool hasDuplicateHashTable(const vector<int>& values) {
    unordered_set<int> seen;

    for (int value : values) {
        if (seen.count(value) > 0) {
            return true;
        }

        seen.insert(value);
    }

    return false;
}

bool hasDuplicateSortAndScan(vector<int>& values) {
    sort(values.begin(), values.end());

    for (size_t i = 0; i + 1 < values.size(); ++i) {
        if (values[i] == values[i + 1]) {
            return true;
        }
    }

    return false;
}

void countdown(int n) {
    if (n < 0) {
        return;
    }

    cout << n << '\n';
    countdown(n - 1);
}

void countdownLoop(int n) {
    while (n >= 0) {
        cout << n << '\n';
        --n;
    }
}

void printStrings(const vector<string>& words) {
    for (const string& word : words) {
        cout << word << '\n';
    }
}

void printIntegers(const vector<int>& values) {
    for (int value : values) {
        cout << value << ' ';
    }
    cout << '\n';
}

int main() {
    cout << "SPACE COMPLEXITY AND ALGORITHM TRADE-OFFS IN C++\n";

    cout << "\n--- Part 2: Uppercase Functions ---\n";

    const vector<string> originalWords = {
        "hello",
        "space complexity",
        "C plus plus"
    };

    vector<string> uppercaseCopy = makeUppercaseCopy(originalWords);

    cout << "Version 1 - uppercase copy:\n";
    printStrings(uppercaseCopy);

    vector<string> inPlaceWords = originalWords;
    makeUppercaseInPlace(inPlaceWords);

    cout << "\nVersion 2 - uppercase in place:\n";
    printStrings(inPlaceWords);

    cout << "\nOriginal words (unchanged):\n";
    printStrings(originalWords);

    cout << "\n--- Part 3: Nested-Loop Duplicate Detection ---\n";

    const vector<int> nestedValues = { 4, 8, 2, 8, 10 };

    cout << "Values: ";
    printIntegers(nestedValues);

    cout << "Contains duplicate: "
        << (hasDuplicateNestedLoops(nestedValues) ? "Yes" : "No")
        << '\n';

    cout << "\n--- Part 4: Hash-Table Duplicate Detection ---\n";

    const vector<int> hashValues = { 3, 6, 9, 12, 15 };

    cout << "Values: ";
    printIntegers(hashValues);

    cout << "Contains duplicate: "
        << (hasDuplicateHashTable(hashValues) ? "Yes" : "No")
        << '\n';

    cout << "\n--- Part 5: Sort-and-Scan Duplicate Detection ---\n";

    vector<int> sortValues = { 7, 2, 9, 4, 2 };

    cout << "Before sorting: ";
    printIntegers(sortValues);

    bool sortedHasDuplicate = hasDuplicateSortAndScan(sortValues);

    cout << "After sorting:  ";
    printIntegers(sortValues);

    cout << "Contains duplicate: "
        << (sortedHasDuplicate ? "Yes" : "No") << '\n';

    cout << "\n--- Part 8: Recursive Countdown ---\n";
    cout << "Countdown from 5:\n";
    countdown(5);

    cout << "\n--- Part 9: Iterative Countdown ---\n";
    cout << "Countdown from 5:\n";
    countdownLoop(5);

    cout << "\n--- Complexity Summary ---\n";
    cout << "Nested loops:       O(N^2) time, O(1) space\n";
    cout << "Hash table:         O(N) average time, O(N) space\n";
    cout << "Sort and scan:      O(N log N) time, O(log N) space\n";
    cout << "Recursive countdown: O(N) time, O(N) space\n";
    cout << "Iterative countdown: O(N) time, O(1) space\n";

    return 0;
}
```
## Background
### 1. What Does N Mean?
First we have to understand what N means. It is the size of the input that an algorithm is processing. As an example, N means the number of elements in an array. So it depends on the operation, and N shows us the total number of characters or number of strings in a vector of strings. So N starting number in a countdown algorithm.

### 2. How do we determine extra memory?
So I can check to see if the algorithm creates extra RAM when starting the program. These examples can consist of vectors or arrays, different data structures, such as a hash table. This includes any temporary items or duplicates. So stack memory can be used through the recursive function calls.

### 3. We then can determine any changes in additional Memory.
To discover whether extra memory grows or remains constant when N rises. As an example, O(1) the amount of extra memory will remain constant. O(N) means the amount of memory increasing through a direct proportion to the size of input.

### 4. What is the Complexity of Auxiliary Space?
We can determine Big O space complexity of extra memory after detecting it. As an example O(1) is the auxiliary space for nested loop duplicate detection. Then a Hash table duplication detection is the auxiliary space of O(N). And, a recursive countdown describes the recursive stack frames resulting in O(N) auxiliary space.

Then I used a flowchart to understand any additional memory that is required for an algorithm as the input size increases:

<img width="672" height="527" alt="Assignment W13 1-1 Space Constraints" src="https://github.com/user-attachments/assets/44ffb723-3124-4962-8c3f-1a863d6a4a7e" />


## Time Complexity vs. Space Complexity
```
|         Statement             |                    Time, Space, or Both?         |             Reasoning         |
|The number of operations grows linearly with N.             Time             The time complexity is O(N) because the number of operations will grow proportionally to the quantity of the input.

|The algorithm creates a second vector containing N elements. Space  Its auxiliary space complexity is O(N) because the extra vector stores N elements

| The algorithm performs nested loops but creates no additional data structure that grows with N.
                                                              Time     The number of operations will increase as O(N^2)  because of nested loops so it becomes unnecessary for O(N) to have more memory.

|A recursive function generates N simultaneously active stack frames at its deepest point.
                                                              Space    The auxiliary space complexity is O(N) because each active call needs a stack frame.
```

## Additional Storage vs. In-Place Modification
```
Question                                                                    Version 1                            Version 2
Does the algorithm create an additional container that grows with N?  Yes because it creates the result vector    No. because it modifies the original vector

Auxiliary-space complexity                                                 O(N)                               O(1) considers the extra container storage in the lab

Does the original input get modified?   No. because it keeps the converted copies after reading the original strings    Yes. because there's a change to the strings of the original vector
```

### 1. Why can Version 2 be described as an in-place approach?
Because version 2 modifies each string immediately within the original vector instead of creating a new vector to store the transformed strings through the in-place method. When doing this the extra container will grow with N is avoided.

### 2. Why does O(1) auxiliary space not mean “zero memory”?
Because O(1) means that when N grows the extra memory remains constant. So the function will still need memory for variables, references, program operations, and extra memory doesn't increase in direct proportion to the size of the input.

### 3. What practical disadvantage can arise from modifying the input in place?
The data originally is changed. Unless there's a second copy saved. So the original strings are no longer accessible to their original form if another section of the program would require them.

## Duplicate Detection: Version 1
### 1. What does N represent?
N represents the number of elements in values

### 2. Why is the time complexity O(N²) in the worst case?
The reason is two nested loops are present. So the inner loop can look through all N elements for any value of i. As a result the number of comparisons increases proportionally to N * N. So the worst-case temporal complexity is O(N)

### 3. What additional memory grows with N?
There's no additional hash table, array, or vector will expand N. Only a fixed number of loop variables, and values are used by the function.

### 4. What is the auxiliary-space complexity?
Its the amount of extra memory remaining constant as the vector expands, the auxiliary space complexity is O(1)

### 5. Why do nested loops affect time complexity but not automatically imply high space complexity?
Because the number of actions the algorithm will increase the nested loops. So they won't always produce more storage. This function has an O(N^2) time complexity, and an O(1) auxiliary space complexity because the program will read the original vector, and only utilize a fixed amount of variables.

## Duplicate Detection: Version 2
### 1. Why can this approach run in O(N) time?
Every input element is visited once by the algorithm. Hash-table lookup is used by the lines of seen.count(), and insertion using seen.insert() have O(1) average time under average-case model presented in this situation. Then the average time to process N elements is O(N).

### 2. In the worst case for memory usage, how many distinct values could be stored in seen?
If each element in the input vector is unique the set could have N different values

### 3. What is the auxiliary-space complexity?
We can observe the visible hash table that expands holding up to N values, and the auxiliary space complexity is O(N)

### 4. What memory is being exchanged for improved speed?
To remember the values the program has already visited, the algorithm will need to make use of extra RAM for the hash table. The average case execution time with nested loops will be reduced from O(N^2) to O(N) resulting in extra O(N) storage.

## Duplicate Detection: Version 3

### 1. Why does sorting make duplicate detection easier?
Because the equal values are arranged next to each other during the sorting process. The algorithm would need to compare any nearby elements to identifying the duplicate once the values have been stored.

### 2. Why does the final scan take only O(N) time?
The loop will compare each element with the next element as it iterates around the vector once. The scan takes O(N) time because it does a maximum of N comparisons.

### 3. Why does the overall time remain O(N log N)?
Because the scan takes O(N) while sorting that will take the time complexity of O(N log N). We can see sorting step will increase more quickly taking up most of the operating time.

### 4. Under the lecture's model, what is the auxiliary-space complexity?
The lab's sorting model the auxiliary space complexity is O(log N)

### 5. What happens to the original ordering of values?
The function will sort the supplied vector to change the original ordering. So the sorted order is applied to the values.

## Compare the Three Algorithms
```
Approach                Time Complexity            Auxiliary Space        Modifies Input?            Reasoning
Nested loops                O(N²)                       O(1)                    No            It requires only the fixed amount of extra memory when comparing components using stacked loops

Hash table            O(N) average case                O(N)                     No            We keep the values that have already been seen in a separate hash table

Sort + adjacent scan     O(N log N)       O(log N), based on lab's model        Yes       We examine any nearby values after sorting the original vector.
```

## Time-Space Trade-off Decisions
### Scenario A — Memory-Constrained Device
#### Choice: Nested loops.
I choose this method because of nested loops, and the device has very little extra RAM, and execution speed is not the primary concern. This technique wouldn't create a new data structure to increase with N, it utilizes O(1) auxiliary space. However the lab characterizes the data as moderate, and emphasizes memory looking back at O(N^2)

### Scenario B — Speed-Critical Application
#### Choice: Hash table.
I would choose this because the hash table method has an O(N) average case time. So the server would need to have proper memory for handling the O(N) auxiliary space it will utilize, and store the encountered values. The average running time is enhanced by the additional RAM.

### Scenario C — Balanced Constraints
#### Choice: Sort + adjacent scan.
I choose this because the O(log N) auxiliary space that's under the lectures model, and avoids O(N^2) running time for nested loops. I'll use this method for sorting the following adjacent scan. We see both memory, and execution time are important because of O(N log N) time, and O(log N) auxiliary space offer a balance together.

### Scenario D — Input Order Must Be Preserved
#### Choice: Hash table.
I choose this because the program will read the original vector, and save the previous values independently for the hash-table method. The input isn't sorted or rearranged. So it will have an average case time of O(N), and utilize the auxiliary space. However this will use O(N^2) time, and nested loops since O(1) needs to be consumed for the auxiliary space to maintain the input order. When maintaining order to avoid quadratic running time are both important to this option for the hash table used.

## The Hidden Space Cost of Recursion
```cpp
void countdown(int n) {
    if (n < 0) {
        return;
    }

    cout << n << "\n";
    countdown(n - 1);
}
```
We see that the function countdown(5) the recursive calls will occur in the values of:
```
5, 4, 3, 2, 1, 0, -1
```
This means that the base case is reached after we call n == -1. Meaning all seven calls are returned, and active.

```
Question                                                                                                 Your Analysis
What values of n appear in active recursive calls before unwinding begins?                              5, 4, 3, 2, 1, 0, and -1.

What is the approximate maximum number of simultaneously active stack frames as a function of N?         We see that the same function is called with nonnegative N, and it takes around N + 2 frames because the base-case call is at -1, and is also made as it grows with O(N)    

What is the time complexity?                                                                      O(N) because the function calls N times before arriving to the base case.                                                           

What is the auxiliary-space complexity?                                                     O(N), because the maximum number of recursive stack frames are active at once N increases proportionally
```

## Recursion vs. Iteration
```
Property                            Recursive Version                                                           Iterative Version
Time complexity                           O(N)                                                                      O(N)
Auxiliary-space complexity                O(N)                                                                      O(1)
Uses repeated stack frames?    Yes because until the base case is reached, each recursive call adds a frame.      No because there's no addition of recursive frames. The same variable is updated by the loop.
```
#### Then explain why two algorithms can have the same time complexity but different space complexities.
We see that both countdown methods have O(N0 time complexity because they process a number of values proportional to N. So every call. The recursive variant creates a new active stack frame. Resulting in O(N) auxiliary space. The iterative variant creates O(1) auxiliary space by using a loop, and a fixed number of variables. Meaning the algorithm will carry out comparable tasks. As they will arrange those tasks in memory in different ways.

## Peak Call-Stack Analysis
```
Situation                                                                 Maximum Stack Depth        Auxiliary Space  Reasoning
Recursive countdown whose argument decreases by 1 from N to 0              Proportional to N               O(N)       When the next call is being placed, each call will stay active. So the number of frames in the deepest point will be proportional to N

Recursive algorithm whose active call depth grows proportionally to log N  Proportional to log N         O(log N)    The maximum number of simultaneous active frames grows the logarithm of N
```
### Relate the second row to the lecture's discussion of Quicksort and its O(log N) recursive stack usage.
Quicksort can require O(log N) recursive stack space when its active recursive depth will increase based on log N in the lecture. We can compare the countdown recursion to the depth increasing based on N, and requires less stack space. So the peak active call depth is important to this measurement.

