AI DISCLAIMER: I forgot to mention this on the previous assignments, but I believe that for the past 2 or 3 assignments, I ask AI to create a skeleton for my .MD file that has all of the headers and sub-headers just to make things more convenient. I don't always use every header so that is why many get removed when I start filling it out. I also copy and pasted my code and outputs per section and asked "Does my code satisfy the requirements of this section"? This was mainly because there are so many sections and parts it is so very easy to miss something.  I did not ask it to provide answers, and when it would volunteer solutions, I did not use them, but may have looked at them for reference - often times the suggested solutions hinted at a direction to go, but didn't adequately solve the problem in a way I wanted and often tried to completely change my code. Sometimes it would also lead me in the wrong direction making me backtrack. As always, I am happy to walk through my code on call. 

Further update: For Dataset B on part 9, I used AI to come up with keys that would have similar or same hash indices. 

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

### Output  
For key 555223  
Found at index: 0  
Positions examined: 1  

For key 555980  
Found at index: 10  
Positions examined: 1  

For key 555000  
Found at index: 4  
Positions examined: 1  

For key 555890  
Found at index: 1  
Positions examined: 3  

For key 123456  
Key not found.  
Positions examined: 4  

For key 123456  
All 11 positions examined, key not found  


### Analysis


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
           else if (hashTable[i].status == "DELETED")
        {
            cout << "DELETED";
        }
        else
        {
            cout << "EMPTY";
        }
        
        cout << endl;
    }
}

void insertRecord(Record hashTable[], int tableSize, int key, string value)
{
    int homePosition = hashFunction(key, tableSize);

    // check whether the key already exists so we dont have duplicate entries on DELETE discovery. 
    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            break;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            hashTable[index].value = value;
            return;
        }
    }

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY" ||
            hashTable[index].status == "DELETED")
        {
            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].status = "USED";

            return;
        }
    }

    cout << "No available spaces in hash table." << endl;
}

