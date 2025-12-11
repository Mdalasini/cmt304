# Chapter 6: Python Implementation of Data Structures

This chapter provides comprehensive Python implementations of the data structures discussed in Chapters 3 and 4, demonstrating practical applications and performance characteristics.

## 6.1 Array Implementations

### Dynamic Array Class
```python
class DynamicArray:
    """Dynamic array implementation with automatic resizing"""
    
    def __init__(self, capacity=10):
        self._capacity = capacity
        self._size = 0
        self._data = [None] * self._capacity
    
    def __len__(self):
        return self._size
    
    def __getitem__(self, index):
        """Get element at index - O(1)"""
        if not 0 <= index < self._size:
            raise IndexError("Array index out of range")
        return self._data[index]
    
    def __setitem__(self, index, value):
        """Set element at index - O(1)"""
        if not 0 <= index < self._size:
            raise IndexError("Array index out of range")
        self._data[index] = value
    
    def append(self, value):
        """Add element to end - O(1) amortized"""
        if self._size >= self._capacity:
            self._resize(2 * self._capacity)
        self._data[self._size] = value
        self._size += 1
    
    def insert(self, index, value):
        """Insert element at index - O(n)"""
        if not 0 <= index <= self._size:
            raise IndexError("Array index out of range")
        
        if self._size >= self._capacity:
            self._resize(2 * self._capacity)
        
        # Shift elements to make room
        for i in range(self._size, index, -1):
            self._data[i] = self._data[i - 1]
        
        self._data[index] = value
        self._size += 1
    
    def delete(self, index):
        """Delete element at index - O(n)"""
        if not 0 <= index < self._size:
            raise IndexError("Array index out of range")
        
        # Shift elements to fill gap
        for i in range(index, self._size - 1):
            self._data[i] = self._data[i + 1]
        
        self._size -= 1
        self._data[self._size] = None
    
    def search(self, value):
        """Linear search - O(n)"""
        for i in range(self._size):
            if self._data[i] == value:
                return i
        return -1
    
    def binary_search(self, value):
        """Binary search on sorted array - O(log n)"""
        left, right = 0, self._size - 1
        
        while left <= right:
            mid = (left + right) // 2
            if self._data[mid] == value:
                return mid
            elif self._data[mid] < value:
                left = mid + 1
            else:
                right = mid - 1
        
        return -1
    
    def _resize(self, new_capacity):
        """Resize internal array"""
        new_data = [None] * new_capacity
        for i in range(self._size):
            new_data[i] = self._data[i]
        self._data = new_data
        self._capacity = new_capacity
    
    def __str__(self):
        return str([self._data[i] for i in range(self._size)])

# Example usage
arr = DynamicArray()
arr.append(10)
arr.append(20)
arr.append(30)
print(f"Array: {arr}")  # [10, 20, 30]
arr.insert(1, 15)
print(f"After insertion: {arr}")  # [10, 15, 20, 30]
arr.delete(2)
print(f"After deletion: {arr}")  # [10, 15, 30]
```

### 2D Array Implementation
```python
class Matrix2D:
    """2D array implementation for matrix operations"""
    
    def __init__(self, rows, cols, default_value=0):
        self.rows = rows
        self.cols = cols
        self.data = [[default_value for _ in range(cols)] for _ in range(rows)]
    
    def __getitem__(self, indices):
        """Get element at [row, col] - O(1)"""
        row, col = indices
        if not (0 <= row < self.rows and 0 <= col < self.cols):
            raise IndexError("Matrix index out of range")
        return self.data[row][col]
    
    def __setitem__(self, indices, value):
        """Set element at [row, col] - O(1)"""
        row, col = indices
        if not (0 <= row < self.rows and 0 <= col < self.cols):
            raise IndexError("Matrix index out of range")
        self.data[row][col] = value
    
    def transpose(self):
        """Transpose matrix - O(rows * cols)"""
        result = Matrix2D(self.cols, self.rows)
        for i in range(self.rows):
            for j in range(self.cols):
                result[j, i] = self[i, j]
        return result
    
    def add(self, other):
        """Matrix addition - O(rows * cols)"""
        if self.rows != other.rows or self.cols != other.cols:
            raise ValueError("Matrices must have same dimensions")
        
        result = Matrix2D(self.rows, self.cols)
        for i in range(self.rows):
            for j in range(self.cols):
                result[i, j] = self[i, j] + other[i, j]
        return result
    
    def __str__(self):
        return '\n'.join([' '.join([str(self.data[i][j]) for j in range(self.cols)]) 
                         for i in range(self.rows)])

# Example usage
matrix = Matrix2D(3, 3)
matrix[0, 0] = 1
matrix[1, 1] = 2
matrix[2, 2] = 3
print("Matrix:")
print(matrix)
print(f"Element at [1,1]: {matrix[1, 1]}")  # 2
```

