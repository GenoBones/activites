# Hash Tables and Linear Probing

## Part 1 — Understanding Hash Functions

### Calculations

Key: 555223
Digit Sum: 5 + 5 + 5 + 2 + 2 + 3 = 22
Table Index: 22 % 10 = 2

Key: 555980
Digit Sum: 5 + 5 + 5 + 9 + 8 + 0 = 32
Table Index: 32 % 10 = 2

Key: 555000
Digit Sum: 5 + 5 + 5 + 0 + 0 + 0 = 15
Table Index: 15 % 10 = 5

Key: 555890
Digit Sum: 5 + 5 + 5 + 8 + 9 + 0 = 32
Table Index: 32 % 10 = 2

### Analysis  
Three keys produced the same table index of 2. This is a called a collision. 

Using % 10 ensures a valid table index because the remainder after dividing by 10 can only be between 0 and 9. Since a 10-slot table has indices 0 through 9, the there will always be an index available within the table.

Increasing the size of the table would not guarantee that collisions never happen. It could reduce the chance of collisions because there would be more possible locations to place values, but different keys could still eventually produce the same index. As long as there are more possible keys than table positions, collisions are always possible.

## Part 2 — Implement a Hash Function

### C++ Implementation

```
#include <iostream>
using namespace std;

int hashFunction(int key, int tableSize)
{
    int digitSum = 0;

    while (key > 0)
    {
        digitSum += key % 10;
        key = key / 10;
    }

    int index = digitSum % tableSize;

    return index;
}

int main()
{
    const int tableSize = 10;
    
    int index1 = hashFunction(555223, tableSize);
    int index2 = hashFunction(555980, tableSize);
    int index3 = hashFunction(555000, tableSize);
    int index4 = hashFunction(555890, tableSize);


    cout << "555223 -> Index at " << index1 << endl;

    cout << "555980 -> Index at " << index2 << endl;

    cout << "555000 -> Index at " << index3 << endl;

    cout << "555890 -> Index at " << index4 << endl;

    return 0;
}
```
### Test Results  
555223 -> Index at 2
555980 -> Index at 2
555000 -> Index at 5
555890 -> Index at 2


## Part 3 — Build a Hash Table

```
#include <iostream>
#include <string>
using namespace std;

struct Record
{
    int key;
    string value;
    string status;
};

int main()
{
    const int tableSize = 11;

    Record hashTable[tableSize];

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
        cout << hashTable[i].status << endl;
    }
    

    return 0;
}
```


## Part 4 — Linear Probing
```
#include <iostream>
#include <string>
using namespace std;

struct Record
{
    int key;
    string value;
    string status;
};

int hashFunction(int key, int tableSize)
{
    int digitSum = 0;

    while (key > 0)
    {
        digitSum += key % 10;
        key = key / 10;
    }

    return digitSum % tableSize;
}

void insertRecord(Record hashTable[], int tableSize, int key, string value)
{
    int homePosition = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].status = "USED";

            return;
        }

        if (hashTable[index].key == key)
        {
            hashTable[index].value = value;

            return;
        }
    }

    cout << "No availible spaces in hash table." << endl;
    return;
}

void printTable(Record hashTable[], int tableSize)
{
    for (int i = 0; i < tableSize; i++)
    {
        cout << "Index " << i << ": ";

        if (hashTable[i].status == "USED")
        {
            cout << hashTable[i].key << " " << hashTable[i].value;
        }
        else
        {
            cout << "EMPTY";
        }

        cout << endl;
    }
}
int main()
{
    const int tableSize = 11;

    Record hashTable[tableSize];

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }

    insertRecord(hashTable, tableSize, 555223, "Geno");
    insertRecord(hashTable, tableSize, 555980, "Lex");
    insertRecord(hashTable, tableSize, 555000, "Zia");
    insertRecord(hashTable, tableSize, 555890, "Zezzy");
    
    printTable(hashTable, tableSize);
not re
    return 0;
}
```
### Analysis
I prevent an infinite loop by only letting the program examine only the total number of positions in the table. The hash table contains 11 slots so it can only make 11 probes. 


## Part 5 — Home Position and Actual Position

```
#include <iostream>
#include <string>
using namespace std;

struct Record
{
    int key;
    string value;
    string status;
};

int hashFunction(int key, int tableSize)
{
    int digitSum = 0;

    while (key > 0)
    {
        digitSum += key % 10;
        key = key / 10;
    }

    return digitSum % tableSize;
}

void insertRecord(Record hashTable[], int tableSize, int key, string value)
{
    int homePosition = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].status = "USED";

            return;
        }

        if (hashTable[index].key == key)
        {
            hashTable[index].value = value;

            return;
        }
    }

    cout << "No availible spaces in hash table." << endl;
    return;
}

void printTable(Record hashTable[], int tableSize)
{
    for (int i = 0; i < tableSize; i++)
    {
        cout << "Index " << i << ": ";

        if (hashTable[i].status == "USED")
        {
            int homePosition = hashFunction(hashTable[i].key, tableSize);

            cout << " | " << "Key - " << hashTable[i].key << " | "
                 << " | " << " Value - " << hashTable[i].value << " | "
                 << " | " << " Home Position: " << homePosition << " | "
                 << " | " << " Actual Position: " << i << " | ";
        }
        else
        {
            cout << "EMPTY";
        }

        cout << endl;
    }
}
int main()
{
    const int tableSize = 11;

    Record hashTable[tableSize];

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }

    insertRecord(hashTable, tableSize, 555223, "Geno");
    insertRecord(hashTable, tableSize, 555980, "Lex");
    insertRecord(hashTable, tableSize, 555000, "Zia");
    insertRecord(hashTable, tableSize, 555890, "Zezzy");
    
    printTable(hashTable, tableSize);

    return 0;
}
```

