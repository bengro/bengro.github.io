## Abstract
Tree-based data structures are excellent building blocks for algorithms and abstract data structures. Trees you can find the the forest as well as in Priority Queues, Sets or Hash Maps. This post walks through basic tree-based data structures, their properties as well as aspects of their implementation. It's a compilation of notes I gathered when working through Binary Tree interview problems. 


## Terminology
Let's get an overview of tree-based abstract data structures we are going to look at:

  * A **Tree** *T* is a acyclic graph consisting of vertices and edges. It has precisely V-1 edges, where V denotes the number f vertices in *T*. It can be interpreted as directed graph or an undirected graph.
  * A **Binary Tree** is a tree, with the additional constraint that each vertex u can have at most two children. The children are called left and right child. It's legal to just make use of one child per node. 
  * A **Binary Search Tree**: is a Binary Tree, with the additional constraint that the children must be in the right order.The child with the smaller value than it's parent goes left, the child with the greater or equal value goes right.  
  * A **Binary Heap** is a Binary Tree with two additional constraints. a) The Binary Tree must be complete, which means that the height of the closest and farthest vertex v from root differs only by at most 1. b) Each node must comply with the heap property. For a max-heap, the property expresses the constraint that the parent must always be greater than it's children. In the case of a min-heap, the parent must be smaller than the children.  
  * A **Balanced Binary Search Tree** is a Binary Search Tree that cannot degenerate (in other words, it cannot become a list). When inserting or deleting nodes, it shuffles around nodes such that the height of the tree is kept to an upper bound which is better than n. AVL tree, or Black-Red-Tree are implementations of a Balanced Binary Search Tree.



##Tree
Like any other graph, a tree can be represented as either an adjacency list, adjacency matrix or objects. When working with Binary Tree structures, objects are usually used to represent the tree.

| Representation   | Edge Look up  | Out edges Look up | Edge Insertion    | Edge Deletion     |
| ---------------- |:-------------:| :----------------:| :----------------:| :----------------:|
| Adjacency matrix | O(1)          | O(V)              | O(1)              | O(1)              |
| Adjacency list   | O(E)          | O(E)              | O(1)              | O(E)              |
| Objects          | O(E)          | O(E)              | O(V)              | O(E)              |

### Notes
  * A tree is by definition acyclic and has one edge less than it has vertices.
  * A tree can be represented in different ways, the "best" representation is relative to the task at hand.
  * A list is a valid tree.
  * A complete tree can be represented with a simple array.



## Binary Tree
A Binary Tree which can be unbalanced, is rarely used in practice as it can degenerate and yield *O(n)* runtime complexities. Note, that even a list can conform with the constraint of a Binary Tree. Still, the binary tree structure (parent with two children), lays the foundation for Binary Heaps or the Binary Search Tree.

The most important programming concept to keep in mind when solving tree problems is **recursion**. Often in interview problems, the recursion of the Binary Tree either builds up to a solution (top-down) or aggregates information at a node (bottom-up). 

A Tree node has two members fields for the children, a member field for the value. It could also have a parent link, especially in cases where we try to avoid recursion.

    public class TreeNode {
       int val;
       TreeNode left;
       TreeNode right;
       TreeNode(int x) { val = x; }
    }

### Binary Tree Traversal
A Binary Tree can be traversed using Breadth-first or Depth-first search.
Given the above TreeNode type, we can use recursion to traverse the nodes in different orders.
If we wanted to traverse the Binary Tree in pre-order we can do the following:

    public void preorder(TreeNode node) {
        if (node == null) return;
        // process node
        preorder(node.left);
        preorder(node.right);
    }


### Notes
  * A binary tree can be traversed in pre-order, post-order, in-order or level-order.
  * A list can be a degenerate, yet valid binary tree.
  * Always make the distinction between Binary Tree and Binary Search Tree as they have very different 

### Interview questions
  1. Flatten a Binary Tree
  2. Traverse a Binary Tree (pre-order, in-order, post-order) without using recursion. (This problem will make you think about recursion and how to replace it with data structures.)
  3. Find a subtree in a Binary Tree.
  4. Find common ancestor in Binary Tree. (Think about TreeNode properties) 


## Binary Heaps
A Binary Heap extends a Binary Tree by two properties:
  
  * it is a **complete Binary Tree**
  * Each vertex v satisfies a **order invariant**

Binary Heaps have the following complexities:

  * Generating a heap with n elements `O(n log n)`.
  * Inserting a node takes `O(log n)`.
  * Deleting a node is `O(log n)`.
  * Accessing the top element is `O(1)`.

Regarding the implementation of a Binary Heap, we need the following routines:

  * **heapify** (Node n): reinforces the heap invariant on Node n. After the call, n will be in the correct position.
  * **build** (int[] array): Given an array, it builds a Binary Heap by applying heapify to all elements.
  
Let's have a look at an implementation of a Binary Heap, in this case with a maximum heap invariant.

    public class MaxHeap {
        int[] sequence;
        int heapSize;
        int sequenceSize;

        public MaxHeap(int[] array) {
            this.sequence = array;
            this.heapSize = array.length;
            this.sequenceSize = array.length;
            build();
        }

        public int[] getHeap () {
            return this.sequence;
        }

        // O(n log n)
        private void build() {
            for (int i = this.sequenceSize - 1; i >= 0; i--) {
                heapify(i);
            }
        }

        // enforces heap property if not already in place, O(log n)
        private void heapify (int pos) {
            int left = 2*pos+1;
            int right = 2*pos+2;
            int largest = pos;
            if (left < this.heapSize && this.sequence[pos] < this.sequence[right]) {
                largest = right;
            }
            if (right < this.heapSize && this.sequence[largest] < this.sequence[left]) {
                largest = left;
            }
            if (largest != pos) {
                int tmp = this.sequence[pos];
                this.sequence[pos] = this.sequence[largest];
                this.sequence[largest] = tmp;
                heapify(largest);
            }
        }
    }

Binary Heaps can be represented with an array. The reason for this lies in the symmetry of a complete Binary Tree.
The above code builds a Binary Heap given an array of integers. 
By providing methods like push(), pop(), top(), the Heap implementation could be turned into a Priority Queue. 

### Note
  * **Heap Sort** is a `O(n log n)` sorting algorithm which is based on a Binary Heap .
  * A **Priority Queue** is a Binary Heap, with additional routines for practicality.


## Binary Search Tree

A Binary Search Tree (BST) has Tree nodes just like the Binary Tree. Binary Search Tree can still degenerate and therefore yield bad complexities.
This is why, it is important to add balancing to a BST.
If we have a complete Binary Search Tree on the other hand we can expect to:

  * find element in `log(n)`
  * Insert an element in `log(n)`
  * Delete a node in `log(n)`

### Note
  * When traversing a BST with in-order traversal, we can obtain the sorted order of the tree nodes.

### Interview questions

  1. Given a binary tree, insert edges that point to the vertex on the right hand side on the same level.
  2. Build a binary search tree from an array. Note: make sure it's a sorted array.
  3. Verify whether binary tree is a binary search tree.


## Balanced Binary Search Tree
To be continued, covering AVL Tree, Red-Black-Tree.