## 6.2 Linked List Implementations

### Singly Linked List
```python
class Node:
    """Node for singly linked list"""
    def __init__(self, data):
        self.data = data
        self.next = None

class SinglyLinkedList:
    """Singly linked list implementation"""
    
    def __init__(self):
        self.head = None
        self.size = 0
    
    def __len__(self):
        return self.size
    
    def is_empty(self):
        """Check if list is empty - O(1)"""
        return self.head is None
    
    def add_first(self, data):
        """Add element at beginning - O(1)"""
        new_node = Node(data)
        new_node.next = self.head
        self.head = new_node
        self.size += 1
    
    def add_last(self, data):
        """Add element at end - O(n)"""
        new_node = Node(data)
        
        if self.is_empty():
            self.head = new_node
        else:
            current = self.head
            while current.next:
                current = current.next
            current.next = new_node
        
        self.size += 1
    
    def remove_first(self):
        """Remove first element - O(1)"""
        if self.is_empty():
            raise IndexError("List is empty")
        
        data = self.head.data
        self.head = self.head.next
        self.size -= 1
        return data
    
    def remove_last(self):
        """Remove last element - O(n)"""
        if self.is_empty():
            raise IndexError("List is empty")
        
        if self.head.next is None:
            data = self.head.data
            self.head = None
        else:
            current = self.head
            while current.next.next:
                current = current.next
            data = current.next.data
            current.next = None
        
        self.size -= 1
        return data
    
    def contains(self, data):
        """Search for element - O(n)"""
        current = self.head
        while current:
            if current.data == data:
                return True
            current = current.next
        return False
    
    def insert_at(self, index, data):
        """Insert element at specific index - O(n)"""
        if index < 0 or index > self.size:
            raise IndexError("Index out of range")
        
        if index == 0:
            self.add_first(data)
        else:
            new_node = Node(data)
            current = self.head
            for _ in range(index - 1):
                current = current.next
            new_node.next = current.next
            current.next = new_node
            self.size += 1
    
    def delete_at(self, index):
        """Delete element at specific index - O(n)"""
        if index < 0 or index >= self.size:
            raise IndexError("Index out of range")
        
        if index == 0:
            return self.remove_first()
        else:
            current = self.head
            for _ in range(index - 1):
                current = current.next
            data = current.next.data
            current.next = current.next.next
            self.size -= 1
            return data
    
    def __str__(self):
        elements = []
        current = self.head
        while current:
            elements.append(str(current.data))
            current = current.next
        return " -> ".join(elements)

# Example usage
sll = SinglyLinkedList()
sll.add_first(30)
sll.add_first(20)
sll.add_first(10)
print(f"Linked List: {sll}")  # 10 -> 20 -> 30
sll.add_last(40)
print(f"After adding 40: {sll}")  # 10 -> 20 -> 30 -> 40
sll.insert_at(2, 25)
print(f"After inserting 25 at index 2: {sll}")  # 10 -> 20 -> 25 -> 30 -> 40
```