### Program Output

Index 0:  | Key - 555223 |  |  Value - Geno |  |  Home Position: 0 |  |  Actual Position: 0 |   
Index 1:  | Key - 555890 |  |  Value - Zezzy |  |  Home Position: 10 |  |  Actual Position: 1 |   
Index 2: EMPTY  
Index 3: EMPTY  
Index 4:  | Key - 555000 |  |  Value - Zia |  |  Home Position: 4 |  |  Actual Position: 4 |   
Index 5: EMPTY  
Index 6: EMPTY  
Index 7: EMPTY  
Index 8: EMPTY  
Index 9: EMPTY  
Index 10:  | Key - 555980 |  |  Value - Lex |  |  Home Position: 10 |  |  Actual Position: 10 |   

### Analysis

Its possible that a key gets stored somewhere other than its home position because that index may already be occupied (collision). When such a scenario occurs, linear probing is used to start from that occupied home position and then checks each following position for an open position. If one isn't found by the end of the table, the search wraps around to the beginning of the table. The first empty position becomes that value actual position. The main reason a large distance between home position and actual position can make an operation slower is simply because more values have to be probed which means more comparisons. 

## Part 6 — Searching with Linear Probing

```
#include <iostream>
#include <string>
using namespace std;

struct Record
{
    int key;
    string value;
    string status;
};

int hashFunction(int key, int tableSize)
{
    int digitSum = 0;

    while (key > 0)
    {
        digitSum += key % 10;
        key = key / 10;
    }

    return digitSum % tableSize;
}

void insertRecord(Record hashTable[], int tableSize, int key, string value)
{
    int homePosition = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].status = "USED";

            return;
        }

        if (hashTable[index].key == key)
        {
            hashTable[index].value = value;

            return;
        }
    }

    cout << "No availible spaces in hash table." << endl;
    return;
}

void printTable(Record hashTable[], int tableSize)
{
    for (int i = 0; i < tableSize; i++)
    {
        cout << "Index " << i << ": ";

        if (hashTable[i].status == "USED")
        {
            int homePosition = hashFunction(hashTable[i].key, tableSize);

            cout << " | " << "Key - " << hashTable[i].key << " | "
                 << " | " << " Value - " << hashTable[i].value << " | "
                 << " | " << " Home Position: " << homePosition << " | "
                 << " | " << " Actual Position: " << i << " | ";
        }
        else
        {
            cout << "EMPTY";
        }

        cout << endl;
    }
}

void searchRecord(Record hashTable[], int tableSize, int key)
{
    int homePosition = hashFunction(key, tableSize);
    int positionsExamined = 0;

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;
        positionsExamined++;

        if (hashTable[index].status == "EMPTY")
        {
            cout << "For key " << key << endl;
            cout << "Key not found." << endl;
            cout << "Positions examined: " << positionsExamined << endl;
            cout << endl;
            return;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            cout << "For key " << key << endl;
            cout << "Found at index: " << index << endl;
            cout << "Positions examined: " << positionsExamined << endl;
            cout << endl;

            return;
        }
    }
    
    cout << "For key " << key << endl;
    cout << "All " << positionsExamined << " positions examined, key not found " << endl;
    cout << endl;
    
    return;
}

int main()
{
    const int tableSize = 11;

    Record hashTable[tableSize];

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }
    
   
    insertRecord(hashTable, tableSize, 555223, "Geno");
    insertRecord(hashTable, tableSize, 555980, "Lex");
    insertRecord(hashTable, tableSize, 555000, "Zia");
    insertRecord(hashTable, tableSize, 555890, "Zezzy");
    
    //Test case for key exists at its home position
    searchRecord(hashTable, tableSize, 555223);
    searchRecord(hashTable, tableSize, 555980);
    searchRecord(hashTable, tableSize, 555000);
    // Case 2: Key exists but was displaced by a collision
    searchRecord(hashTable, tableSize, 555890);
    // Case 3: Key does not exist and search reaches an EMPTY slot
    searchRecord(hashTable, tableSize, 123456);
    
    //Test case for key not found
    insertRecord(hashTable, tableSize, 2, "Test2");
    insertRecord(hashTable, tableSize, 3, "Test3");
    insertRecord(hashTable, tableSize, 5, "Test5");
    insertRecord(hashTable, tableSize, 6, "Test6");
    insertRecord(hashTable, tableSize, 7, "Test7");
    insertRecord(hashTable, tableSize, 8, "Test8");
    insertRecord(hashTable, tableSize, 9, "Test9");
    
    searchRecord(hashTable, tableSize, 123456);

    // printTable(hashTable, tableSize);

    return 0;
}
```

### Analysis


## Part 7 — Deletion and Tombstones

### Remove Operation

### C++ Implementation

### Demonstration

### Analysis


## Part 8 — Load Factor

### Load Factor Calculation

### Experiment

### Results

### Analysis


## Part 9 — Hash Function Quality

### Dataset A

### Dataset B

### Results

### Analysis


## Part 10 — Complexity Analysis

### Question 1

### Question 2

### Question 3

### Question 4

### Question 5


## Part 11 — Hashing vs. Encryption

### Hashing

### Encryption

### Comparison

### Example Where Hashing Is Appropriate

### Example Where Encryption Is Appropriate


## Part 12 — Cryptographic and Non-Cryptographic Hashing

### Deterministic Behavior

### Pre-image Resistance

### Avalanche Effect

### Collision Resistance

### Comparison


## Part 13 — Applications and Limitations

### Scenario A — Exact Lookup

### Scenario B — Range Query

### Scenario C — Sorted Traversal

### Scenario D — Minimum Key

### Scenario E — Username Lookup

### Analysis


## Analysis and Reflection
