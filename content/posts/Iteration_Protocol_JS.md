+++
title = 'Iteration_Protocol_JS'
date = 2024-08-17T22:41:49+05:30
draft = true
tags = ["JS" , "Symbols"]
+++

# Iteration Protocol in JS

## Symbol.Iterator

In JavaScript, the `Symbol.iterator` is a special symbol that defines the default iterator for an object. When you use `for...of`, spread syntax (`...`), or destructuring on an object, JavaScript looks for this iterator.

A generator function (`function*`) simplifies creating iterators because it automatically handles the iterator protocol. The `yield` keyword in a generator pauses execution and returns a value when the iterator's `next()` method is called.

### Example: Simple Iterable Object

```javascript
const iterableObject = {
  *[Symbol.iterator]() {
    yield 1;
    yield 2;
    yield 3;
  }
};

for (const value of iterableObject) {
  console.log(value);
}
// Output: 1, 2, 3
```

---

## Binary Tree Class

Before we dive into DFS and BFS, let’s define a basic binary tree structure:

```javascript
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right;
  }
}

class BinaryTree {
  constructor(root = null) {
    this.root = root;
  }
}
```

This defines:

- `TreeNode` to represent each node in the binary tree.
- `BinaryTree` to hold the root node and traversal logic.

---

## Depth-First Search (DFS) with `Symbol.iterator`

Depth-First Search (DFS) explores as far as possible along each branch before backtracking. Common DFS traversals include **in-order**, **pre-order**, and **post-order**.

### In-Order Traversal (Left, Root, Right)

Here’s how you can implement in-order traversal using `yield`:

```javascript
class BinaryTree {
  constructor(root = null) {
    this.root = root;
  }

  *inOrderTraversal(node) {
    if (node) {
      // Traverse left subtree
      yield* this.inOrderTraversal(node.left);
      // Yield current node's value
      yield node.value;
      // Traverse right subtree
      yield* this.inOrderTraversal(node.right);
    }
  }

  [Symbol.iterator]() {
    return this.inOrderTraversal(this.root);
  }
}
```

### Application

```javascript
const tree = new BinaryTree(
  new TreeNode(1, 
    new TreeNode(2, new TreeNode(4), new TreeNode(5)), 
    new TreeNode(3)
  )
);

for (const value of tree) {
  console.log(value);
}
// Output: 4, 2, 5, 1, 3
```

This approach works for other DFS traversals like pre-order or post-order by changing the order of recursive calls and the `yield` statements.

---

## Breadth-First Search (BFS) with `Symbol.iterator`

Breadth-First Search (BFS), also known as level-order traversal, visits nodes level by level from top to bottom.

Here’s how you can implement BFS using a queue:

```javascript
class BinaryTree {
  constructor(root = null) {
    this.root = root;
  }

  *breadthFirstTraversal() {
    if (!this.root) return;

    const queue = [this.root];
    while (queue.length > 0) {
      const current = queue.shift();
      yield current.value;

      if (current.left) queue.push(current.left);
      if (current.right) queue.push(current.right);
    }
  }

  [Symbol.iterator]() {
    return this.breadthFirstTraversal();
  }
}
```

### Usage

```javascript
const tree = new BinaryTree(
  new TreeNode(1, 
    new TreeNode(2, new TreeNode(4), new TreeNode(5)), 
    new TreeNode(3, null, new TreeNode(6))
  )
);

for (const value of tree) {
  console.log(value);
}
// Output: 1, 2, 3, 4, 5, 6
```

### Full Example: Combining DFS and BFS

```javascript
class BinaryTree {
  constructor(root = null) {
    this.root = root;
  }

  *inOrderTraversal(node) {
    if (node) {
      yield* this.inOrderTraversal(node.left);
      yield node.value;
      yield* this.inOrderTraversal(node.right);
    }
  }

  *breadthFirstTraversal() {
    if (!this.root) return;

    const queue = [this.root];
    while (queue.length > 0) {
      const current = queue.shift();
      yield current.value;

      if (current.left) queue.push(current.left);
      if (current.right) queue.push(current.right);
    }
  }

  [Symbol.iterator]() {
    return this.inOrderTraversal(this.root); // Change to breadthFirstTraversal for BFS
  }
}

// Example usage
const tree = new BinaryTree(
  new TreeNode(1, 
    new TreeNode(2, new TreeNode(4), new TreeNode(5)), 
    new TreeNode(3, null, new TreeNode(6))
  )
);

console.log('DFS:');
for (const value of tree) {
  console.log(value);
}

console.log('BFS:');
tree[Symbol.iterator] = tree.breadthFirstTraversal.bind(tree);
for (const value of tree) {
  console.log(value);
}
```