### Doubly Linked List
```python
class DoublyNode:
    """Node for doubly linked list"""
    def __init__(self, data):
        self.data = data
        self.next = None
        self.prev = None

class DoublyLinkedList:
    """Doubly linked list implementation"""
    
    def __init__(self):
        self.head = None
        self.tail = None
        self.size = 0
    
    def __len__(self):
        return self.size
    
    def is_empty(self):
        """Check if list is empty - O(1)"""
        return self.head is None
    
    def add_first(self, data):
        """Add element at beginning - O(1)"""
        new_node = DoublyNode(data)
        
        if self.is_empty():
            self.head = self.tail = new_node
        else:
            new_node.next = self.head
            self.head.prev = new_node
            self.head = new_node
        
        self.size += 1
    
    def add_last(self, data):
        """Add element at end - O(1)"""
        new_node = DoublyNode(data)
        
        if self.is_empty():
            self.head = self.tail = new_node
        else:
            new_node.prev = self.tail
            self.tail.next = new_node
            self.tail = new_node
        
        self.size += 1
    
    def remove_first(self):
        """Remove first element - O(1)"""
        if self.is_empty():
            raise IndexError("List is empty")
        
        data = self.head.data
        
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.head = self.head.next
            self.head.prev = None
        
        self.size -= 1
        return data
    
    def remove_last(self):
        """Remove last element - O(1)"""
        if self.is_empty():
            raise IndexError("List is empty")
        
        data = self.tail.data
        
        if self.head == self.tail:
            self.head = self.tail = None
        else:
            self.tail = self.tail.prev
            self.tail.next = None
        
        self.size -= 1
        return data
    
    def traverse_forward(self):
        """Forward traversal - O(n)"""
        elements = []
        current = self.head
        while current:
            elements.append(current.data)
            current = current.next
        return elements
    
    def traverse_backward(self):
        """Backward traversal - O(n)"""
        elements = []
        current = self.tail
        while current:
            elements.append(current.data)
            current = current.prev
        return elements
    
    def __str__(self):
        return " <-> ".join(map(str, self.traverse_forward()))

# Example usage
dll = DoublyLinkedList()
dll.add_first(30)
dll.add_first(20)
dll.add_first(10)
print(f"Doubly Linked List: {dll}")  # 10 <-> 20 <-> 30
dll.add_last(40)
print(f"Forward: {dll.traverse_forward()}")  # [10, 20, 30, 40]
print(f"Backward: {dll.traverse_backward()}")  # [40, 30, 20, 10]
```

## 6.3 Stack Implementation

```python
class ArrayStack:
    """Stack implementation using dynamic array"""
    
    def __init__(self):
        self.data = DynamicArray()
    
    def __len__(self):
        return len(self.data)
    
    def is_empty(self):
        """Check if stack is empty - O(1)"""
        return len(self.data) == 0
    
    def push(self, element):
        """Add element to top - O(1) amortized"""
        self.data.append(element)
    
    def pop(self):
        """Remove and return top element - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.data.delete(len(self.data) - 1)
    
    def peek(self):
        """Return top element without removal - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.data[len(self.data) - 1]
    
    def __str__(self):
        return str(self.data)

class LinkedListStack:
    """Stack implementation using linked list"""
    
    def __init__(self):
        self.list = SinglyLinkedList()
    
    def __len__(self):
        return len(self.list)
    
    def is_empty(self):
        """Check if stack is empty - O(1)"""
        return self.list.is_empty()
    
    def push(self, element):
        """Add element to top - O(1)"""
        self.list.add_first(element)
    
    def pop(self):
        """Remove and return top element - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.list.remove_first()
    
    def peek(self):
        """Return top element without removal - O(1)"""
        if self.is_empty():
            raise IndexError("Stack is empty")
        return self.list.head.data
    
    def __str__(self):
        return str(self.list)

# Example usage - Expression evaluation
stack = ArrayStack()
expression = "3 4 + 2 * 7 -"
tokens = expression.split()

for token in tokens:
    if token.isdigit():
        stack.push(int(token))
    else:
        # Operator: pop two operands and apply operation
        b = stack.pop()
        a = stack.pop()
        if token == '+':
            stack.push(a + b)
        elif token == '-':
            stack.push(a - b)
        elif token == '*':
            stack.push(a * b)
        elif token == '/':
            stack.push(a / b)

result = stack.pop()
print(f"Result of '{expression}': {result}")  # 13
```

## 6.4 Queue Implementation

