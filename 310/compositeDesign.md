# Composite Design

**Recursion: Finite Infinity**

Two basic types:

- structural recursion
- algorithmic recursion

| Abstraction         | Base Case(s)                  | Inductive Case(s)                |
| ------------------- | ----------------------------- | -------------------------------- |
| Chemicals           | Atom                          | Molecule                         |
| Lego Creation       | Individual Lego Block         | A bunch of blocks stuck together |
| Tree Processing Alg | Leaf node case                | Interior node case               |
| GUI                 | Button, text field, label,... | panel, frame, ...                |





```sequence
Title: Composite Design
participant Super
participant Inductive
participant Concrete2
participant Concrete1
Concrete1->Super:
Concrete2->Super:
Inductive->Super:
Inductive->>Super:
```

