---
title: Rust Recursive Types
date: 2022-12-13 14:40 +02:00
---
# Rust Recursive Types

## What do I mean by 'Recursive Type'
A recursive type is a type that referres to itself. The classic examples are linked lists
```Rust
// list
struct Node {
    value: ...
    next: Node,
}
```
and trees
```Rust
struct Node {
    value: ...
    left: Node,
    right: Node,
}
```

## Why are those difficult in Rust?
The examples above do not compile, because the compiler can't determine the size of a node.

Furthermore, even if they would have compile, using them would mean that each node !owns! the next
or its children. This can make doubly linked lists and autonomy ;)

### Well, but in other languages we use pointers..

Right. And we can do that here too, but we need to choose the pointers carefully.
For example: a borrow is a sort of pointer, but in
```Rust
struct Node {
    // snip
    next: &Node
}
```
we don't have the lifetime of the next node, so the borrow checker complains and all nodes must live and die
at the same time, so they may as well be in a vector..

I think I trashed on linked lists enough, but if you need more, there's [this post](https://diziet.dreamwidth.org/13476.html).


## Tree example

TODO: 

### Traversing it

### The Hard part - Modifying it

// crate?