```python
class ArrayQueue:
    """Queue implementation using circular array"""
    
    def __init__(self, capacity=10):
        self.capacity = capacity
        self.data = [None] * capacity
        self.front = 0
        self.rear = 0
        self.size = 0
    
    def __len__(self):
        return self.size
    
    def is_empty(self):
        """Check if queue is empty - O(1)"""
        return self.size == 0
    
    def is_full(self):
        """Check if queue is full - O(1)"""
        return self.size == self.capacity
    
    def enqueue(self, element):
        """Add element to rear - O(1)"""
        if self.is_full():
            self._resize(2 * self.capacity)
        
        self.data[self.rear] = element
        self.rear = (self.rear + 1) % self.capacity
        self.size += 1
    
    def dequeue(self):
        """Remove and return front element - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        
        element = self.data[self.front]
        self.data[self.front] = None
        self.front = (self.front + 1) % self.capacity
        self.size -= 1
        return element
    
    def front_element(self):
        """Return front element without removal - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.data[self.front]
    
    def rear_element(self):
        """Return rear element without removal - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.data[(self.rear - 1) % self.capacity]
    
    def _resize(self, new_capacity):
        """Resize circular array"""
        new_data = [None] * new_capacity
        
        # Copy elements in order
        for i in range(self.size):
            new_data[i] = self.data[(self.front + i) % self.capacity]
        
        self.data = new_data
        self.front = 0
        self.rear = self.size
        self.capacity = new_capacity
    
    def __str__(self):
        elements = []
        for i in range(self.size):
            elements.append(str(self.data[(self.front + i) % self.capacity]))
        return " <- ".join(elements)

class LinkedListQueue:
    """Queue implementation using linked list"""
    
    def __init__(self):
        self.list = DoublyLinkedList()
    
    def __len__(self):
        return len(self.list)
    
    def is_empty(self):
        """Check if queue is empty - O(1)"""
        return self.list.is_empty()
    
    def enqueue(self, element):
        """Add element to rear - O(1)"""
        self.list.add_last(element)
    
    def dequeue(self):
        """Remove and return front element - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.list.remove_first()
    
    def front_element(self):
        """Return front element without removal - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.list.head.data
    
    def rear_element(self):
        """Return rear element without removal - O(1)"""
        if self.is_empty():
            raise IndexError("Queue is empty")
        return self.list.tail.data
    
    def __str__(self):
        return str(self.list)

# Example usage - Task scheduling
queue = ArrayQueue()
tasks = ["Task A", "Task B", "Task C", "Task D"]

print("Adding tasks to queue:")
for task in tasks:
    queue.enqueue(task)
    print(f"Enqueued: {task}")

print(f"\nQueue: {queue}")
print(f"Front task: {queue.front_element()}")
print(f"Rear task: {queue.rear_element()}")

print("\nProcessing tasks:")
while not queue.is_empty():
    task = queue.dequeue()
    print(f"Processing: {task}")
```

## 6.5 Tree Implementations

### Binary Tree Node and Basic Structure
```python
class TreeNode:
    """Node for binary tree"""
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None

class BinaryTree:
    """Basic binary tree implementation"""
    
    def __init__(self):
        self.root = None
        self.size = 0
    
    def __len__(self):
        return self.size
    
    def is_empty(self):
        """Check if tree is empty - O(1)"""
        return self.root is None
    
    def height(self):
        """Calculate tree height - O(n)"""
        return self._height_recursive(self.root)
    
    def _height_recursive(self, node):
        """Helper method for height calculation"""
        if node is None:
            return -1
        
        left_height = self._height_recursive(node.left)
        right_height = self._height_recursive(node.right)
        return max(left_height, right_height) + 1
    
    def count_nodes(self):
        """Count total nodes - O(n)"""
        return self._count_recursive(self.root)
    
    def _count_recursive(self, node):
        """Helper method for node counting"""
        if node is None:
            return 0
        return 1 + self._count_recursive(node.left) + self._count_recursive(node.right)
    
    def count_leaves(self):
        """Count leaf nodes - O(n)"""
        return self._count_leaves_recursive(self.root)
    
    def _count_leaves_recursive(self, node):
        """Helper method for leaf counting"""
        if node is None:
            return 0
        if node.left is None and node.right is None:
            return 1
        return self._count_leaves_recursive(node.left) + self._count_leaves_recursive(node.right)
    
    def preorder_traversal(self):
        """Preorder traversal: Root -> Left -> Right - O(n)"""
        result = []
        self._preorder_recursive(self.root, result)
        return result
    
    def _preorder_recursive(self, node, result):
        """Helper method for preorder traversal"""
        if node:
            result.append(node.data)
            self._preorder_recursive(node.left, result)
            self._preorder_recursive(node.right, result)
    
    def inorder_traversal(self):
        """Inorder traversal: Left -> Root -> Right - O(n)"""
        result = []
        self._inorder_recursive(self.root, result)
        return result
    
    def _inorder_recursive(self, node, result):
        """Helper method for inorder traversal"""
        if node:
            self._inorder_recursive(node.left, result)
            result.append(node.data)
            self._inorder_recursive(node.right, result)
    
    def postorder_traversal(self):
        """Postorder traversal: Left -> Right -> Root - O(n)"""
        result = []
        self._postorder_recursive(self.root, result)
        return result
    
    def _postorder_recursive(self, node, result):
        """Helper method for postorder traversal"""
        if node:
            self._postorder_recursive(node.left, result)
            self._postorder_recursive(node.right, result)
            result.append(node.data)
    
    def level_order_traversal(self):
        """Level-order traversal using queue - O(n)"""
        if self.is_empty():
            return []
        
        result = []
        queue = ArrayQueue()
        queue.enqueue(self.root)
        
        while not queue.is_empty():
            node = queue.dequeue()
            result.append(node.data)
            
            if node.left:
                queue.enqueue(node.left)
            if node.right:
                queue.enqueue(node.right)
        
        return result
    
    def find(self, value):
        """Search for value in tree - O(n)"""
        return self._find_recursive(self.root, value)
    
    def _find_recursive(self, node, value):
        """Helper method for finding value"""
        if node is None:
            return False
        if node.data == value:
            return True
        return self._find_recursive(node.left, value) or self._find_recursive(node.right, value)

# Example usage - Building a sample tree
bt = BinaryTree()
bt.root = TreeNode('A')
bt.root.left = TreeNode('B')
bt.root.right = TreeNode('C')
bt.root.left.left = TreeNode('D')
bt.root.left.right = TreeNode('E')
bt.root.right.left = TreeNode('F')

print(f"Tree height: {bt.height()}")  # 2
print(f"Total nodes: {bt.count_nodes()}")  # 6
print(f"Leaf nodes: {bt.count_leaves()}")  # 3
print(f"Preorder: {bt.preorder_traversal()}")  # ['A', 'B', 'D', 'E', 'C', 'F']
print(f"Inorder: {bt.inorder_traversal()}")  # ['D', 'B', 'E', 'A', 'F', 'C']
print(f"Postorder: {bt.postorder_traversal()}")  # ['D', 'E', 'B', 'F', 'C', 'A']
print(f"Level-order: {bt.level_order_traversal()}")  # ['A', 'B', 'C', 'D', 'E', 'F']
```

