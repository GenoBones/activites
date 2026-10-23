# Binary Heaps in C++ Lab

## Part 1 — Is It a Valid Max-Heap?  
A: A is valid because it fulfills the condition that each child is less than its parent and values are populated in the correct order - top to bottom left to right.   
B is invalid because it violates the complete-tree condition. 40 should be the left child, not the right.   
C: C is invalid because the left child of the root is greater than the parent. 

## Part 2 — Heap vs. Binary Search Tree  
1: Its a valid max heap because it fulfills the heap condition that each child is < the parent as well as the complete tree condition. 
2: It is not necessarily a Binary Search Tree because a heap does not require smaller values to be on the left and larger values to be on the right. It only requires each parent to be greater than its descendants.  
3: You cannot determine which direction to search based on the root
4: Arbitrary searching is generally O(N) because a heap does not tell us whether a value will be on the left or right, so we may need to inspect every node.  
5: The maximum value can be accessed immediately at the root.

## Part 3 — Represent a Heap Using an Array  
index 0: 90  
index 1: 70  
index 2: 80  
index 3: 20  
index 4: 40  
index 5: 60  
index 6: 50  

Index 1:  
Parent: 0  
Left child: 3  
Right child: 4  

Index 2:  
Parent: 0  
Left child: 5  
Right child: 6  

Index 4:  
Parent: 1  
Left child: does not exist  
Right child: does not exist

Index 5:  
Parent: 2  
Left child: does not exist  
Right child: does not exist  

## Part 4 — Why Use an Array? 
Using an array makes it easy to keep track of the last node because the last node will always be the final element in the array. When a new value is inserted, it is first added to the end of the array, which preserves the complete-tree condition. For the same reason, removing the last element does not leave a gap in the tree. With linked nodes, finding the last node would be more complicated because you would have to move through the tree to locate it instead of simply accessing the last array index.  

## Part 5 — Implement a MaxHeap Class   

## Part 6 — Implement the Index Functions  
1: The root does not have a valid parent because it is the top of the heap and is stored at index 0.  
2: A calculated child index exists if it is less than the size of the heap.  
3: These formulas work because the heap is a complete tree stored from top to bottom, left to right. The parent and child positions follow a predictable pattern in the array. 

## Part 7 — Heap Insertion  
```
Current Index | Parent Index | Current Value | Parent Value | Action  
7             | 3            | 85            | 20           | Swap  
3             | 1            | 85            | 70           | Swap  
1             | 0            | 85            | 90           | Stop  
```

```
     TREE DIAGRAM:  

          90  
        /    \  
      85      80  
     /  \    /  \  
   70   40  60   50  
  /  
20  
```

The new value has to be placed at the end of the array first because this preserves the complete-tree condition. Once it is added, it can trickle upward based on its value until the heap condition is restored. Putting it directly where it seems to belong based on value could leave gaps in the tree.

## Part 8 — Accessing the Maximum  
Reading the maximum is O(1) because the maximum value is always stored at the root, which is index 0 in the array. Removing the maximum is O(log N) because after removing the root, another value may need to trickle down through the levels of the heap to restore the heap condition.  

If the heap is empty, the function should handle it before trying to access index 0. In my implementation, it prints that the heap is empty and returns -1.

## Part 9 — Removing the Root

## Part 10 — Trickle-Down
```
Current Index | Left Child | Right Child | Larger Child | Action  
0             | 80         | 75          | 80           | Swap with 80  
1             | 60         | 70          | 70           | Swap with 70  
4             | None       | None        | None         | Stop  

     TREE DIAGRAM

          80  
        /    \  
      70      75  
     /  \    /  \  
   60   30  40   50  
  /  
20  
```
## Part 11 — Why the Larger Child?  
When trickling a value down, we compare it with the larger of its two children. If we swapped with the smaller child instead, the larger child could still be greater than the parent, which would leave the heap condition broken. Choosing the larger child makes sure the parent is greater than both children after the swap.  

## Part 12 — One-Child Edge Case  
If a node only has a left child, we only need to compare the current value with that child and swap them if the child is larger. In a complete binary heap, a node cannot have only a right child.  
  
If a node has no children, then it is a leaf node and there is nowhere else for it to trickle down, so the process stops.  

