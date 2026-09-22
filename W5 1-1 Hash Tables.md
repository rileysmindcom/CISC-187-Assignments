# Assignment W5>1/1 Hash tables
## Lab Objective
My lab objective is understanding how hash tables store, and retrieve key value pairs, and how hash functions impact those operations performances. In this lab I'll use linear probing to create a hash table in C++, and handle collisions. I'll work on developing a hash function, handling collisions, and adding the finding records removing records with tombstones. Then figuring out the load factor. I will look into how a hash function's quality, and a table's fullness would have an impact on how many positions are looked at during operations. Another goal of this lab is comparing the hash table searching with linear, and binary to recognize why hash tables can provide O(1) average-case lookup while still having O(N) worst-case behavior. Then I will at last examine the difference between hashing, and encryption to identify situations where hash tables are, and are not appropriate data structures.
## Assignment Code
```cpp
#include <iostream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;

enum class SlotState {
    EMPTY,
    OCCUPIED,
    DELETED
};

struct Record {
    int key = 0;
    string value = "";
    int homePosition = -1;
    SlotState state = SlotState::EMPTY;
};

class HashTable {
private:
    vector<Record> table;
    int tableSize;
    int collisionCount;

public:
    HashTable(int size) {
        tableSize = size;
        table.resize(size);
        collisionCount = 0;
    }

    int hashFunction(int key) const {
        int digitSum = 0;

        if (key == 0) {
            return 0;
        }

        key = abs(key);

        while (key > 0) {
            digitSum += key % 10;
            key /= 10;
        }

        return digitSum % tableSize;
    }

    bool insert(int key, const string& value) {
        int home = hashFunction(key);
        int firstDeleted = -1;

        for (int i = 0; i < tableSize; i++) {
            int index = (home + i) % tableSize;

            if (table[index].state == SlotState::EMPTY) {
                if (firstDeleted != -1) {
                    index = firstDeleted;
                }

                table[index].key = key;
                table[index].value = value;
                table[index].homePosition = home;
                table[index].state = SlotState::OCCUPIED;

                return true;
            }

            if (table[index].state == SlotState::DELETED) {
                if (firstDeleted == -1) {
                    firstDeleted = index;
                }

                continue;
            }

            if (table[index].key == key) {
                table[index].value = value;
                return true;
            }

            collisionCount++;
        }

        if (firstDeleted != -1) {
            table[firstDeleted].key = key;
            table[firstDeleted].value = value;
            table[firstDeleted].homePosition = home;
            table[firstDeleted].state = SlotState::OCCUPIED;

            return true;
        }

        return false;
    }

    bool search(int key, string& value, int& positionsExamined) const {
        int home = hashFunction(key);
        positionsExamined = 0;

        for (int i = 0; i < tableSize; i++) {
            int index = (home + i) % tableSize;
            positionsExamined++;

            if (table[index].state == SlotState::EMPTY) {
                return false;
            }

            if (table[index].state == SlotState::OCCUPIED &&
                table[index].key == key) {
                value = table[index].value;
                return true;
            }

        }

        return false;
    }

    bool remove(int key) {
        int home = hashFunction(key);

        for (int i = 0; i < tableSize; i++) {
            int index = (home + i) % tableSize;

            if (table[index].state == SlotState::EMPTY) {
                return false;
            }

            if (table[index].state == SlotState::OCCUPIED &&
                table[index].key == key) {

                table[index].state = SlotState::DELETED;
                table[index].value = "";
                return true;
            }
        }

        return false;
    }

    double loadFactor() const {
        int occupied = 0;

        for (const Record& record : table) {
            if (record.state == SlotState::OCCUPIED) {
                occupied++;
            }
        }

        return static_cast<double>(occupied) / tableSize;
    }

    void display() const {
        cout << "\nHash Table\n";
        cout << "------------------------------------------------------------\n";
        cout << left
            << setw(8) << "Index"
            << setw(12) << "Key"
            << setw(20) << "Value"
            << setw(12) << "Home"
            << setw(12) << "State"
            << "\n";
        cout << "------------------------------------------------------------\n";

        for (int i = 0; i < tableSize; i++) {
            cout << left << setw(8) << i;

            if (table[i].state == SlotState::EMPTY) {
                cout << setw(12) << "-"
                    << setw(20) << "-"
                    << setw(12) << "-"
                    << setw(12) << "EMPTY";
            }
            else if (table[i].state == SlotState::DELETED) {
                cout << setw(12) << "-"
                    << setw(20) << "-"
                    << setw(12) << "-"
                    << setw(12) << "DELETED";
            }
            else {
                cout << setw(12) << table[i].key
                    << setw(20) << table[i].value
                    << setw(12) << table[i].homePosition
                    << setw(12) << "OCCUPIED";
            }

            cout << "\n";
        }

        cout << "------------------------------------------------------------\n";
        cout << "Load Factor: " << loadFactor() << "\n";
    }

    int getCollisionCount() const {
        return collisionCount;
    }
};

int main() {
    HashTable hashTable(11);

    hashTable.insert(555223, "John Smith");
    hashTable.insert(555980, "Sarah Connor");
    hashTable.insert(555000, "Isaac Newton");
    hashTable.insert(555890, "Ada Lovelace");

    cout << "Initial Table:";
    hashTable.display();

    string value;
    int positionsExamined;

    cout << "\nSearch Results\n";

    if (hashTable.search(555223, value, positionsExamined)) {
        cout << "555223 found: " << value
            << " | Positions examined: "
            << positionsExamined << "\n";
    }

    if (hashTable.search(555890, value, positionsExamined)) {
        cout << "555890 found: " << value
            << " | Positions examined: "
            << positionsExamined << "\n";
    }

    if (!hashTable.search(123456, value, positionsExamined)) {
        cout << "123456 not found"
            << " | Positions examined: "
            << positionsExamined << "\n";
    }

    cout << "\nDeleting 555223...\n";
    hashTable.remove(555223);

    hashTable.display();

    if (hashTable.search(555980, value, positionsExamined)) {
        cout << "\n555980 is still searchable after deletion."
            << " Positions examined: "
            << positionsExamined << "\n";
    }

    cout << "\nTotal collisions recorded: "
        << hashTable.getCollisionCount() << "\n";

    return 0;
}
```

