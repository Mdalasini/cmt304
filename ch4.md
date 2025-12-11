# Chapter 4: Non-Linear Data Structures

Non-linear data structures organize elements in hierarchical or networked relationships rather than simple sequences. Trees represent the most important non-linear structure, providing efficient organization and fast search capabilities.

## 4.1 Trees

### Tree Structure and Concept
A tree is a hierarchical structure consisting of nodes connected by edges. The tree has a single root node at the top, and all other nodes descend from it. Each node can have zero or more children, but only one parent (except the root, which has no parent).

Think of a tree like an organizational chart or family tree - there's a clear hierarchy with some nodes being "above" others.

### Tree Terminology

**Degree**
The degree of a node is the number of children it has. A leaf node has degree 0, while an internal node has degree ≥ 1.

**Level or Depth of a Node**
The level or depth indicates how many edges connect a node to the root. The root is at level 0, its immediate children are at level 1, and so on.

**Leaf Nodes**
Leaf nodes are nodes with no children. They're at the bottom of the tree hierarchy and represent terminal points in the structure.

**Child Nodes**
Child nodes are nodes directly connected below another node. Every node except the root is a child of exactly one parent node.

**Grandchildren**
Grandchildren are nodes two levels below a given node - the children of your children.

**Ancestors**
Ancestors are all nodes on the path from a given node to the root, including the parent, grandparent, and so on.

**Height of Tree**
The height is the longest path from root to leaf. A tree with only a root node has height 0, while a tree with root and one level of children has height 1.

### Application Areas of Tree Data Structures
Trees solve many practical problems:
- **File systems**: Organizing directories and files hierarchically
- **Database indexing**: Efficient data retrieval through B-trees
- **Decision making**: Decision trees for classification problems
- **Organization structure**: Representing corporate hierarchies
- **Parsing**: Syntax trees for programming languages

## 4.2 Binary Search Trees (BST)

### BST Structure and Properties
A Binary Search Tree is a special type of tree where each node has at most two children, and the left child's value is less than the parent's value, while the right child's value is greater. This property enables efficient search operations.

The BST property ensures that for any node:
- All values in the left subtree are less than the node's value
- All values in the right subtree are greater than the node's value
- Both left and right subtrees are also binary search trees

### BST Construction
Building a BST involves inserting nodes while maintaining the BST property. The first inserted value becomes the root, and subsequent values are placed in appropriate positions based on comparisons with existing nodes.

### BST Insertion Operations
Insertion follows a simple recursive process:
1. Start at the root
2. If the new value is less than the current node, go left; if greater, go right
3. Continue until you find an empty position
4. Insert the new node at that position

### BST Operations and Performance
- **Search**: O(log n) average case, O(n) worst case (if tree becomes unbalanced)
- **Insertion**: O(log n) average case, O(n) worst case
- **Deletion**: O(log n) average case, O(n) worst case

Performance depends on tree balance - completely unbalanced trees perform like linked lists.

### Array Representation of Trees
Trees can be represented using arrays where:
- Root is at index 0
- Left child of node i is at index 2i + 1
- Right child of node i is at index 2i + 2
- Parent of node i is at index (i-1)/2

This representation works well for complete or near-complete trees but wastes space for sparse trees.

### BST ADT Implementation
The abstract data type for BSTs typically includes:
- **insert(value)**: Add value while maintaining BST property
- **search(value)**: Find if value exists in tree
- **delete(value)**: Remove value while maintaining BST property
- **traversal()**: Visit all nodes in specific order
- **min()**: Find minimum value in tree
- **max()**: Find maximum value in tree

### Applications of Binary Search Trees
BSTs are fundamental to many applications:
- **Database indexing**: B-trees and variants for disk-based storage
- **Sorting algorithms**: In-order traversal produces sorted sequence
- **Symbol tables**: Compiler symbol management
- **Priority queues**: Heap structures built on tree principles

## 4.3 Tree Traversals

Tree traversal determines the order in which nodes are visited. Each traversal method serves different purposes and reveals different properties of the tree structure.

### Preorder Traversal (Definition and Implementation)
Preorder visits nodes in this sequence: Root, Left subtree, Right subtree.

The algorithm works recursively:
1. Visit the current node
2. Traverse left subtree recursively
3. Traverse right subtree recursively

Use cases include:
- Creating tree copies
- Generating directory listings
- Evaluating expression trees

### Inorder Traversal (Definition and Implementation)
Inorder visits nodes in this sequence: Left subtree, Root, Right subtree.

The algorithm:
1. Traverse left subtree recursively
2. Visit the current node
3. Traverse right subtree recursively

For BSTs, inorder traversal produces values in ascending order, making it useful for validation and sorting.

### Postorder Traversal (Definition and Implementation)
Postorder visits nodes in this sequence: Left subtree, Right subtree, Root.

The algorithm:
1. Traverse left subtree recursively
2. Traverse right subtree recursively
3. Visit the current node

This order is essential for:
- Memory cleanup (delete children before parent)
- Expression tree evaluation
- Directory deletion operations

### Traversal Algorithms and Outputs
The same tree produces different output sequences depending on traversal method:

For tree: Root(A) with left child(B) and right child(C):
- **Preorder**: A, B, C
- **Inorder**: B, A, C  
- **Postorder**: B, C, A

Each traversal reveals different aspects of the tree's structure and serves specific algorithmic purposes.