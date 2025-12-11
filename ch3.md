# Chapter 3: Linear Data Structures

Linear data structures organize elements in a sequential manner where each element has exactly one predecessor and one successor (except for first and last elements). These structures form the foundation for many programming tasks.

## 3.1 Arrays

### 1D Array Structure and Operations
A one-dimensional array stores elements in consecutive memory locations, enabling direct access through index numbers. The array's structure allows O(1) random access - you can jump directly to any element using its index. Basic operations include reading elements, updating values, and iterating through all elements.

### 2D Array Structure and Operations
Two-dimensional arrays organize data in rows and columns, similar to a matrix or spreadsheet. Accessing elements requires two indices - row and column. They're useful for representing grid-like data, images, or mathematical matrices. Memory allocation is typically contiguous for the entire structure.

### Array Representation
Arrays can be implemented using contiguous memory blocks, where each element occupies a fixed size determined by the data type. This representation enables fast random access but can make resizing expensive since it may require allocating new memory and copying existing elements.

### Operations: Insertion, Deletion, Search, Traversal
- **Search**: Linear search O(n), binary search O(log n) on sorted arrays
- **Insertion**: O(n) in worst case (requires shifting elements)
- **Deletion**: O(n) due to element shifting
- **Traversal**: O(n) to visit all elements once

### Best Case and Worst Case Performance
- **Search**: Best O(1) if element is first, worst O(n) if it's last
- **Insertion**: Best O(1) if inserting at end, worst O(n) if inserting at beginning
- **Deletion**: Best O(1) if deleting last element, worst O(n) if deleting first

### Array vs Linked List Advantages
**Array Advantages**:
- O(1) random access
- Better cache performance due to memory locality
- Lower memory overhead
- Predictable performance characteristics

**Linked List Advantages**:
- O(1) insertion/deletion at known positions
- Dynamic size without pre-allocation
- Efficient for implementing stacks and queues

## 3.2 Linked Lists

### Singly Linked List Structure
A singly linked list consists of nodes where each node contains data and a reference to the next node. The first node is called the head, and the last node's next reference is null. Navigation can only proceed forward through the list.

### Doubly Linked List Structure
Doubly linked lists enhance singly linked lists by adding a previous pointer to each node. This enables bidirectional traversal but requires maintaining both next and previous references for each node, increasing memory usage.

### Node Representation
Nodes typically contain:
- **Data field**: The actual information being stored
- **Next pointer**: Reference to the following node (singly linked lists)
- **Previous pointer**: Reference to the preceding node (doubly linked lists)

### ADT (Abstract Data Type) for Linked Lists
The abstract data type defines the interface without implementation details. Common operations include:
- **addFirst()**: Insert element at the beginning
- **addLast()**: Insert element at the end
- **removeFirst()**: Delete first element
- **removeLast()**: Delete last element
- **contains()**: Check if element exists
- **size()**: Return number of elements

### Linked List Operations
Operations vary based on insertion/deletion location:
- **Head insertion/deletion**: O(1)
- **Tail insertion**: O(1) with tail pointer, O(n) without
- **Middle operations**: O(n) to locate position, then O(1) for actual operation

### Construction and Implementation
Implementation involves managing references correctly. Insertion requires updating pointers in the right order to maintain list integrity. Deletion requires proper memory management to prevent leaks.

### Insertion and Deletion Operations
Proper insertion maintains the list structure:
1. Create new node with data
2. Update new node's next pointer
3. Update previous node's next pointer (if applicable)

Deletion involves:
1. Locate the node to delete
2. Update previous node's next pointer
3. Remove references to the deleted node

### Application Areas of Linked Lists
Linked lists excel in scenarios requiring frequent insertions and deletions:
- **Dynamic memory allocation**
- **Undo functionality in applications**
- **Implementing other data structures (stacks, queues)**
- **Adjacency lists in graph representations**

### Advantages over Arrays
- **Dynamic sizing**: No need to pre-define size
- **Efficient insertions/deletions**: No element shifting required
- **Memory efficiency**: Only allocate memory when needed