bool removeRecord(Record hashTable[], int tableSize, int key)
{
    int homePosition = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            return false;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            hashTable[index].status = "DELETED";
            return true;
        }
    }

    return false;
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
    // searchRecord(hashTable, tableSize, 555223);
    // searchRecord(hashTable, tableSize, 555980);
    // searchRecord(hashTable, tableSize, 555000);
    // Case 2: Key exists but was displaced by a collision
    // searchRecord(hashTable, tableSize, 555890);
    // Case 3: Key does not exist and search reaches an EMPTY slot
    // searchRecord(hashTable, tableSize, 123456);
    
    //Test case for key not found
    // insertRecord(hashTable, tableSize, 2, "Test2");
    // insertRecord(hashTable, tableSize, 3, "Test3");
    // insertRecord(hashTable, tableSize, 5, "Test5");
    // insertRecord(hashTable, tableSize, 6, "Test6");
    // insertRecord(hashTable, tableSize, 7, "Test7");
    // insertRecord(hashTable, tableSize, 8, "Test8");
    // insertRecord(hashTable, tableSize, 9, "Test9");
    // searchRecord(hashTable, tableSize, 123456);

    printTable(hashTable, tableSize);
    cout << endl;
    cout << endl;
    
    bool removed = removeRecord(hashTable, tableSize, 555980);

    if (removed)
    {
        cout << "Key 555980 removed successfully." << endl;
    }
    else
    {
        cout << "Key not found." << endl;
    }
    
    printTable(hashTable, tableSize);
    cout << endl;
    cout << endl;
    
    cout << "Searching for displaced key down the probe chain:" << endl;
    searchRecord(hashTable, tableSize, 555890);

    return 0;
}
```

### Output  
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


Key 555980 removed successfully.  
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
Index 10: DELETED  


Searching for displaced key down the probe chain:  
For key 555890  
Found at index: 1  
Positions examined: 3  

### Analysis

We cant just mark it as EMPTY because doing so could have a probe stop prematurely. Instead we check if its EMPTY or DELETED, and then insert in DELETED only after confirming the key does not exist elsewhere in the chain. 


## Part 8 — Load Factor  
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
        else if (hashTable[i].status == "DELETED")
        {
            cout << "DELETED";
        }
        else
        {
            cout << "EMPTY";
        }
        
        cout << endl;
    }
}

void insertRecord(Record hashTable[], int tableSize, int key, string value, int &collisionsThisInsertion, int &totalPositionsExamined)
{
    int homePosition = hashFunction(key, tableSize);
    collisionsThisInsertion = 0;

    // check whether the key already exists so we dont have duplicate entries on DELETE discovery.
    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            break;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            hashTable[index].value = value;
            return;
        }
    }

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        totalPositionsExamined++;

        if (hashTable[index].status == "EMPTY" ||
            hashTable[index].status == "DELETED")
        {
            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].status = "USED";

            return;
        }
        else
        {
            collisionsThisInsertion++;
        }
    }

    cout << "No available spaces in hash table." << endl;
}

bool removeRecord(Record hashTable[], int tableSize, int key)
{
    int homePosition = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            return false;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            hashTable[index].status = "DELETED";
            return true;
        }
    }

    return false;
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

double loadFactor(Record hashTable[], int tableSize)
{
    int occupiedSlots = 0;

    for (int i = 0; i < tableSize; i++)
    {
        if (hashTable[i].status == "USED")
        {
            occupiedSlots++;
        }
    }
    
    return static_cast<double>(occupiedSlots) / tableSize;
}

int main()
{
    const int tableSize = 11;
    int collisionsThisInsertion = 0;
    int totalPositionsExamined = 0;
    int numberOfElements = 0;

    Record hashTable[tableSize];

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }
    
   
    insertRecord(hashTable, tableSize, 555223, "Geno", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555980, "Lex", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555000, "Zia", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555890, "Zezzy", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555214, "Test5", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555205, "Test6", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555196, "Test7", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555187, "Test8", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555178, "Test9", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;
    
    cout << endl;
    
    
    //Test case for key exists at its home position
    // searchRecord(hashTable, tableSize, 555223);
    // searchRecord(hashTable, tableSize, 555980);
    // searchRecord(hashTable, tableSize, 555000);
    // Case 2: Key exists but was displaced by a collision
    // searchRecord(hashTable, tableSize, 555890);
    // Case 3: Key does not exist and search reaches an EMPTY slot
    // searchRecord(hashTable, tableSize, 123456);
    
    
    //Test case for key not found
    // insertRecord(hashTable, tableSize, 2, "Test2", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 3, "Test3", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 5, "Test5", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 6, "Test6", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 7, "Test7", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 8, "Test8", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 9, "Test9", collisionsThisInsertion, totalPositionsExamined);
    // searchRecord(hashTable, tableSize, 123456);


    printTable(hashTable, tableSize);
    cout << endl;
    cout << endl;
    
    
    // bool removed = removeRecord(hashTable, tableSize, 555980);

    // if (removed)
    // {
    //     cout << "Key 555980 removed successfully." << endl;
    // }
    // else
    // {
    //     cout << "Key not found." << endl;
    // }
    
    // printTable(hashTable, tableSize);
    // cout << endl;
    // cout << endl;
    
    // cout << "Searching for displaced key down the probe chain:" << endl;
    // searchRecord(hashTable, tableSize, 555890);

    return 0;
}
```

### Analysis
As the table becomes increasingly full the number of collisions starts to rise. The less elements in the table, the more likely a key and value could be placed directly in their home positions. However, as it got more full, linear probing had to be utilized to greater extents to find available space. A hash table using open addressing cannot have a load factor over 1 because each position can only contain one record. As the load factor rises and approaches 1 performance decreases because more and more collisions are encountered. 

## Part 9 — Hash Function Quality
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
        else if (hashTable[i].status == "DELETED")
        {
            cout << "DELETED";
        }
        else
        {
            cout << "EMPTY";
        }
        
        cout << endl;
    }
}

