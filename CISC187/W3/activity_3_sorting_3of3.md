AI DISCLAIMER: Chat GPT was used to generate a slightly disordered array. No other use AI code was used.   
QUERY: "can you generate me an array with 50 elements that is slightly disordered. There should be 10 disordered pairs for me to test some sorting algos with"

OUTPUT: "Here’s a 50-element array with exactly 10 adjacent out-of-order pairs. That should work well for testing your function:"
```
#include <iostream>
int numbers[arraySize] =
{
    1, 2, 4, 3, 5,
    6, 8, 7, 9, 10,
    12, 11, 13, 15, 14,
    16, 17, 19, 18, 20,
    21, 23, 22, 24, 25,
    27, 26, 28, 30, 29,
    31, 32, 34, 33, 35,
    36, 38, 37, 39, 40,
    41, 42, 43, 44, 45,
    46, 47, 48, 49, 50
};
```

# Adaptive Sorting Strategy

## Part A — Adaptive Sorting Selection

### Step 1 — Analyze the Input Order

```
#include <iostream>
using namespace std;

int examineOrderliness(const int numbers[], int size)
{
    int disorderedPairCount = 0;

    for (int i = 0; i < size - 1; i++)
    {
        if (numbers[i] > numbers[i + 1])
        {
            disorderedPairCount++;
        }
    }

    return disorderedPairCount;
}

int main()
{
    const int arraySize = 50;
    
    int numbers[arraySize] =
{
    1, 2, 4, 3, 5,
    6, 8, 7, 9, 10,
    12, 11, 13, 15, 14,
    16, 17, 19, 18, 20,
    21, 23, 22, 24, 25,
    27, 26, 28, 30, 29,
    31, 32, 34, 33, 35,
    36, 38, 37, 39, 40,
    41, 42, 43, 44, 45,
    46, 47, 48, 49, 50
};
    int disorderedPairCount = examineOrderliness(numbers, arraySize);

    cout << "Out-of-order adjacent pairs: "
         << disorderedPairCount << endl;

    return 0;
}
```

Output:  
Out-of-order adjacent pairs: 10

### Step 2 — Define a Threshold
Since a 50 element array has 49 adjacent pairs, and my array is known to have 10 disordered adjacent pairs, that means only about 20% of the adjacent pairs are out of order. Because the large majority of the array is still ordered correctly, I am considering this to be Best/Nearly Sorted. To keep things simple, arrays with about 20% or fewer disordered adjacent pairs will be considered nearly sorted. Anything in the middle will be considered Average/Partially Ordered, while arrays with a large majority of disordered adjacent pairs will be considered Worst/Highly Reverse-Ordered. That being said, the explicit threshold values are as follows:

0–10 OoO pairs = Best/Nearly Sorted
11–34 OoO pairs = Average/Partially Ordered
35–49 OoO pairs = Worst/Highly Reverse-Ordered

```
#include <iostream>
#include <string>
using namespace std;

int examineOrderliness(const int numbers[], int size)
{
    int disorderedPairCount = 0;

    for (int i = 0; i < size - 1; i++)
    {
        if (numbers[i] > numbers[i + 1])
        {
            disorderedPairCount++;
        }
    }

    return disorderedPairCount;
}

string classifyOrderlinessThreshhold(int disorderedPairCount)
    {
        if (disorderedPairCount >= 0 && disorderedPairCount <= 10)
        {
            return "best";
        }
        else if (disorderedPairCount > 10 && disorderedPairCount <= 34)
        {
         return "average";   
        }
        else
        {
         return "worst";   
        }
    }

int main()
{
    const int arraySize = 50;
    
    int numbers[arraySize] =
{
    1, 2, 4, 3, 5,
    6, 8, 7, 9, 10,
    12, 11, 13, 15, 14,
    16, 17, 19, 18, 20,
    21, 23, 22, 24, 25,
    27, 26, 28, 30, 29,
    31, 32, 34, 33, 35,
    36, 38, 37, 39, 40,
    41, 42, 43, 44, 45,
    46, 47, 48, 49, 50
};
    int disorderedPairCount = examineOrderliness(numbers, arraySize);
    
    string orderLevel = classifyOrderlinessThreshhold(disorderedPairCount);

    cout << "Out-of-order adjacent pair count: "
     << disorderedPairCount << endl
     << "Orderliness Threshold is: "
     << orderLevel << endl;

    return 0;
}
```
Output:  
Out-of-order adjacent pair count: 10
Orderliness Threshold is: best

### Step 3 — Select the Sorting Algorithm

