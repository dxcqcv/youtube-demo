# Data Structures

## Data Structures in JavaScript
1. Introduction
As business logic moves from the back to the front more and more, expertise in Frontend Engineering becomes ever more crucial. As Frontend Engineers, we depend on view libraries like React to be productive. View libraries in turn depend on state libraries like Redux to manage the data. Together, React and Redux subscribe to the reactive programming paradigm in which UI updates react to data changes. Increasingly, backends act simply as API servers, providing endpoints only to retrieve and update the data. In effect, the backend just “forwards” the database to the frontend, expecting the Frontend Engineer to handle all the controller logic. The rising popularity of microservices and GraphQL attest to this growing trend.

Now, in addition to having aesthetic understanding of HTML and CSS, Frontend Engineers are expected to master JavaScript as well. As datastores on the client become “replicas” of databases on the server, intimate knowledge of idiomatic data structures becomes pivotal. In fact, an engineer’s level of experience can be inferred from his or her ability to distinguish when and why to use a particular data structure.

Bad programmers worry about the code. Good programmers worry about data structures and their relationships.

— Linus Torvalds, Creator of Linux and Git

At a high level, there are basically three types of data structures. Stacks and Queues are array-like structures that differ only in how items are inserted and removed. Linked Lists, Trees, and Graphs are structures with nodes that keep references to other nodes. Hash Tables depend on hash functions to save and locate data.

In terms of complexity, Stacks and Queues are the simplest and can be constructed from Linked Lists. Trees and Graphs are the most complex because they extend the concept of a linked list. Hash Tables need to utilize these data structures to perform reliably. In terms of efficiency, Linked Lists are most optimal for recording and storing of data, while Hash Tables are most performant for searching and retrieving of data.

To explain why and illustrate when, this article will conform to the order of these dependencies. Let’s begin!

Stack
Arguably the most important Stack in JavaScript is the call stack where we push in the scope of a function whenever we execute it. Programmatically, it’s just an array with two principled operations: push and pop. Push adds elements to the top of the array, while Pop removes them from the same location. In other words, Stacks follow the “Last In, First Out” protocol (LIFO).

Below is an example of a Stack in code. Notice that we can reverse the order of the stack: the bottom becomes the top and the top becomes the bottom. As such, we can use the array’s unshift and shift methods in place of push and pop, respectively.

class Stack {
  constructor(...items) {
    this.reverse = false;
    this.stack = [...items];
  }

  push(...items) {
    return this.reverse
      ? this.stack.unshift(...items)
      : this.stack.push(...items);
  }

  pop() {
    return this.reverse ? this.stack.shift() : this.stack.pop();
  }
}

mocha.setup("bdd");
const { assert } = chai;

describe("Stacks", () => {
  it("Should push items to top of stack", () => {
    const stack = new Stack(4, 5);
    assert.equal(stack.push(1, 2, 3), 5);
    assert.deepEqual(stack.stack, [4, 5, 1, 2, 3]);
  });

  it("Should push items to bottom of stack", () => {
    const stack = new Stack(4, 5);
    stack.reverse = true;
    assert.equal(stack.push(1, 2, 3), 5);
    assert.deepEqual(stack.stack, [1, 2, 3, 4, 5]);
  });

  it("Should pop item from top of stack", () => {
    const stack = new Stack(1, 2, 3);
    assert.equal(stack.pop(), 3);
  });

  it("Should pop item from bottom of stack", () => {
    const stack = new Stack(1, 2, 3);
    stack.reverse = true;
    assert.equal(stack.pop(), 1);
  });
});

mocha.run();

As the number of items grows, push/pop becomes increasingly more performant than unshift/shift because every item needs to be reindexed in the latter but not the former.

Queue
JavaScript is an event-driven programming language which makes it possible to support non-blocking operations. Internally, the browser manages only one thread to run the entire JavaScript code, using the event queue to enqueue listeners and the event loop to listen for the registered events. To support asynchronicity in a single-threaded environment (to save CPU resources and enhance the web experience), listener functions dequeue and execute only when the call stack is empty. Promises depend on this event-driven architecture to allow a “synchronous-style” execution of asynchronous code that does not block other operations.