void insertRecord(Record hashTable[], int tableSize, int key, string value, int &collisionsThisInsertion, int &totalPositionsExamined)
{
    int homePosition = hashFunction(key, tableSize);
    collisionsThisInsertion = 0;

    // check whether the key already exists so we dont have duplicate entries on DELETE discovery.
    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            break;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            hashTable[index].value = value;
            return;
        }
    }

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        totalPositionsExamined++;

        if (hashTable[index].status == "EMPTY" ||
            hashTable[index].status == "DELETED")
        {
            hashTable[index].key = key;
            hashTable[index].value = value;
            hashTable[index].status = "USED";

            return;
        }
        else
        {
            collisionsThisInsertion++;
        }
    }

    cout << "No available spaces in hash table." << endl;
}

bool removeRecord(Record hashTable[], int tableSize, int key)
{
    int homePosition = hashFunction(key, tableSize);

    for (int i = 0; i < tableSize; i++)
    {
        int index = (homePosition + i) % tableSize;

        if (hashTable[index].status == "EMPTY")
        {
            return false;
        }

        if (hashTable[index].status == "USED" &&
            hashTable[index].key == key)
        {
            hashTable[index].status = "DELETED";
            return true;
        }
    }

    return false;
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

double loadFactor(Record hashTable[], int tableSize)
{
    int occupiedSlots = 0;

    for (int i = 0; i < tableSize; i++)
    {
        if (hashTable[i].status == "USED")
        {
            occupiedSlots++;
        }
    }
    
    return static_cast<double>(occupiedSlots) / tableSize;
}

int main()
{
    const int tableSize = 11;
    int collisionsThisInsertion = 0;
    int totalPositionsExamined = 0;
    int numberOfElements = 0;

    Record hashTable[tableSize];

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }
    
   
    insertRecord(hashTable, tableSize, 555223, "Geno", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555980, "Lex", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555000, "Zia", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555890, "Zezzy", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555214, "Test5", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555205, "Test6", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555196, "Test7", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555187, "Test8", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;


    insertRecord(hashTable, tableSize, 555178, "Test9", collisionsThisInsertion, totalPositionsExamined);
    numberOfElements++;

    cout << "Elements: " << numberOfElements << " | "
         << "Load factor: " << loadFactor(hashTable, tableSize) << " | "
         << "Collisions this insertion: " << collisionsThisInsertion << " | "
         << "Avg. positions examined: " << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;
    
    cout << endl;
    
    
    //Test case for key exists at its home position
    // searchRecord(hashTable, tableSize, 555223);
    // searchRecord(hashTable, tableSize, 555980);
    // searchRecord(hashTable, tableSize, 555000);
    // Case 2: Key exists but was displaced by a collision
    // searchRecord(hashTable, tableSize, 555890);
    // Case 3: Key does not exist and search reaches an EMPTY slot
    // searchRecord(hashTable, tableSize, 123456);
    
    
    //Test case for key not found
    // insertRecord(hashTable, tableSize, 2, "Test2", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 3, "Test3", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 5, "Test5", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 6, "Test6", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 7, "Test7", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 8, "Test8", collisionsThisInsertion, totalPositionsExamined);
    // insertRecord(hashTable, tableSize, 9, "Test9", collisionsThisInsertion, totalPositionsExamined);
    // searchRecord(hashTable, tableSize, 123456);


    // printTable(hashTable, tableSize);
    // cout << endl;
    // cout << endl;
    
    
    // bool removed = removeRecord(hashTable, tableSize, 555980);

    // if (removed)
    // {
    //     cout << "Key 555980 removed successfully." << endl;
    // }
    // else
    // {
    //     cout << "Key not found." << endl;
    // }
    
    // printTable(hashTable, tableSize);
    // cout << endl;
    // cout << endl;
    
    // cout << "Searching for displaced key down the probe chain:" << endl;
    // searchRecord(hashTable, tableSize, 555890);


    //Dataset A
    
    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }

    collisionsThisInsertion = 0;
    totalPositionsExamined = 0;
    numberOfElements = 0;

    int datasetCollisions = 0;
    int maxPositionsExamined = 0;
    int positionsBeforeInsert = 0;
    int positionsThisInsertion = 0;

    int datasetA[7] =
    {
        555000,
        555001,
        555002,
        555003,
        555004,
        555005,
        555006
    };

    cout << "Dataset A" << endl;

    for (int i = 0; i < 7; i++)
    {
        positionsBeforeInsert = totalPositionsExamined;

        insertRecord(hashTable, tableSize, datasetA[i], "Dataset A", collisionsThisInsertion, totalPositionsExamined);

        numberOfElements++;
        datasetCollisions += collisionsThisInsertion;

        positionsThisInsertion =
            totalPositionsExamined - positionsBeforeInsert;

        if (positionsThisInsertion > maxPositionsExamined)
        {
            maxPositionsExamined = positionsThisInsertion;
        }
    }

    cout << "Number of keys: " << numberOfElements << endl;
    cout << "Collisions: " << datasetCollisions << endl;
    cout << "Maximum positions examined: " << maxPositionsExamined << endl;
    cout << "Average positions examined: "
         << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;

    cout << endl;


    //Dataset B

    for (int i = 0; i < tableSize; i++)
    {
        hashTable[i].status = "EMPTY";
    }

    collisionsThisInsertion = 0;
    totalPositionsExamined = 0;
    numberOfElements = 0;
    datasetCollisions = 0;
    maxPositionsExamined = 0;
    positionsBeforeInsert = 0;
    positionsThisInsertion = 0;

    int datasetB[7] =
    {
        555980,
        555890,
        555881,
        555872,
        555863,
        555854,
        555845
    };

    cout << "Dataset B" << endl;

    for (int i = 0; i < 7; i++)
    {
        positionsBeforeInsert = totalPositionsExamined;

        insertRecord(hashTable, tableSize, datasetB[i], "Dataset B",
                     collisionsThisInsertion, totalPositionsExamined);

        numberOfElements++;
        datasetCollisions += collisionsThisInsertion;

        positionsThisInsertion =
            totalPositionsExamined - positionsBeforeInsert;

        if (positionsThisInsertion > maxPositionsExamined)
        {
            maxPositionsExamined = positionsThisInsertion;
        }
    }

    cout << "Number of keys: " << numberOfElements << endl;
    cout << "Collisions: " << datasetCollisions << endl;
    cout << "Maximum positions examined: " << maxPositionsExamined << endl;
    cout << "Average positions examined: "
         << static_cast<double>(totalPositionsExamined) / numberOfElements << endl;

    return 0;
}
```
### Output  
Elements: 1 | Load factor: 0.0909091 | Collisions this insertion: 0 | Avg. positions examined: 1  
Elements: 2 | Load factor: 0.181818 | Collisions this insertion: 0 | Avg. positions examined: 1  
Elements: 3 | Load factor: 0.272727 | Collisions this insertion: 0 | Avg. positions examined: 1  
Elements: 4 | Load factor: 0.363636 | Collisions this insertion: 2 | Avg. positions examined: 1.5  
Elements: 5 | Load factor: 0.454545 | Collisions this insertion: 2 | Avg. positions examined: 1.8  
Elements: 6 | Load factor: 0.545455 | Collisions this insertion: 3 | Avg. positions examined: 2.16667  
Elements: 7 | Load factor: 0.636364 | Collisions this insertion: 0 | Avg. positions examined: 2  
Elements: 8 | Load factor: 0.727273 | Collisions this insertion: 7 | Avg. positions examined: 2.75  
Elements: 9 | Load factor: 0.818182 | Collisions this insertion: 8 | Avg. positions examined: 3.44444  

Dataset A  
Number of keys: 7  
Collisions: 0  
Maximum positions examined: 1  
Average positions examined: 1  

Dataset B  
Number of keys: 7  
Collisions: 21  
Maximum positions examined: 7  
Average positions examined: 4  

### Analysis
Dataset A performed much better. All keys were distributed to different home positions so no collisions occurred. On the other hand, dataset B produced far more because I had AI deliberately generate keys that hashed to the same index of 10. So after the first insertion, every subsequent key had to move further down the table via linear probing. When comparing these two data sets, its obvious that the quality of a hash function can greatly affect performance. Right now we are just taking a sum of the integers and dividing by the number of indices, but perhaps a more complex hashing method would allow for a better distribution and less reliance on probing. Naturally, we can also conclude that having keys aggregate around the same area of the table is also not ideal as there will be random insertions that require a lot more work. 

## Part 10 — Complexity Analysis

### Question 1
Linear search may have to examine all 1,000 elements - worst case O(N)

### Question 2
log₂(1000) = around 9.97, so almost 10 comparisons. 

### Question 3
As we established earlier, a well designed hash function either makes probing unnecessary or very minimal. This is because the function can go directly to the keys home position and find what it is looking for. A good distribution from a good hash function keeps collisions and probing to a minimum.

### Question 4
As we saw with dataset B, if every value hashes to the same value, collisions rise linearly: O(N). In a worst case situation, the program may have to probe through every value before finding the requested key or determining it doesn't exist. 

### Question 5
O(1) is the average time complexity for hash table search, and that's for an average distribution efficiency. The very existiance of collisions and probing show that the term "always" is inaccurate. 


## Part 11 — Hashing vs. Encryption

Hashing: This is a one way operation because the original data converts to a hash, but you cant always take that hash and determine what the original data was as multiple different values can hash to the same thing. 

Encryption: This is considered a two way operation because after a value is transformed via encryption, it could then be converted if the appropriate key is provided. It provides a way to protect data and make it unreadable unless an authorized user needs to access it with a valid key. 

Comparison: I feel like I described the differences pretty explicitly above. Hashing is a one way operation (defined above) and Encryption is a two way operation. Hashing would be appropriate for organizing something like student records, where a student ID can be converted into a table index to make searching more efficient. Encryption would be more appropriate for sensitive information, such as bank information, because the data needs to be protected but still recoverable by an authorized user.


## Part 12 — Cryptographic and Non-Cryptographic Hashing

### Deterministic Behavior  
Deterministic means that the same input should always result in the same hash.

### Pre-image Resistance  
Pre-image resistance means that if someone is given a hash value, it should be very hard to determine the input that would result in that hash.

### Avalanche Effect  
The avalanche effect means that even a very small change to the original input should result in a significantly different hash.

### Collision Resistance  
Collision resistance means that it should be difficult to find two different inputs that produce the same hash value.

### Comparison
A cryptographic hash is designed with security in mind with performance as a secondary goal. A non-cryptographic hash function is more focused on performance and efficiently distributing values.

## Part 13 — Applications and Limitations

### Scenario A — Exact Lookup  
Hash table here is good choice because hashing is known to be useful when a key is known. This allows for quick access and efficiency. 

### Scenario B — Range Query  
According to the lecture, a hash table would not be ideal here since its stated that hash tables are not suitable for finding records within a range since records are sorted by their hash values which means they are not necessarily sorted. 

### Scenario C — Sorted Traversal
Also in the lecture it states that hash tables do not naturally support visiting orders in key order. Hashing determines where records are placed based on their hash values, so the physical positions do not naturally represent the sorted order of the keys.

### Scenario D — Minimum Key  
The lecture also explicitly states that hash tables do not naturally support finding a min or max key as the keys are not necessarily stored in sorted order, as evidenced by the existence of collisions and probing. 

### Scenario E — Username Lookup  
This would be a good use of a hash lookup. In fact this is an example used throughout the course materials on this subject. Since the username is the key and the user info is the value, if the exact username is known, a program can quickly determine where to start looking. 

### Analysis
A
1.) Hashing can go directly to a key’s calculated location, giving O(1) average search time. Linear search is O(N), while binary search is O(log N).  
2.) Different keys can map to the same table index, which creates a collision.  
3.) Linear probing checks the following table positions until it finds an empty slot or the target key.  
4.) A good hash function spreads keys evenly, which reduces collisions and improves performance.  
5.) As the load factor increases, collisions become more likely and more positions may need to be examined.  
6.) Marking a deleted slot as EMPTY could cause a search to stop too early, so a tombstone is used instead.  
7.) Tombstones allow searches to continue through deleted positions, but they can increase the amount of probing required.  
8.) They are usually O(1) when keys are distributed well, but many collisions can force the program to examine much of the table, producing O(N) behavior.  
9.) Hash tables are not ideal for range searches, sorted traversal, or quickly finding minimum and maximum keys.  

