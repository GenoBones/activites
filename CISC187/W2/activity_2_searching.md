# Array Data Structure

## 1. Linear Search: [2, 4, 6, 8, 10, 12, 13] -> Target is 8

For linear search, it would take 4 comparisons in order to find the number 8. It starts at index zero and increments by one comparing values until it matches at the third index. 

## 2. Binary Search: [2, 4, 6, 8, 10, 12, 13] -> Target is 8

If you were to run a binary search on the same array, it would only take one comparison to find the match since it starts at the middle index. That is, floor((0 + (7 - 1)) / 2) = 3. Therefore, it would start at index 3, which is technically the 4th value in the array. Since 4 is smack dab in the middle of 7, and the value 8 is the middle value, only one comparison would need to be made.

## 3. Binary Search on a Large Dataset: array[100000]
The way binary search works is by looking at the middle value of an array (based on the equation given above) and then determining if that value is more or less than the value provided. If the middle value is less, every value including the middle value is discarded. If it’s more, every value above, including the middle value, is discarded. Then a new middle value is selected for comparison in the remaining set. This continues in a logarithmic reduction until the value is either found or determined not to exist within the set. Because of this, the maximum number would be log₂(100,000), which is around 16.61. Since you can’t do .61 of an operation, you would round up to 17 operations.


## 4. Linear Search vs. Binary Search

```cpp
#include <iostream>
#include <vector>
using namespace std;

const int lowValue = 800;
const int highValue = 80000;
const int nonexistentValue = -2;

void linearSearch(const vector<int>& orderedArray, int givenValue)
{
    int comparisonCount = 0;
    
    for (int i = 0; i < orderedArray.size(); i++)
    {
        comparisonCount ++;
        
        if (orderedArray[i] == givenValue) 
        {
            cout << "Linear search for number " << givenValue << " was found after " << comparisonCount << " comparisons. "
            << endl;
            
            return;
        }
        
    }
    cout << "Linear search for " << givenValue
     << " not found after " << comparisonCount << " comparisons."
     << endl;
}

void binarySearch(const vector<int>& orderedArray, int givenValue)
{
   int comparisonCount = 0;
   int bottomEnd = 0;
   int topEnd = orderedArray.size() - 1;
   
   int middleIndex;
   int middleValue;
   
   while (bottomEnd <= topEnd)
    {
        comparisonCount ++;
        
        middleIndex = (bottomEnd + topEnd) / 2;
        middleValue = orderedArray[middleIndex];
        
        if (middleValue < givenValue) 
        {
            bottomEnd = middleIndex + 1;
        } 
        
        else if (middleValue > givenValue)
        {
            topEnd = middleIndex - 1;
        }
        
        else
        {
            cout << "Binary search for " << givenValue
            << " found after " << comparisonCount << " comparisons."
            << endl;  
            
            return;
        }
    }
    cout << "Binary search for " << givenValue
     << " not found after " << comparisonCount << " comparisons."
     << endl;
}
int main()
{
    
    vector<int> orderedArray(100000);

    for (int i = 0; i < 100000; i++)
    {
        orderedArray[i] = i + 1;
    }
    
    linearSearch(orderedArray, lowValue);
    linearSearch(orderedArray, highValue);
    linearSearch(orderedArray, nonexistentValue);
    
    binarySearch(orderedArray, lowValue);
    binarySearch(orderedArray, highValue);
    binarySearch(orderedArray, nonexistentValue);
    
    return 0;
}
```

## Output for Task 4:

Linear search for number 800 was found after 800 comparisons.  
Linear search for number 80000 was found after 80000 comparisons.  
Linear search for -2 not found after 100000 comparisons.  
Binary search for 800 found after 16 comparisons.  
Binary search for 80000 found after 15 comparisons.  
Binary search for -2 not found after 16 comparisons.

### Why linear search is O(N)

The worst case time complexity for linear search is O(N) because it has to check every element in an array one at a time. If the value it is looking for is at the end of the array or doesn't exist it will have to go through each individual value starting at index 0. As the number of elements goes up, so does the max number of comparisons 1 to 1 at the same rate. For instance, I chose -2 as my int value that didn't exist and as you can see it took 100000 comparisons (vs 16 for binary) for the program to realize that. 

### Why binary search is O(log N):