Programmatically, Queues are just arrays with two primary operations: unshift and pop. Unshift enqueues items to the end of the array, while Pop dequeues them from the beginning of the array. In other words, Queues follow the “First In, First Out” protocol (FIFO). If the direction is reversed, we can replace unshift and pop with push and shift, respectively.

An example of a Queue in code:

class Queue {
  constructor(...items) {
    this.reverse = false;
    this.queue = [...items];
  }

  enqueue(...items) {
    return this.reverse
      ? this.queue.push(...items)
      : this.queue.unshift(...items);
  }

  dequeue() {
    return this.reverse ? this.queue.shift() : this.queue.pop();
  }
}

mocha.setup("bdd");
const { assert } = chai;

describe("Queues", () => {
  it("Should enqueue items to the left", () => {
    const queue = new Queue(4, 5);
    assert.equal(queue.enqueue(1, 2, 3), 5);
    assert.deepEqual(queue.queue, [1, 2, 3, 4, 5]);
  });

  it("Should enqueue items to the right", () => {
    const queue = new Queue(4, 5);
    queue.reverse = true;
    assert.equal(queue.enqueue(1, 2, 3), 5);
    assert.deepEqual(queue.queue, [4, 5, 1, 2, 3]);
  });

  it("Should dequeue item from the right", () => {
    const queue = new Queue(1, 2, 3);
    assert.equal(queue.dequeue(), 3);
  });

  it("Should dequeue item from the left", () => {
    const queue = new Queue(1, 2, 3);
    queue.reverse = true;
    assert.equal(queue.dequeue(), 1);
  });
});

mocha.run();

Linked List
Like arrays, Linked Lists store data elements in sequential order. Instead of keeping indexes, linked lists hold pointers to other elements. The first node is called the head while the last node is called the tail. In a singly-linked list, each node has only one pointer to the next node. Here, the head is where we begin our walk down the rest of the list. In a doubly-linked list, a pointer to the previous node is also kept. Therefore, we can also start from the tail and walk “backwards” toward the head.

Linked lists have constant-time insertions and deletions because we can just change the pointers. To do the same operations in arrays requires linear time because subsequent items need to be shifted over. Also, linked lists can grow as long as there is space. However, even “dynamic” arrays that automatically resize could become unexpectedly expensive. Of course, there’s always a tradeoff. To lookup or edit an element in a linked list, we might have to walk the entire length which equates to linear time. With array indexes, however, such operations are trivial.

Like arrays, linked lists can operate as stacks. It’s as simple as having the head be the only place for insertion and removal. Linked lists can also operate as queues. This can be achieved with a doubly-linked list, where insertion occurs at the tail and removal occurs at the head, or vice versa. For large numbers of elements, this way of implementing queues is more performant than using arrays because shift and unshift operations at the beginning of arrays require linear time to re-index every subsequent element.

Linked lists are useful on both the client and server. On the client, state management libraries like Redux structure its middleware logic in a linked-list fashion. When actions are dispatched, they are piped from one middleware to the next until all is visited before reaching the reducers. On the server, web frameworks like Express also structure its middleware logic in a similar fashion. When a request is received, it is piped from one middleware to the next until a response is issued.

An example of a Doubly-Linked List in code:

class Node {
  constructor(value, next, prev) {
    this.value = value;
    this.next = next;
    this.prev = prev;
  }
}

class LinkedList {
  constructor() {
    this.head = null;
    this.tail = null;
  }

  addToHead(value) {
    const node = new Node(value, null, this.head);
    if (this.head) this.head.next = node;
    else this.tail = node;
    this.head = node;
  }

  addToTail(value) {
    const node = new Node(value, this.tail, null);
    if (this.tail) this.tail.prev = node;
    else this.head = node;
    this.tail = node;
  }

  removeHead() {
    if (!this.head) return null;
    const value = this.head.value;
    this.head = this.head.prev;
    if (this.head) this.head.next = null;
    else this.tail = null;
    return value;
  }

  removeTail() {
    if (!this.tail) return null;
    const value = this.tail.value;
    this.tail = this.tail.next;
    if (this.tail) this.tail.prev = null;
    else this.head = null;
    return value;
  }

  search(value) {
    let current = this.head;
    while (current) {
      if (current.value === value) return value;
      current = current.prev;
    }
    return null;
  }

