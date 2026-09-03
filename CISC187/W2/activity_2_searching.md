1.) [2, 4, 6, 8, 10, 12, 13] -> Target is 8

For linear search, it would take 4 comparisons in order to find the number 8. It starts at index zero and increments by one comparing values until it matches at the third index. 

2.) If you were to run a binary search on the same array, it would only take one comparison to find the match since it starts at the middle index. That is, floor((0 + (7 - 1)) / 2) = 3. Therefore, it would start at index 3, which is technically the 4th value in the array. Since 4 is smack dab in the middle of 7, and the value 8 is the middle value, only one comparison would need to be made.

3.) The way binary search works is by looking at the middle value of an array (based on the equation given above) and then determining if that value is more or less than the value provided. If the middle value is less, every value including the middle value is discarded. If it’s more, every value above, including the middle value, is discarded. Then a new middle value is selected for comparison in the remaining set. This continues in a logarithmic reduction until the value is either found or determined not to exist within the set. Because of this, the maximum number would be log₂(100,000), which is around 16.61. Since you can’t do .61 of an operation, you would round up to 17 operations.

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
}```