## Analysis, and Reflection
# Analysis

## Part 1 — Hash Function Calculations

Each key's individual digits are added by simple hash functions to use the table size determining the remainder
| Key    | Digit Sum | Table Index |
| ------ | --------: | ----------: |
| 555223 |        22 |           2 |
| 555980 |        32 |           2 |
| 555000 |        15 |           5 |
| 555890 |        35 |           5 |

A collision occurs when the keys `555223` and `555980` generate the same index.

When %10 i used is to ensure the final index falls between 0, and 9 to correspond to a 10 slot table's valid indexes. An index from 0 to 'tableSize - 1' is produced when '%tablesize ' is used.
This can lower the likelihood of accidents increasing the table size cannot ensure that they won't happen because numerous potential keys are mapped into a small number of table places with several keys resulting in the same index.

## Part 2 — Home Position and Actual Position

The Index that has the hash function initially generated is the home position. In the event that another key occupies that position, linear probing looks for an open spot by checking the available positions
As an example:

```text
Home position: 3

3 -> occupied
4 -> occupied
5 -> available
```

The key's real location changes to '5' from its home position of '3'.
Searching, and insertion may take longer if there is a greater gap between the home position, and the real position because more table positions must be looked at.

## Part 3 — Linear Probing and Searching

Looking at the consecutive table places linear probing resolves collisions:
```text
home -> home + 1 -> home + 2 -> ...
```

The probe sequence can loop back to the table's beginning. Because of the modulo operator:

```cpp
index = (home + i) % tableSize;
```

A hash-table searching is typically defined as 'O(1)' average-case, a misplaced key may take many table accesses. The 'O(1)' makes the asumption that the table is not very big, and that the hash algorithm destributes the keys fairly.
Multiple inspections could be necessary for a lost key. Especially if the key's probe sequence contains deleted or occupied places.
## Part 4 — Deletion and Tombstones

When using a linear probing it is not possible to simply change a removed slot to 'EMPTY'.
As an example:

```text
Index 3 -> Key A
Index 4 -> Key B
Index 5 -> Key C
```

Let's say Key A is removed. A search for Key C could start at index '3', find an empty slot, and mistakenly assume Key C does not exist, if index '3' becomes 'EMPTY'
Instead, index '3' turns into
```text
DELETED
```
We can refer this to a *tombstone*

Because another key might appear later in the same collision chain, a search proceeds through a tombstone. While a tombstone may possibly be reused in subsequent insertions.
## Part 5 — Load Factor

The load factor can be calculated as
```text
Load Factor = Occupied Slots / Total Slots
```

As an example if there are 5 out of 11 positions are occupied

```text
Load Factor = 5 / 11
            ≈ 0.455
```

Each table slot can only contain one active record an open-addressing hash table cannot have a load factor higher than '1.0'. The table reaches a load of '1.0' every position is occupied.
Meaning the performance for insertion, and searching decreases when the load factor gets closer to '1.0' because collisions, and probing sequences typically can get longer.

### Experimental Results