  indexOf(value) {
    const indexes = [];
    let current = this.tail;
    let index = 0;
    while (current) {
      if (current.value === value) indexes.push(index);
      current = current.next;
      index++;
    }
    return indexes;
  }
}

mocha.setup("bdd");
const { assert } = chai;

describe("Linked List", () => {
  it("Should add to head", () => {
    const list = new LinkedList();
    list.addToHead(1);
    list.addToHead(2);
    list.addToHead(3);
    assert.equal(list.tail.prev, null);
    assert.equal(list.tail.value, 1);
    assert.equal(list.tail.next.value, 2);
    assert.equal(list.head.prev.value, 2);
    assert.equal(list.head.value, 3);
    assert.equal(list.head.next, null);
    assert.equal(list.head.prev.prev.value, 1);
    assert.equal(list.tail.next.next.value, 3);
  });

  it("Should add to tail", () => {
    const list = new LinkedList();
    list.addToTail(1);
    list.addToTail(2);
    list.addToTail(3);
    assert.equal(list.tail.prev, null);
    assert.equal(list.tail.value, 3);
    assert.equal(list.tail.next.value, 2);
    assert.equal(list.head.prev.value, 2);
    assert.equal(list.head.value, 1);
    assert.equal(list.head.next, null);
    assert.equal(list.head.prev.prev.value, 3);
    assert.equal(list.tail.next.next.value, 1);
  });

  it("Should remove head", () => {
    const list = new LinkedList();
    list.addToHead(1);
    list.addToHead(2);
    list.addToHead(3);
    assert.equal(list.removeHead(), 3);
    assert.equal(list.head.value, 2);
    assert.equal(list.tail.value, 1);
    assert.equal(list.tail.next.value, 2);
    assert.equal(list.head.prev.value, 1);
    assert.equal(list.head.next, null);
    assert.equal(list.removeHead(), 2);
    assert.equal(list.head.value, 1);
    assert.equal(list.tail.value, 1);
    assert.equal(list.tail.next, null);
    assert.equal(list.head.prev, null);
    assert.equal(list.head.next, null);
    assert.equal(list.removeHead(), 1);
    assert.equal(list.head, null);
    assert.equal(list.tail, null);
  });

  it("Should remove tail", () => {
    const list = new LinkedList();
    list.addToTail(1);
    list.addToTail(2);
    list.addToTail(3);
    assert.equal(list.removeTail(), 3);
    assert.equal(list.head.value, 1);
    assert.equal(list.tail.value, 2);
    assert.equal(list.tail.next.value, 1);
    assert.equal(list.head.prev.value, 2);
    assert.equal(list.tail.prev, null);
    assert.equal(list.removeTail(), 2);
    assert.equal(list.head.value, 1);
    assert.equal(list.tail.value, 1);
    assert.equal(list.tail.next, null);
    assert.equal(list.head.prev, null);
    assert.equal(list.tail.prev, null);
    assert.equal(list.removeTail(), 1);
    assert.equal(list.head, null);
    assert.equal(list.tail, null);
  });

  it("Should search for value", () => {
    const list = new LinkedList();
    list.addToHead(1);
    list.addToHead(2);
    list.addToHead(3);
    assert.equal(list.search(1), 1);
    assert.equal(list.search(2), 2);
    assert.equal(list.search(3), 3);
    assert.equal(list.search(4), null);
  });

  it("Should search for indexes of value", () => {
    const list = new LinkedList();
    list.addToTail(1);
    list.addToTail(2);
    list.addToTail(3);
    list.addToTail(3);
    list.addToTail(1);
    assert.deepEqual(list.indexOf(1), [0, 4]);
    assert.deepEqual(list.indexOf(2), [3]);
    assert.deepEqual(list.indexOf(3), [1, 2]);
    assert.deepEqual(list.indexOf(4), []);
  });
});

mocha.run();

Tree
A Tree is like a linked list, except it keeps references to many child nodes in a hierarchical structure. In other words, each node can have no more than one parent. The Document Object Model (DOM) is such a structure, with a root html node that branches into the head and body nodes, which further subdivide into all the familiar html tags. Under the hood, prototypal inheritance and composition with React components also produce tree structures. Of course, as an in-memory representation of the DOM, React’s Virtual DOM is also a tree structure.