### Main Applications of Linked Lists
- **Browser history navigation**
- **Music playlist management**
- **Operating system process scheduling**
- **Memory management systems**

## 3.3 Stacks

### Stack Structure and Concept
A stack operates on Last-In-First-Out (LIFO) principle. Think of it like a stack of plates - you can only add or remove from the top. This restriction makes stacks perfect for certain algorithms and applications where you need to remember previous states.

### Stack Operations (Push, Pop)
- **Push**: Adds element to the top of the stack
- **Pop**: Removes and returns the top element
- **Peek**: Returns top element without removing it
- **IsEmpty**: Checks if stack contains any elements

### Stack Implementation Using Arrays
Array implementation uses a fixed-size array with a top index pointer. Push increments the top index and stores the element. Pop returns the element at the top index and decrements the top index. This provides O(1) operations but has fixed maximum capacity.

### Stack ADT
The abstract stack interface provides these fundamental operations regardless of underlying implementation:
- **push(element)**: Add element to top
- **pop()**: Remove and return top element
- **peek()**: Return top element without removal
- **size()**: Return number of elements
- **isEmpty()**: Check if stack is empty

### LIFO (Last In First Out) Principle
LIFO means the last element added is the first one removed. This creates a natural "remember and return" behavior useful for backtracking, recursion simulation, and state management.

### Practical Uses of Stacks
Stacks solve problems where you need to remember previous states:
- **Function call management**: Track function parameters and return addresses
- **Expression evaluation**: Process mathematical expressions correctly
- **Undo functionality**: Remember previous states for rollback
- **Backtracking algorithms**: Navigate through solution spaces

### Application Areas of Stack Data Structures
- **Compilers**: Syntax analysis and code generation
- **Web browsers**: Back button functionality
- **Text editors**: Undo/redo operations
- **Graph algorithms**: Depth-first search traversal

### Real-World Applications
Everyday applications demonstrate stack utility:
- **Call stack in programming**: Manages function execution
- **Expression parsing**: Calculator operations
- **Memory management**: Stack-based allocation
- **Algorithm backtracking**: Sudoku solvers and maze navigation

## 3.4 Queues

### Queue Structure and Concept
Queues operate on First-In-First-Out (FIFO) principle, similar to a line at a bank teller. The first person in line is served first, and new arrivals join at the end. This makes queues ideal for managing sequential processing tasks.

### Queue Operations (Enqueue, Dequeue)
- **Enqueue**: Add element to the rear of the queue
- **Dequeue**: Remove and return element from the front
- **Front**: Return front element without removal
- **Rear**: Return rear element without removal
- **IsEmpty**: Check if queue contains any elements

### Queue Implementation
Queues can be implemented using arrays or linked lists. Array implementation uses two pointers (front and rear) to track boundaries. Linked list implementation maintains references to both head and tail nodes for efficient operations.

### FIFO (First In First Out) Principle
FIFO ensures orderly processing by serving elements in the order they were added. This principle is crucial for maintaining fairness and predictable behavior in systems handling multiple requests.

### Array-Based Queue Implementation
Array implementation uses a circular buffer approach to maximize space efficiency. When rear reaches the array's end, it wraps around to the beginning. This prevents "wasted" space that would occur with simple linear array implementation.

### Linked List-Based Queue Implementation
Linked list implementation naturally handles queue behavior:
- Enqueue creates new node at tail
- Dequeue removes node from head
- Both operations are O(1) when maintaining both head and tail references

### Advantages and Disadvantages of Implementations
**Array Implementation**:
- Advantages: O(1) operations, better cache performance, no memory overhead
- Disadvantages: Fixed capacity, potential wasted space, complex resizing

**Linked List Implementation**:
- Advantages: Dynamic sizing, simple resizing, no capacity limit
- Disadvantages: Higher memory overhead, worse cache performance, potential memory fragmentation

### Real-Life Applications of Queues
Queues model many real-world scenarios:
- **Print job scheduling**: Process documents in arrival order
- **CPU task scheduling**: Manage process execution priorities
- **Breadth-first search**: Explore graph levels systematically
- **Request handling**: Web servers processing user requests
- **Buffer management**: Smooth data flow between different processing speeds