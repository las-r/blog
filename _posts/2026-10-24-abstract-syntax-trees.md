---
layout: post
title: "Abstract Syntax Trees"
date: 2026-10-24
---

## Introduction
How can a program's source code be represented as a tree of objects?

An interpreter is a program that executes another program. A tree-walk interpreter does this by traversing, or walking, an **Abstract Syntax Tree (AST)**, evaluating each node as it goes along.

My code examples will be written in Python, but anything more than basic semantic knowledge of it isn't needed to understand the examples.

## Nodes
A **node** is a representation of a piece of code before it gets executed. It describes something that can be evaluated or executed, such as a literal value, variable reference, or even other parts of a program, such as if statements or function definitions.

### Structure of a Node
Every node should produce a value upon evaluation, even if that value is `None`. To do this in our tree-walk interpreter, every node should will an `eval()` method, which should take in an environment dictionary (`env = {}`) as an argument. The environment will be used later to store and retrieve values.

Here is an example of a very basic node that we will be using:
```py
class LiteralNode:
    def __init__(self, value):
        self.value = value

    def eval(self, env: dict):
        return self.value
```

This node simply returns the value it was initialized with. If we initialize it as `LiteralNode(10)`, it will return `10`. From now on, when I mention a node's return value, I will mean the value returned by it's `eval()` method.

### Nesting Nodes
More complex nodes will have **children nodes**. For example, a simple addition node would look like this:
```py
class AddNode:
    def __init__(self, a_node, b_node):
        self.a_node = a_node
        self.b_node = b_node
    
    def eval(self, env: dict):
        a = self.a_node.eval(env)
        b = self.b_node.eval(env)
        return a + b
```

Note that `AddNode` doesn't take in raw values to add as arguments. It takes in two other nodes, it's child nodes. When `AddNode.eval()` is called, it evaluates its children and adds their return values together.

This allows us to **nest nodes**, meaning one node can contain other nodes, which themselves can contain even more nodes. For example, consider the expression `1 + 2 + 3`. We should represent it using nesting like this:
```py
AddNode(
    LiteralNode(1),
    AddNode(
        LiteralNode(2),
        LiteralNode(3)
    )
)
```

When we evaluate the outer node, it will first evaluate its left child, which returns `1`. Then, it will evaluate its second child, which will return `2 + 3`, or `5`. It will add these values together, returning `6`.

In other words, evaluating a node will recursively evaluate it's children nodes. This structure is the foundation of an **Abstract Syntax Tree**.

## Abstract Syntax Tree
An **Abstract Syntax Tree**, abbreviated to **AST**, is a tree-shaped representation of a program's structure. Each node within it represent some construct in the program, and each node's child nodes represent the constructs contains within them.

### Structure of an AST
Consider the expression `1 + 2 + 3` from earlier. We represented it using nested `AddNode` and `LiteralNode` objects. This structure is an AST.

We can visualize it as a tree:
![An AST representing 1 + 2 + 3]({{ '/assets/images/ast_example.svg' | relative_url }})

The outer `AddNode` is the **root** of the tree. Its children are the `LiteralNode` and the `AddNode`. The inner `AddNode` itself has two children, both of which are `LiteralNode` objects.

Instead of storing the expression as a sequence of characters, we represent its operation and operands as objects connected through parent-child relationships.

### Traversing an AST
An AST is evaluated by being **traversed**, or walked. Starting at the root, the interpreter evaluates its child nodes and uses their results to calculate the final value.

This can also be used to represent other structures in programming. A complete program could include nodes for variable declarations, conditionals, loops, function definitions, and more. For example, an `IfNode` may have a child node for the condition and a list of children nodes for the body.

These nodes are what allow an entire program to be represented as a single tree.

## Conclusion
In conclusion, an **Abstract Syntax Tree** represents the structure of a program through a hierarchy of nodes. By nesting nodes within one another, everything from simple expressions to entire programs can be represented.

In the next article, we'll look at how to turn source code into tokens, the first step toward constructing an AST from an actual program.

Happy coding!