Because each comparison reduces eliminates roughly half of the remaining search space the time complexity can be order of magnitude smaller (represented by the log). This is evident with the ordered set in my code taking no more than 16 comparisons to get an answer. I explained above how binary search functions.

### Why binary search requires sorted data while linear search does not:

Because binary search uses a middle value / given value comparison in order to determine which half of the remaining data to discard, it is absolutely dependent on ordered sets. Linear search in its nature does not have any such constraints as it checks every value one at time. 

## 5. Randomized Search:

### Part A — Pseudocode:

Create a vector with 100000 elements in ascending order called orderedArray

Create givenValue int to find within an array  

Run randomized search function  

Within randomized search:  
Create int called comparisonCount and set to 0  
New vector with 100000 elements is created called uncheckedIndices.  
Each elements in uncheckedIndices corresponds to its index number initially.  

While size of uncheckedIndices != 0  
Randomly select a position from uncheckedIndices
Pull the index value stored at that position
Pull the value from orderedArray at that index 
Increase comparisonCount by 1  
Compare givenValue to selected value  

if the givenValue is equals the selected value
Print that the value was found.
Print comparisonCount.
End the search.

else, replace the selected entry in uncheckedIndices with the last entry in uncheckedIndices.
remove last entry/index from uncheckedIndices

If uncheckedIndices becomes empty:
Print that the value was not found.
Print comparisonCount.

### Part B — Complexity Analysis:

The best case time complexity would be O(1) since there is technically a 1/100000 chance it gets the right comparison on the first comparison. The average case would still be O(N), much like linear search, because even though the indices are checked randomly it may still need to check a large portion of the array before finding the value. The worst case would also be O(N) because the target could be the final unchecked value, or not exist at all, which would require checking every element.

## Part C (++) — C++ Implementation

```cpp
#include <iostream>
#include <vector>
#include <random>
using namespace std;

void randSearch(const vector<int>& orderedArray, int givenValue)
{
    int comparisonCount = 0;

    vector<int> uncheckedIndices(orderedArray.size());
    for (int i = 0; i < uncheckedIndices.size(); i++)
    {
        uncheckedIndices[i] = i;
    } 
    
    random_device rd;
    mt19937 generator(rd());

    while (uncheckedIndices.size() > 0)
{
    int lastValidIndex = uncheckedIndices.size() - 1;

    uniform_int_distribution<int> validRange(0, lastValidIndex);

    int randomIndex = validRange(generator);

    int selectedIndex = uncheckedIndices[randomIndex];

    comparisonCount++;

    if (orderedArray[selectedIndex] == givenValue)
    {
        cout << "Value " << givenValue << " was found after " << comparisonCount << " comparisons." << endl;

        return;
    }
     uncheckedIndices[randomIndex] = uncheckedIndices[lastValidIndex];

        uncheckedIndices.pop_back();
}
    cout << "Given value " << givenValue << " was not found after " << comparisonCount << " comparisons." << endl;
}


int main()
{
    const int givenValue = 1337;
    const int arraySize = 100000;

    vector<int> orderedArray(arraySize);


    for (int i = 0; i < arraySize; i++)
    {
        orderedArray[i] = i + 1;
    }

     randSearch(orderedArray, givenValue);

    
    return 0;
}
```
## Output for Task 5:

Value 1337 was found after 69824 comparisons.

## Part D — Comparison

Between all of the different search types, I can safely say that binary search is the most efficient for an ordered array. If the array wasn't ordered, binary search would be pretty much useless without first sorting it. Linear search is the most versatile of these three as it would also work on an unordered array, albeit at an O(N) efficiency. Random search is also pretty versatile in the fact it can also be used on an unordered array, and in its best-case scenario, it can potentially have an O(1) efficiency, but on average, it has the same efficiency as linear search, which is O(N). The other downside to random search is the fact that a separate vector must be maintained for "book keeping", and the added complexity creates more opportunities for errors. I think the advantages are as follows: If you know an array is unsorted, both linear and random work; however, I would err on the side of linear just to reduce complexity and memory use. Random search does not gain an efficiency advantage simply by checking elements randomly, because after each unsuccessful comparison it has still only eliminated one possible element. Furthermore, since uncheckedIndices can contain N indices, the randomized implementation also requires O(N) additional memory in the form of the secondary "book keeping" array. For definitive ordered sets, binary search is almost always the most advantageous, with an average and worst-case efficiency of O(log N), regardless of the size of the data set. 