Then I can record my actual results in the after running the program.
|        Elements |     Load Factor |      Collisions | Average Positions Examined |
| --------------: | --------------: | --------------: | -------------------------: |
| [actual result] | [actual result] | [actual result] |            [actual result] |
| [actual result] | [actual result] | [actual result] |            [actual result] |
| [actual result] | [actual result] | [actual result] |            [actual result] |
| [actual result] | [actual result] | [actual result] |            [actual result] |

As the table gets increasingly crowded it is projected that the number of searches would rise.
## Part 6 — Hash Function Quality

Keys should be distributed equally throughout the table with a suitable hash algorithm. Longer collision chains are produced by linear probing when multiple keys are consistently producing the same or adjacent indexes.
An example:
| Measurement    | Dataset A | Dataset B |
| -------------- | --------: | --------: |
| Number of keys |  [actual] |  [actual] |
| Collisions     |  [actual] |  [actual] |
| Maximum probes |  [actual] |  [actual] |
| Average probes |  [actual] |  [actual] |

Dataset B should include the keys that generate the same or comparable indexes, Dataset A should comprise the keys generating a diversity of indices.
## Part 7 — Complexity Analysis

### Question 1
In the worst scenario linear search may look at all 1,000 elements in an unordered array.

```text
Worst case = O(N)
```

### Question 2

Then binary search divides the search space in half for 1,000 elements

```text
log₂(1000) ≈ 9.97
```

The logarithmic number of divisions, and binary search become necessary for 10 comparisons, as the number of comparisons depend on implementation.
### Question 3

Because the hash function immediately determines an index where the key should be found a well-designed hash table can find a key in constant time on average. Only a few positions need to be look at if the collisions are restricted.
### Question 4

When a large number of keys collide the hash function distributes keys poorly or the table is very crowded hash table searching can degenerate to 'O(N)'. In the worst scenario a significant amount of the table could need to be examined by the search.
### Question 5

The statement:

```text
Hash table search is always O(1).
```

Is incorrect because the right circumstances for average-case behavior can be described by 'O(1)' in the worst scenario hash-table operations can become 'O(N)' when collisions result in lengthy searching sequences.
## Part 8 — Hashing vs. Encryption

Hashing, is intended to be a one-way process. A value is transformed into a fixed-size hash value, and is typically not possible to retrieve the original value straight from the hash. When using an appropriate password-hashing technique can save the password verification information is an example of hashing. Information protected by encryption but with the right key authorized users can retrieve the original data. While, encryption is used as a safeguard for critical files or private messages.

```text
Hashing != Encryption
```

## Part 9 — Cryptographic Hashing

One of the characteristics for cryptographic hash functions are important for security.
**Deterministic behavior:** - Produces a hash output the same for the same input.

**Pre-image resistance:** - We find an input that generates a hash that should be computationally challenging given a hash value.

**Avalanche effect:** - A small change in the input should cause a large and unpredictable change in the resulting hash.

**Collision resistance:** - We find two inputs that yield to the same hash that should be computationally challenging.

This means that a typically in-memory hash-table hash function are different. Instead than cryptographic security its main objective is the effective distribution of keys among table seats. Compared to a cryptographic hash function, a hash table function can be significantly faster, and simpler.

## Part 10 — Applications and Limitations

### Scenario A — Exact Lookup

The key can be mapped directly to a table location, a hash table becomes a good choice for finding a student record from the exact student ID.

### Scenario B — Range Query

We can find every student whose ID false between 500000, and 600000 is typically not possible with a hash table. Instead of keeping keys in sorted order, hash tables are designed for exact-key look ups.

### Scenario C — Sorted Traversal

The display of records in ascending key order is not a natural feature of a hash table. This operation would typically be better served by another data structure such as a sorted array or balance searched tree.

### Scenario D — Minimum Key

The smallest key cannot be found using a hash table. We can find the minimum could be necessary at looking many or all sorted records because it does not keep the keys sorted in order.
### Scenario E — Username Lookup

A username can be used as a key to retrieve a user profile, hash table can be helpful for precise username lookup.

## Flowchart
<img width="602" height="593" alt="W5 1-1 Hash Tables" src="https://github.com/user-attachments/assets/db9ce19b-1455-4788-a4d1-10460d2171b8" />

## Challenges
The challenges I've experienced was the difference between a key's home location, and its real position following a collision. Then I needed to understand how linear searching is available for the next position, and how the same searching sequence is used during that process. Then was understanding why tombstones are required when erasing records presented. To prevent an endless loop I needed to ensure that the software stops searching after examining each table location. And, at last was recognizing a hash-table efficiency by relating collisions, and load factor to the O(1) average, and O(N) worst-case performance.
