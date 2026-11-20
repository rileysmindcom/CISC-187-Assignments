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