The Binary Search Tree is special because each node can have no more than two children. The left child must have a value that is smaller than or equal to its parent, while the right child must have a greater value. Structured and balanced in this way, we can search for any value in logarithmic time because we can ignore one-half of the branching with each iteration. Inserting and deleting can also happen in logarithmic time. Moreover, the smallest and largest value can easily be found at the leftmost and rightmost leaf, respectively.

Traversal through the tree can happen in a vertical or horizontal procedure. In Depth-First Traversal (DFT) in the vertical direction, a recursive algorithm is more elegant than an iterative one. Nodes can be traversed in pre-order, in-order, or post-order. If we need to explore the roots before inspecting the leaves, we should choose pre-order. But, if we need to explore the leaves before the roots, we should choose post-order. As its name implies, in-order enables us to traverse the nodes in sequential order. This property makes Binary Search Trees optimal for sorting.

In Breadth-First Traversal (BFT) in the horizontal direction, an iterative approach is more elegant than a recursive one. This requires the use of a queue to track all the children nodes with each iteration. The memory needed for such a queue might not be trivial, however. If the shape of a tree is wider than deep, BFT is a better choice than DFT. Also, the path that BFT takes between any two nodes is the shortest one possible.

An example of a Binary Search Tree in code:

class Tree {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }

  insert(value) {
    if (value <= this.value) {
      if (!this.left) this.left = new Tree(value);
      else this.left.insert(value);
    } else {
      if (!this.right) this.right = new Tree(value);
      else this.right.insert(value);
    }
  }

  contains(value) {
    if (value === this.value) return true;
    if (value < this.value) {
      if (!this.left) return false;
      else return this.left.contains(value);
    } else {
      if (!this.right) return false;
      else return this.right.contains(value);
    }
  }

  depthFirstTraverse(order, callback) {
    order === "pre" && callback(this.value);
    this.left && this.left.depthFirstTraverse(order, callback);
    order === "in" && callback(this.value);
    this.right && this.right.depthFirstTraverse(order, callback);
    order === "post" && callback(this.value);
  }

  breadthFirstTraverse(callback) {
    const queue = [this];
    while (queue.length) {
      const root = queue.shift();
      callback(root.value);
      root.left && queue.push(root.left);
      root.right && queue.push(root.right);
    }
  }

  getMinValue() {
    if (this.left) return this.left.getMinValue();
    return this.value;
  }

  getMaxValue() {
    if (this.right) return this.right.getMaxValue();
    return this.value;
  }
}

mocha.setup("bdd");
const { assert } = chai;

const tree = new Tree(5);
for (const value of [3, 6, 1, 7, 8, 4, 10, 2, 9]) tree.insert(value);

/*
  5
 3 6
1 4 7
 2   8
      10
     9  
*/

describe("Binary Search Tree", () => {
  it("Should implement insert", () => {
    assert.equal(tree.value, 5);
    assert.equal(tree.left.value, 3);
    assert.equal(tree.right.value, 6);
    assert.equal(tree.left.left.value, 1);
    assert.equal(tree.right.right.value, 7);
    assert.equal(tree.right.right.right.value, 8);
    assert.equal(tree.left.right.value, 4);
    assert.equal(tree.right.right.right.right.value, 10);
    assert.equal(tree.left.left.right.value, 2);
    assert.equal(tree.right.right.right.right.left.value, 9);
  });

  it("Should implement contains", () => {
    assert.equal(tree.contains(2), true);
    assert.equal(tree.contains(9), true);
    assert.equal(tree.contains(0), false);
    assert.equal(tree.contains(11), false);
  });

  it("Should implement depthFirstTraverse", () => {
    const _pre = [];
    const _in = [];
    const _post = [];
    tree.depthFirstTraverse("pre", value => _pre.push(value));
    tree.depthFirstTraverse("in", value => _in.push(value));
    tree.depthFirstTraverse("post", value => _post.push(value));
    assert.deepEqual(_pre, [5, 3, 1, 2, 4, 6, 7, 8, 10, 9]);
    assert.deepEqual(_in, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]);
    assert.deepEqual(_post, [2, 1, 4, 3, 9, 10, 8, 7, 6, 5]);
  });

  it("Should implement breadthDepthTraverse", () => {
    const result = [];
    tree.breadthFirstTraverse(value => result.push(value));
    assert.deepEqual(result, [5, 3, 6, 1, 4, 7, 2, 8, 10, 9]);
  });

  it("Should implement getMinValue", () => {
    assert.equal(tree.getMinValue(), 1);
  });

  it("Should implement getMaxValue", () => {
    assert.equal(tree.getMaxValue(), 10);
  });
});