```
#include <iostream>
#include <string>
using namespace std;

int examineOrderliness(const int numbers[], int size)
{
    int disorderedPairCount = 0;

    for (int i = 0; i < size - 1; i++)
    {
        if (numbers[i] > numbers[i + 1])
        {
            disorderedPairCount++;
        }
    }

    return disorderedPairCount;
}

string classifyOrderlinessThreshhold(int disorderedPairCount)
{
    if (disorderedPairCount == 0)
    {
        return "perfect";
    }
    else if (disorderedPairCount >= 0 && disorderedPairCount <= 10)
    {
        return "best";
    }
    else if (disorderedPairCount > 10 && disorderedPairCount <= 34)
    {
        return "average";
    }
    else
    {
        return "worst";
    }
}

void insertionSort(int numbers[], int size)
{
    for (int i = 1; i < size; i++)
    {
        int key = numbers[i];
        int j = i - 1;

        while (j >= 0 && numbers[j] > key)
        {
            numbers[j + 1] = numbers[j];
            j--;
        }

        numbers[j + 1] = key;
    }
}

void selectionSort(int numbers[], int size)
{
    for (int i = 0; i < size - 1; i++)
    {
        int smallestIndex = i;

        for (int j = i + 1; j < size; j++)
        {
            if (numbers[j] < numbers[smallestIndex])
            {
                smallestIndex = j;
            }
        }

        int temporaryValue = numbers[i];
        numbers[i] = numbers[smallestIndex];
        numbers[smallestIndex] = temporaryValue;
    }
}

void printArray(const int numbers[], int size)
{
    for (int i = 0; i < size; i++)
    {
        cout << numbers[i] << " ";
    }

    cout << endl;
}

int main()
{
    const int arraySize = 50;

    int numbers[arraySize] =
        {
            1, 2, 4, 3, 5,
            6, 8, 7, 9, 10,
            12, 11, 13, 15, 14,
            16, 17, 19, 18, 20,
            21, 23, 22, 24, 25,
            27, 26, 28, 30, 29,
            31, 32, 34, 33, 35,
            36, 38, 37, 39, 40,
            41, 42, 43, 44, 45,
            46, 47, 48, 49, 50};

    cout << "Array Before Sorting:" << endl;
    printArray(numbers, arraySize);

    int disorderedPairCount = examineOrderliness(numbers, arraySize);

    string orderLevel = classifyOrderlinessThreshhold(disorderedPairCount);

    cout << "Out-of-order adjacent pair count: "
         << disorderedPairCount << endl
         << "Orderliness Threshold is: "
         << orderLevel << endl;

    if (orderLevel == "best")
    {
        cout << "Sorting Algorithm Selected: Insertion Sort" << endl;

        insertionSort(numbers, arraySize);
    }
    else
    {
        cout << "Sorting Algorithm Selected: Selection Sort" << endl;

        selectionSort(numbers, arraySize);
    }

    disorderedPairCount = examineOrderliness(numbers, arraySize);
    orderLevel = classifyOrderlinessThreshhold(disorderedPairCount);

    cout << endl;
    cout << "Array After Sorting:" << endl;
    printArray(numbers, arraySize);
    cout << "Out-of-order adjacent pair count: "
         << disorderedPairCount << endl;

    cout << "Orderliness Threshold is: "
         << orderLevel << endl;

    return 0;
}
```

Output:
Array Before Sorting:  
1 2 4 3 5 6 8 7 9 10 12 11 13 15 14 16 17 19 18 20 21 23 22 24 25 27 26 28 30 29 31 32 34 33 35 36 38 37 39 40 41 42 43 44 45 46 47 48 49 50   
Out-of-order adjacent pair count: 10  
Orderliness Threshold is: best  
Sorting Algorithm Selected: Insertion Sort  

Array After Sorting:  
1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50   
Out-of-order adjacent pair count: 0  
Orderliness Threshold is: perfect  

## Part B — Case Classification Without Sorting

```
#include <iostream>
#include <string>
using namespace std;

int examineOrderliness(const int numbers[], int size)
{
    int disorderedPairCount = 0;

    for (int i = 0; i < size - 1; i++)
    {
        if (numbers[i] > numbers[i + 1])
        {
            disorderedPairCount++;
        }
    }

    return disorderedPairCount;
}

string classifyOrderlinessThreshhold(int disorderedPairCount)
{
    if (disorderedPairCount == 0)
    {
        return "perfect";
    }
    else if (disorderedPairCount >= 0 && disorderedPairCount <= 10)
    {
        return "best";
    }
    else if (disorderedPairCount > 10 && disorderedPairCount <= 34)
    {
        return "average";
    }
    else
    {
        return "worst";
    }
}

void printArray(const int numbers[], int size)
{
    for (int i = 0; i < size; i++)
    {
        cout << numbers[i] << " ";
    }

    cout << endl;
}

int main()
{
    const int arraySize = 50;
    
    int numbers[arraySize];

    cout << "Enter 50 integers:" << endl;

    for (int i = 0; i < arraySize; i++)
    {
        cin >> numbers[i];
    }

    int disorderedPairCount = examineOrderliness(numbers, arraySize);
    
    string orderLevel = classifyOrderlinessThreshhold(disorderedPairCount);

    cout << "Out-of-order adjacent pair count: "
         << disorderedPairCount << endl
         << "Orderliness Threshold is: "
         << orderLevel << endl;

    return 0;
}
```

Input:
1, 2, 3, 4, 5,
9, 8, 7, 6, 10,
11, 12, 13, 14, 20,
19, 18, 17, 16, 15,
21, 22, 23, 34, 24,
25, 26, 27, 29, 28,
30, 31, 32, 33, 34,
40, 39, 38, 37, 36,
35, 41, 42, 43, 44,
45, 56, 46, 47, 48

Output:
Out-of-order adjacent pair count: 16
Orderliness Threshold is: average

## Part C — Complexity of the Classification

## Part D — Documentation and Analysis

### Threshold Definition

### Threshold Justification

### Algorithm Selection

### Time Complexity

## Analysis and Reflection