### Binary Search Tree (BST) Implementation
```python
class BSTNode:
    """Node for binary search tree"""
    def __init__(self, data):
        self.data = data
        self.left = None
        self.right = None
        self.parent = None

class BinarySearchTree:
    """Binary Search Tree implementation"""
    
    def __init__(self):
        self.root = None
        self.size = 0
    
    def __len__(self):
        return self.size
    
    def is_empty(self):
        """Check if BST is empty - O(1)"""
        return self.root is None
    
    def insert(self, value):
        """Insert value maintaining BST property - O(log n) average, O(n) worst"""
        if self.is_empty():
            self.root = BSTNode(value)
        else:
            self._insert_recursive(self.root, value)
        self.size += 1
    
    def _insert_recursive(self, node, value):
        """Helper method for insertion"""
        if value < node.data:
            if node.left is None:
                node.left = BSTNode(value)
                node.left.parent = node
            else:
                self._insert_recursive(node.left, value)
        else:  # value >= node.data
            if node.right is None:
                node.right = BSTNode(value)
                node.right.parent = node
            else:
                self._insert_recursive(node.right, value)
    
    def search(self, value):
        """Search for value - O(log n) average, O(n) worst"""
        return self._search_recursive(self.root, value)
    
    def _search_recursive(self, node, value):
        """Helper method for searching"""
        if node is None:
            return False
        if value == node.data:
            return True
        elif value < node.data:
            return self._search_recursive(node.left, value)
        else:
            return self._search_recursive(node.right, value)
    
    def find_min(self):
        """Find minimum value - O(log n)"""
        if self.is_empty():
            raise IndexError("BST is empty")
        return self._find_min_node(self.root).data
    
    def _find_min_node(self, node):
        """Helper method to find minimum node"""
        current = node
        while current.left:
            current = current.left
        return current
    
    def find_max(self):
        """Find maximum value - O(log n)"""
        if self.is_empty():
            raise IndexError("BST is empty")
        return self._find_max_node(self.root).data
    
    def _find_max_node(self, node):
        """Helper method to find maximum node"""
        current = node
        while current.right:
            current = current.right
        return current
    
    def delete(self, value):
        """Delete value maintaining BST property - O(log n) average, O(n) worst"""
        node_to_delete = self._find_node(self.root, value)
        if node_to_delete is None:
            return False
        
        self._delete_node(node_to_delete)
        self.size -= 1
        return True
    
    def _find_node(self, node, value):
        """Helper method to find node with given value"""
        if node is None:
            return None
        if value == node.data:
            return node
        elif value < node.data:
            return self._find_node(node.left, value)
        else:
            return self._find_node(node.right, value)
    
    def _delete_node(self, node):
        """Helper method to delete a node"""
        # Case 1: Node has no children (leaf)
        if node.left is None and node.right is None:
            if node.parent is None:  # Root node
                self.root = None
            elif node.parent.left == node:
                node.parent.left = None
            else:
                node.parent.right = None
        
        # Case 2: Node has one child
        elif node.left is None:  # Only right child
            if node.parent is None:  # Root node
                self.root = node.right
                node.right.parent = None
            elif node.parent.left == node:
                node.parent.left = node.right
                node.right.parent = node.parent
            else:
                node.parent.right = node.right
                node.right.parent = node.parent
        
        elif node.right is None:  # Only left child
            if node.parent is None:  # Root node
                self.root = node.left
                node.left.parent = None
            elif node.parent.left == node:
                node.parent.left = node.left
                node.left.parent = node.parent
            else:
                node.parent.right = node.left
                node.left.parent = node.parent
        
        # Case 3: Node has two children
        else:
            # Find inorder successor (minimum in right subtree)
            successor = self._find_min_node(node.right)
            node.data = successor.data  # Copy successor's data
            self._delete_node(successor)  # Delete successor
    
    def inorder_traversal(self):
        """Inorder traversal for BST (sorted order) - O(n)"""
        result = []
        self._inorder_recursive(self.root, result)
        return result
    
    def _inorder_recursive(self, node, result):
        """Helper method for inorder traversal"""
        if node:
            self._inorder_recursive(node.left, result)
            result.append(node.data)
            self._inorder_recursive(node.right, result)
    
    def preorder_traversal(self):
        """Preorder traversal - O(n)"""
        result = []
        self._preorder_recursive(self.root, result)
        return result
    
    def _preorder_recursive(self, node, result):
        """Helper method for preorder traversal"""
        if node:
            result.append(node.data)
            self._preorder_recursive(node.left, result)
            self._preorder_recursive(node.right, result)
    
    def postorder_traversal(self):
        """Postorder traversal - O(n)"""
        result = []
        self._postorder_recursive(self.root, result)
        return result
    
    def _postorder_recursive(self, node, result):
        """Helper method for postorder traversal"""
        if node:
            self._postorder_recursive(node.left, result)
            self._postorder_recursive(node.right, result)
            result.append(node.data)
    
    def height(self):
        """Calculate tree height - O(n)"""
        return self._height_recursive(self.root)
    
    def _height_recursive(self, node):
        """Helper method for height calculation"""
        if node is None:
            return -1
        
        left_height = self._height_recursive(node.left)
        right_height = self._height_recursive(node.right)
        return max(left_height, right_height) + 1
    
    def is_valid_bst(self):
        """Validate BST property - O(n)"""
        return self._is_valid_bst_recursive(self.root, float('-inf'), float('inf'))
    
    def _is_valid_bst_recursive(self, node, min_val, max_val):
        """Helper method to validate BST property"""
        if node is None:
            return True
        
        if node.data <= min_val or node.data >= max_val:
            return False
        
        return (self._is_valid_bst_recursive(node.left, min_val, node.data) and
                self._is_valid_bst_recursive(node.right, node.data, max_val))

# Example usage - Building and manipulating a BST
bst = BinarySearchTree()
values = [50, 30, 70, 20, 40, 60, 80, 10, 25, 35, 45]

print("Building BST with values:", values)
for val in values:
    bst.insert(val)

print(f"BST size: {len(bst)}")
print(f"Tree height: {bst.height()}")
print(f"Inorder (sorted): {bst.inorder_traversal()}")
print(f"Preorder: {bst.preorder_traversal()}")
print(f"Postorder: {bst.postorder_traversal()}")

print(f"\nMin value: {bst.find_min()}")
print(f"Max value: {bst.find_max()}")

print(f"Search for 40: {bst.search(40)}")
print(f"Search for 100: {bst.search(100)}")

print("\nDeleting values 30, 50, 20:")
bst.delete(30)
bst.delete(50)
bst.delete(20)
print(f"Inorder after deletions: {bst.inorder_traversal()}")
print(f"Is valid BST: {bst.is_valid_bst()}")
```