mocha.run();

Graph
If a tree is free to have more than one parent, it becomes a Graph. Edges that connect nodes together in a graph can be directed or undirected, weighted or unweighted. Edges that have both direction and weight are analogous to vectors.

Multiple inheritances in the form of Mixins and data objects that have many-to-many relationships produce graph structures. A social network and the Internet itself are also graphs. The most complicated graph in nature is our human brain, which neural networks attempt to replicate to give machines superintelligence.

An example of a Graph in code:

TK

Hash Table
A Hash Table is a dictionary-like structure that pairs keys to values. The location in memory of each pair is determined by a hash function, which accepts a key and returns the address where the pair should be inserted and retrieved. Collisions can result if two or more keys convert to the same address. For robustness, getters and setters should anticipate these events to ensure that all data can be recovered and no data is overwritten. Usually, linked lists offer the simplest solution. Having very large tables also helps.

If we know our addresses will be in integer sequences, we can simply use Arrays to store our key-value pairs. For more complex address mappings, we can use Maps or Objects. Hash tables have insertion and lookup of constant time on average. Because of collisions and resizing, this negligible cost could grow to linear time. In practice, however, we can assume that hash functions are clever enough that collisions and resizing are rare and cheap. If keys represent addresses, and therefore no hashing is needed, a simple object literal can suffice. Of course, there’s always a tradeoff. The simple correspondence between keys and values, and the simple associativity between keys and addresses, sacrifice relationships between data. Thus, hash tables are suboptimal for storing data.

If a tradeoff decision favors retrieval over storage, no other data structure can match the speed of hash tables for lookup, insertion, and deletion. It’s no surprise, therefore, that it’s used everywhere. From the database, to the server, to the client, hash tables, and in particular, hash functions, are crucial to the performance and security of software applications. The speed of database queries relies heavily upon keeping tables of indexes that point to records in sorted order. This way, binary searches can be performed in logarithmic time, a huge performance win especially for Big Data.

On both the client and server, many popular libraries utilize memoization to maximize performance. By keeping a record of the inputs and outputs in a hash table, functions run only once for the same inputs. The popular Reselect library uses this caching strategy to optimize mapStateToProps functions in Redux-enabled applications. In fact, under the hood, the JavaScript engine also utilizes hash tables called heaps to store all the variables and primitives we create. They are accessed from pointers on the call stack.

The Internet itself also relies on hashing algorithms to function securely. The structure of the internet is such that any computer can communicate with any other computer through a web of interconnected devices. Whenever a device logs onto the internet, it also becomes a router through which data streams can travel. However, it’s a double-edged sword. A decentralized architecture means any device in the network can listen in and tamper with the data packages that it helps to relay. Hash functions such as MD5 and SHA256 play a critical role in preventing such man-in-the-middle attacks. E-commerce over HTTPS is safe only because these hashing functions are used.

Inspired by the Internet, blockchain technologies seek to open source the very structure of the web at the protocol level. By using hash functions to create immutable fingerprints for every block of data, essentially the entire database can exist openly on the web for anyone to see and contribute to. Structurally, blockchains are just singly-linked lists of binary trees of cryptographic hashes. The hashing is so cryptic that a database of financial transactions can be created and updated out in the open by anyone! The incredible implication is the awesome power to create money itself. What was once only possible for governments and central banks, now anyone can securely create his or her own currency! This is the key insight realized by the founder of Ethereum and the pseudonymous founder of Bitcoin.

As more and more databases move out into the open, the need for Frontend Engineers who can abstract away all the low-level cryptographic complexities will compound as well. In this future, the main differentiator will be the user experience.

An example of a Hash Table in code:

class Node {
  constructor(key, value) {
    this.key = key;
    this.value = value;
    this.next = null;
  }
}

class Table {
  constructor(size) {
    this.cells = new Array(size);
  }

  hash(key) {
    let total = 0;
    for (let i = 0; i < key.length; i++) total += key.charCodeAt(i);
    return total % this.cells.length;
  }

  insert(key, value) {
    const hash = this.hash(key);
    if (!this.cells[hash]) {
      this.cells[hash] = new Node(key, value);
    } else if (this.cells[hash].key === key) {
      this.cells[hash].value = value;
    } else {
      let node = this.cells[hash];
      while (node.next) {
        if (node.next.key === key) {
          node.next.value = value;
          return;
        }
        node = node.next;
      }
      node.next = new Node(key, value);
    }
  }

  get(key) {
    const hash = this.hash(key);
    if (!this.cells[hash]) return null;
    else {
      let node = this.cells[hash];
      while (node) {
        if (node.key === key) return node.value;
        node = node.next;
      }
      return null;
    }
  }

  getAll() {
    const table = [];
    for (let i = 0; i < this.cells.length; i++) {
      const cell = [];
      let node = this.cells[i];
      while (node) {
        cell.push(node.value);
        node = node.next;
      }
      table.push(cell);
    }
    return table;
  }
}

mocha.setup("bdd");
const { assert } = chai;

const table = new Table(5);
table.insert("baa", 1);
table.insert("aba", 2);
table.insert("aba", 3);
table.insert("aac", 4);
table.insert("aac", 5);

describe("Hash Table", () => {
  it("Should implement hash", () => {
    assert.equal(table.hash("abc"), 4);
  });

  it("Should implement insert", () => {
    assert.equal(table.cells[table.hash("baa")].value, 1);
    assert.equal(table.cells[table.hash("aba")].next.value, 3);
    assert.equal(table.cells[table.hash("aac")].value, 5);
  });

  it("Should implement get", () => {
    assert.equal(table.get("baa"), 1);
    assert.equal(table.get("aba"), 3);
    assert.equal(table.get("aac"), 5);
    assert.equal(table.get("abc"), null);
  });

  it("Should implement getAll", () => {
    assert.deepEqual(table.getAll(), [[], [], [1, 3], [5], []]);
  });
});

mocha.run();

Conclusion
As logic moves increasingly from the server to the client, the data layer on the frontend becomes paramount. The proper management of this layer entails mastery of the data structures upon which logic rests. No one data structure is perfect for every situation because optimizing for one property always equates to losing another. Some structures are more efficient at storing data while some others are more performant for searching through them. Usually, one is sacrificed for the other. At one extreme, linked lists are optimal for storage and can be made into stacks and queues (linear time). At the other, no other structure can match the search speed of hash tables (constant time). Tree structures lie somewhere in the middle (logarithmic time), and only a graph can portray nature’s most complex structure: the human brain (polynomial time). Having the skillset to distinguish when and articulate why is a hallmark of a rockstar engineer.

Examples of these data structures can be found everywhere. From the database, to the server, to the client, and even the JavaScript engine itself, these data structures concretize what essentially are just on and off “switches” on silicon chips into lifelike “objects”. Though only digital, the impact these objects have on society is tremendous. Your ability to read this article freely and securely attests to the awesome architecture of the internet and the structure of its data. Yet, this is only the beginning. Artificial intelligence and decentralized blockchains in the coming decades will redefine what it means to be human and the role of institutions that govern our lives. Existential insights and institutional disintermediation will be characteristics of an internet that has finally matured.

To help transition us towards this more equitable future, we at 
HeartBank®
 channel networks of artificial neurons to imbue our 
Kiitos
 with the power to issue money on the blockchain, coupled with the capacity to empathize the human condition. From the anonymous thanks we give and receive by writing to 
Kiitos
, 
Kiitos
 learns about our kindnesses and their effects, rewarding us in such a way that reduces the economic inequities between us, in a gradual and mysterious process that preserves our personal liberty and freedom. Perhaps the ultimate graph structure in nature is not the human brain, but the human ❤️, if only we can see the heartstrings that connect us all.



2. [link](https://medium.com/siliconwat/data-structures-in-javascript-1b9aed0ea17c)