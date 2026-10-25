---
layout: post
title: "Abstract Syntax Trees"
date: 2026-10-24
---

## Introduction
How can a program's source code be represented in a way that an interpreter can understand and execute?

One way is to represent the program as a hierarchy of objects called an **Abstract Syntax Tree (AST)**. Each object, called a **node**, represents a part of the program, such as a value, an operation, or a control-flow statement. By nesting these nodes within one another, we can represent the structure of an entire program.

An interpreter can then traverse this tree, evaluating each node to execute the program. This approach is called a **tree-walk interpreter**.

My code examples will be written in Python, but you won't need more than a basic understanding of the language to follow along.

## Nodes
A **node** is a representation of a piece of code before it gets executed. It describes something that can be evaluated or executed, such as a literal value, variable reference, or even other parts of a program, such as if statements or function definitions.

### Structure of a Node
Every node in our interpreter will return a value when evaluated, even if that value is `None`. To do this in our tree-walk interpreter, every node will have an `eval()` method, which will take in an environment dictionary (`env = {}`) as an argument. The environment will be used later to store and retrieve values.

Here is an example of a very basic node that we will be using:
```py
class LiteralNode:
    def __init__(self, value):
        self.value = value

    def eval(self, env: dict):
        return self.value
```

This node simply returns the value it was initialized with. If we initialize it as `LiteralNode(10)`, it will evaluate to `10`.

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

Unlike `LiteralNode`, which stores a value directly, `AddNode` takes in two other nodes, which would be its child nodes. When `AddNode.eval()` is called, it evaluates its children and adds their return values together.

This allows us to **nest nodes**, meaning one node can contain other nodes, which themselves can contain even more nodes. For example, consider the expression `1 + (2 + 3)`. We can represent it using nesting like this:
```py
AddNode(
    LiteralNode(1),
    AddNode(
        LiteralNode(2),
        LiteralNode(3)
    )
)
```

When we evaluate the outer node, the following will happen:
1. `LiteralNode(1)` evaluates to `1`.
2. The inner `AddNode` evaluates its children, `LiteralNode(2)` and `LiteralNode(3)`, which return `2` and `3`.
3. The inner `AddNode` adds these values and returns `5`.
4. The outer `AddNode` adds `1` and `5`, returning `6`.

In other words, evaluating a node will recursively evaluate it's children nodes. This structure is the foundation of an **Abstract Syntax Tree**.

## Abstract Syntax Tree
An **Abstract Syntax Tree**, abbreviated to **AST**, is a tree-shaped representation of a program's structure. Each node represents a construct in the program, while its child nodes represent the constructs contained within it.

### Structure of an AST
Consider the expression `1 + (2 + 3)` from earlier. We represented it using nested `AddNode` and `LiteralNode` objects. This structure is an AST.

We can visualize it as a tree:
![An AST representing 1 + (2 + 3)]({{ '/assets/images/ast_example.svg' | relative_url }})

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