# Assignment W13 > 1/1 Space Constraints
## Lab Objective

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