## Part 13 — Test the Complete Heap
```
#include <iostream>
#include <vector>

using namespace std;

class MaxHeap
{
private:
    vector<int> heap;

    int parentIndex(int index) const;
    int leftChildIndex(int index) const;
    int rightChildIndex(int index) const;

    void trickleUp(int index);
    void trickleDown(int index);

public:
    bool empty() const;
    int size() const;

    int maximum() const;

    void insert(int value);
    bool removeMax();

    void display() const;
};


int MaxHeap::parentIndex(int index) const
{
    return (index - 1) / 2;
}

int MaxHeap::leftChildIndex(int index) const
{
    return 2 * index + 1;
}

int MaxHeap::rightChildIndex(int index) const
{
    return 2 * index + 2;
}

void MaxHeap::insert(int value)
{
    heap.push_back(value);

    int newIndex = heap.size() - 1;

    trickleUp(newIndex);
}

void MaxHeap::trickleUp(int index)
{
    while (index > 0)
    {
        int parent = parentIndex(index);

        if (heap[index] <= heap[parent])
        {
            break;
        }

        int temp = heap[index];
        heap[index] = heap[parent];
        heap[parent] = temp;

        index = parent;
    }
}

bool MaxHeap::removeMax()
{
    if (heap.empty())
    {
        cout << "Heap is empty." << endl;
        return false;
    }

    if (heap.size() == 1)
    {
        heap.pop_back();
        return true;
    }

    heap[0] = heap[heap.size() - 1];

    heap.pop_back();

    trickleDown(0);

    return true;
}

void MaxHeap::trickleDown(int index)
{
    while (true)
    {
        int left = leftChildIndex(index);
        int right = rightChildIndex(index);

        if (left >= heap.size())
        {
            break;
        }

        int largerChild = left;

        if (right < heap.size() && heap[right] > heap[left])
        {
            largerChild = right;
        }

        if (heap[index] >= heap[largerChild])
        {
            break;
        }

        int temp = heap[index];
        heap[index] = heap[largerChild];
        heap[largerChild] = temp;

        index = largerChild;
    }
}

bool MaxHeap::empty() const
{
    return heap.empty();
}

int MaxHeap::size() const
{
    return heap.size();
}

int MaxHeap::maximum() const
{
    if (heap.empty())
    {
        cout << "Heap is empty." << endl;
        return -1;
    }

    return heap[0];
}

void MaxHeap::display() const
{
    if (heap.empty())
    {
        cout << "Heap is empty." << endl;
        return;
    }

    for (int i = 0; i < heap.size(); i++)
    {
        cout << heap[i] << " ";
    }

    cout << endl;
}

int main()
{
    MaxHeap heap;

    cout << "Initial heap:" << endl;
    heap.display();

    cout << endl;
    cout << "Inserting values:" << endl;

    heap.insert(90);
    heap.insert(70);
    heap.insert(80);
    heap.insert(20);
    heap.insert(40);
    heap.insert(60);
    heap.insert(50);

    heap.display();

    cout << endl;
    cout << "Inserting 85:" << endl;

    heap.insert(85);
    heap.display();

    cout << endl;
    cout << "Maximum value: " << heap.maximum() << endl;

    cout << endl;
    cout << "Removing maximum:" << endl;

    heap.removeMax();
    heap.display();

    cout << endl;
    cout << "Testing a one-element heap:" << endl;

    MaxHeap singleHeap;

    singleHeap.insert(42);
    singleHeap.display();

    cout << "Removing the only value:" << endl;

    singleHeap.removeMax();
    singleHeap.display();

    cout << endl;
    cout << "Trying to remove from an empty heap:" << endl;

    singleHeap.removeMax();

    return 0;
}
```

## Part 14 — Complexity Analysis  
Read maximum - O(1) - The maximum is always stored at the root at index 0, so it can be accessed directly.  
Insert - O(log N) - The new value may need to trickle up through the levels of the heap.  
Remove maximum - O(log N) - The value moved to the root may need to trickle down through the levels of the heap.  
Search for arbitrary value - O(N) - A heap does not tell us whether a value is on the left or right, so every value may need to be checked.  
Access last node in array representation - O(1) - The last node is always the final element in the array, so it can be accesse


## Part 15 — Heap vs. Ordered Array

A heap can still be better overall because it keeps both insertion and removal efficient. An ordered array can remove the highest-priority item in O(1), but inserting a new item can take O(N) because values may need to be shifted. A heap keeps both insertion and removal at O(log N), which makes it more efficient when items are constantly being added and processed.  

## Part 16 — Priority Queue Reasoning  

Final Heap: [10, 9, 2, 4, 7]
```
FINAL HEAP DIAGRAM  

        10  
       /  \  
      9    2  
     / \  
    4   7  
```
Task E is processed first because it has the highest priority. After removing 10, task B is processed next.  

A heap works well for a priority queue because the highest-priority item is always at the root. Insertion and removal are both O(log N).  


## Analysis and Reflection

1: A heap must be complete so the nodes can be stored in an array without gaps and the parent/child index formulas will work correctly.  
2: A max-heap is not the same as a Binary Search Tree because a heap only requires each parent to be greater than its children. It does not require smaller values on the left and larger values on the right.  
3: Arbitrary search is inefficient because the heap does not tell us which side a value will be on, so we may need to check every value.  
4: The root gives immediate access to the highest-priority value because the maximum value is always stored at index 0.  
5: Insertion starts at the end of the array to preserve the complete-tree structure. The new value then trickles up if needed.  
6: Deletion replaces the root with the last node because removing the last array element preserves the complete-tree structure. The new root can then trickle down.  
7: Trickle-down chooses the larger child so the parent will remain greater than both children after a swap.  
8: Insertion and removal are O(log N) because a value only needs to move through the levels of the heap.  
9: The heap's weak ordering is useful for priority queues because we do not need every value fully sorted. We mainly need quick access to the highest-priority value while keeping insertion and removal efficient.  
