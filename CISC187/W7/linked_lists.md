# Singly Linked Lists in C++

## Part 1 — Understanding Nodes and Pointers

### Linked List Diagram
head  -> first
first -> data = 7
first -> nextptr -> second

second->data = 13
second -> nextptr -> third

third -> data = 21
third -> nextptr = nullptr

### Analysis
1: No. head does not contain the value 7 - it points to the node that contains 7.  
2: head stores the memory address of the first node.  
3: first -> nextptr stores the address of the second node.  
4: third -> nextptr contains nullptr, meaning there is no node after it.  

---

## Part 2 — Trace a Linked List

### Trace
1: nxtptr->data = 10 | nxtptr->next points to 20
2: nxtptr->data = 20 | nxtptr->next points to 30
3: nxtptr->data = 30 | nxtptr->next points to 40
4: nxtptr->data = 40 | nxtptr->next points to nullptr

The traversal stops when nxtptr becomes nullptr, because there are no more nodes to visit.

### Complexity  
O(N) because each node is visited once.

---

## Part 3 — Build a LinkedList Class

...

---

## Part 4 — Initialize the List

### Analysis
1: nullptr is appropriate for an empty linked list because there is no first node for head to point to. Once a node is added, head will store the address of that first node instead.  

---

## Part 5 — Insert at the Beginning

### Pointer Analysis
If you update head to point to the new node before setting newNodePtr->next to the old head, you lose access to the old first node and the rest of the list.  

### Complexity

---

## Part 6 — Insert at the End

### Analysis

1: Because in order to get to the last node, we need to travers the entire linkedlist looking for the node where next == nullptr.  
2: If we maintain a tail pointer, we can immediately access the end of the list, set tail->next to newNodePtr, and then update tail to point to the new node.   

---

## Part 7 — Search the List

### Analysis  

1: Best case is O(1) if the value is in the first node.  
2: Worst case is O(N) if the value is at the end or is not in the list, because we may need to check every node.  

---

## Part 8 — Insert After a Specific Value

### insertAfter()

### Pointer Reasoning  
1: We have to set the new node’s next pointer first so we do not lose the rest of the list. Then we can safely make the target node point to the new node.  

### Complexity  
2: insertAfter() is O(N) because we may need to search through the list to find the target node. Once the target is found, inserting the new node itself is O(1).  

---

## Part 9 — Delete a Node

### Analysis  
1: Deleting the head requires special handling because head must be updated to point to the next node before the old first node is deleted.  
2: We must save access to the node before deleting it so we can reconnect the remaining list and safely release its memory.  
3: A memory leak happens when allocated memory is never released with delete and can no longer be accessed.  
4: Accessing a node after delete is unsafe because that memory has already been released and is no longer valid to use.  
 
---

## Part 10 — Test the Complete Linked List