### Array Representation of Binary Tree
```python
class ArrayBinaryTree:
    """Binary tree using array representation"""
    
    def __init__(self, capacity=100):
        self.capacity = capacity
        self.data = [None] * capacity
        self.size = 0
    
    def __len__(self):
        return self.size
    
    def is_empty(self):
        """Check if tree is empty - O(1)"""
        return self.size == 0
    
    def insert(self, value):
        """Insert value at next available position - O(1)"""
        if self.size >= self.capacity:
            raise IndexError("Tree capacity exceeded")
        
        self.data[self.size] = value
        self.size += 1
    
    def get_parent(self, index):
        """Get parent index - O(1)"""
        if index <= 0 or index >= self.size:
            return None
        parent_idx = (index - 1) // 2
        return parent_idx if parent_idx < self.size else None
    
    def get_left_child(self, index):
        """Get left child index - O(1)"""
        left_idx = 2 * index + 1
        return left_idx if left_idx < self.size else None
    
    def get_right_child(self, index):
        """Get right child index - O(1)"""
        right_idx = 2 * index + 2
        return right_idx if right_idx < self.size else None
    
    def preorder_traversal(self):
        """Preorder traversal for array representation - O(n)"""
        result = []
        self._preorder_array(0, result)
        return result
    
    def _preorder_array(self, index, result):
        """Helper method for array preorder traversal"""
        if index < self.size and self.data[index] is not None:
            result.append(self.data[index])
            left_child = self.get_left_child(index)
            if left_child is not None:
                self._preorder_array(left_child, result)
            right_child = self.get_right_child(index)
            if right_child is not None:
                self._preorder_array(right_child, result)
    
    def inorder_traversal(self):
        """Inorder traversal for array representation - O(n)"""
        result = []
        self._inorder_array(0, result)
        return result
    
    def _inorder_array(self, index, result):
        """Helper method for array inorder traversal"""
        if index < self.size and self.data[index] is not None:
            left_child = self.get_left_child(index)
            if left_child is not None:
                self._inorder_array(left_child, result)
            result.append(self.data[index])
            right_child = self.get_right_child(index)
            if right_child is not None:
                self._inorder_array(right_child, result)
    
    def postorder_traversal(self):
        """Postorder traversal for array representation - O(n)"""
        result = []
        self._postorder_array(0, result)
        return result
    
    def _postorder_array(self, index, result):
        """Helper method for array postorder traversal"""
        if index < self.size and self.data[index] is not None:
            left_child = self.get_left_child(index)
            if left_child is not None:
                self._postorder_array(left_child, result)
            right_child = self.get_right_child(index)
            if right_child is not None:
                self._postorder_array(right_child, result)
            result.append(self.data[index])
    
    def height(self):
        """Calculate height from array representation - O(n)"""
        if self.is_empty():
            return -1
        return self._height_array(0)
    
    def _height_array(self, index):
        """Helper method for height calculation"""
        if index >= self.size or self.data[index] is None:
            return -1
        
        left_height = -1
        left_child = self.get_left_child(index)
        if left_child is not None:
            left_height = self._height_array(left_child)
        
        right_height = -1
        right_child = self.get_right_child(index)
        if right_child is not None:
            right_height = self._height_array(right_child)
        
        return max(left_height, right_height) + 1
    
    def __str__(self):
        if self.is_empty():
            return "Empty tree"
        
        lines = []
        level = 0
        level_size = 1
        index = 0
        
        while index < self.size:
            level_nodes = []
            for i in range(level_size):
                if index + i < self.size and self.data[index + i] is not None:
                    level_nodes.append(str(self.data[index + i]))
                else:
                    level_nodes.append("None")
            lines.append(f"Level {level}: {' '.join(level_nodes)}")
            index += level_size
            level_size *= 2
            level += 1
        
        return '\n'.join(lines)

# Example usage
array_tree = ArrayBinaryTree()
values = ['A', 'B', 'C', 'D', 'E', 'F', 'G']

print("Building array-based binary tree:")
for val in values:
    array_tree.insert(val)

print(f"Tree size: {len(array_tree)}")
print(f"Tree height: {array_tree.height()}")
print(f"Preorder: {array_tree.preorder_traversal()}")
print(f"Inorder: {array_tree.inorder_traversal()}")
print(f"Postorder: {array_tree.postorder_traversal()}")

print("\nTree structure:")
print(array_tree)
```