### Complete Program   
```
#include <iostream>
using namespace std;

struct Node
{
    int data;
    Node* next;
};

class LinkedList
{
private:
    Node* head;

public:
    LinkedList();

    bool empty() const;
    void display() const;
    bool search(int value) const;

    void insertFront(int value);
    void insertBack(int value);
    bool insertAfter(int target, int value);

    bool remove(int value);

    ~LinkedList();
};

LinkedList::LinkedList()
{
    head = nullptr;
}

bool LinkedList::empty() const
{
    return head == nullptr;
}

void LinkedList::display() const
{
    Node* currentPtr = head;

    while (currentPtr != nullptr)
    {
        cout << currentPtr->data << " -> ";
        currentPtr = currentPtr->next;
    }

    cout << "nullptr" << endl;
}

void LinkedList::insertFront(int value)
{
    Node* newNodePtr = new Node;

    newNodePtr->data = value;
    newNodePtr->next = head;

    head = newNodePtr;
}

void LinkedList::insertBack(int value)
{
    Node* newNodePtr = new Node;

    newNodePtr->data = value;
    newNodePtr->next = nullptr;

    if (empty())
    {
        head = newNodePtr;
    }
    else
    {
        Node* currentPtr = head;

        while (currentPtr->next != nullptr)
        {
            currentPtr = currentPtr->next;
        }

        currentPtr->next = newNodePtr;
    }
}

bool LinkedList::search(int value) const
{
    Node* currentPtr = head;

    while (currentPtr != nullptr)
    {
        if (currentPtr->data == value)
        {
            return true;
        }

        currentPtr = currentPtr->next;
    }

    return false;
}

bool LinkedList::insertAfter(int target, int value)
{
    Node* currentPtr = head;

    while (currentPtr != nullptr)
    {
        if (currentPtr->data == target)
        {
            Node* newNodePtr = new Node;

            newNodePtr->data = value;
            newNodePtr->next = currentPtr->next;

            currentPtr->next = newNodePtr;

            return true;
        }

        currentPtr = currentPtr->next;
    }

    return false;
}

bool LinkedList::remove(int value)
{
    if (empty())
    {
        return false;
    }

    if (head->data == value)
    {
        Node* deletePtr = head;
        head = head->next;

        delete deletePtr;

        return true;
    }

    Node* previousPtr = head;
    Node* currentPtr = head->next;

    while (currentPtr != nullptr)
    {
        if (currentPtr->data == value)
        {
            previousPtr->next = currentPtr->next;

            delete currentPtr;

            return true;
        }

        previousPtr = currentPtr;
        currentPtr = currentPtr->next;
    }

    return false;
}

LinkedList::~LinkedList()
{
    Node* currentPtr = head;

    while (currentPtr != nullptr)
    {
        Node* deletePtr = currentPtr;
        currentPtr = currentPtr->next;

        delete deletePtr;
    }
}

int main()
{
    LinkedList list;

    cout << boolalpha;

    cout << "Empty list:" << endl;
    cout << "Empty: " << list.empty() << endl;
    list.display();

    cout << endl;

    cout << "Insert values at front:" << endl;
    list.insertFront(20);
    list.display();

    list.insertFront(10);
    list.display();

    cout << endl;

    cout << "Insert values at back:" << endl;
    list.insertBack(30);
    list.display();

    list.insertBack(40);
    list.display();

    cout << endl;

    cout << "Search tests:" << endl;
    cout << "Search for 20: " << list.search(20) << endl;
    cout << "Search for 99: " << list.search(99) << endl;

    cout << endl;

    cout << "Insert 25 after 20:" << endl;
    cout << "Inserted: " << list.insertAfter(20, 25) << endl;
    list.display();

    cout << endl;

    cout << "Attempt insert after nonexistent value 99:" << endl;
    cout << "Inserted: " << list.insertAfter(99, 100) << endl;
    list.display();

    cout << endl;

    cout << "Remove first node:" << endl;
    cout << "Removed: " << list.remove(10) << endl;
    list.display();

    cout << endl;

    cout << "Remove middle node:" << endl;
    cout << "Removed: " << list.remove(25) << endl;
    list.display();

    cout << endl;

    cout << "Remove last node:" << endl;
    cout << "Removed: " << list.remove(40) << endl;
    list.display();

    cout << endl;

    cout << "Attempt to remove nonexistent value 99:" << endl;
    cout << "Removed: " << list.remove(99) << endl;
    list.display();

    cout << endl;

    cout << "Final list:" << endl;
    list.display();

    return 0;
}
```

### Test Results   

Empty list:  
Empty: true  
nullptr  

Insert values at front:  
20 -> nullptr  
10 -> 20 -> nullptr  

Insert values at back:  
10 -> 20 -> 30 -> nullptr  
10 -> 20 -> 30 -> 40 -> nullptr  

Search tests:  
Search for 20: true  
Search for 99: false  
 
Insert 25 after 20:  
Inserted: true  
10 -> 20 -> 25 -> 30 -> 40 -> nullptr  

Attempt insert after nonexistent value 99:  
Inserted: false  
10 -> 20 -> 25 -> 30 -> 40 -> nullptr  

Remove first node:  
Removed: true  
20 -> 25 -> 30 -> 40 -> nullptr  

Remove middle node:  
Removed: true  
20 -> 30 -> 40 -> nullptr  

Remove last node:  
Removed: true  
20 -> 30 -> nullptr  

Attempt to remove nonexistent value 99:  
Removed: false  
20 -> 30 -> nullptr  

Final list:  
20 -> 30 -> nullptr  
---

## Part 11 — Memory Management  

### Analysis  

1: If nodes are created with new but never deleted, the allocated memory stays reserved even after it is no longer needed.  
2: This is a memory leak because the program keeps losing memory without releasing it.  
3: Memory management is important with raw pointers because memory created with new must eventually be released with delete. The destructor   makes sure any remaining nodes are deleted when the linked list is destroyed.  
 
---

## Part 12 — Complexity Analysis  

### Operation Complexity  

Access element at position i: O(N) | Must traverse from head until reaching position i.  

Search: O(N) | May need to check every node before finding the value or reaching nullptr.  

Insert at beginning: O(1) | Only updates the new node’s next pointer and head.  

Insert at end: O(N) | Must traverse the list to find the last node because there is no tail pointer.  
 
Insert after a specified value: O(N) | Must first search for the target node, even though the actual pointer insertion is O(1).  

Delete first node: O(1) | Only moves head to the next node and deletes the old first node.  

Delete specified value: O(N) | May need to traverse the list to find the node that should be removed.  

Traverse entire list: O(N) | Every node must be visited once.  

---

## Part 13 — Linked List vs. Array  

### Comparison  

### Analysis  

1: Linked lists can insert or delete nodes efficiently because they can change pointers instead of shifting many elements. However, they cannot provide O(1) random access because you must start at head and follow each next pointer until reaching the desired node.
    
---

## Part 14 — Reasoning About a Tail Pointer  

### Analysis  

---

## Part 15 — Singly vs. Doubly Linked Lists  

### Analysis  

---

## Part 16 — Circular Linked Lists  
 
### Analysis  

---

# Analysis and Reflection

1:

2:

3:

4:

5:

6:

7:

8:

9:

10:
