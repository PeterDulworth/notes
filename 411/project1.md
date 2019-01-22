# C411

Three main parts:

- Specifying the synatax and semantics of programming languages.
  - syntax: parsing the program text into abstract syntax
  - more detailed parsing is left to 412
  - "recursive descent" style parsers are the style taught in this course



## Parsing

https://www.cs.rice.edu/~javaplt/411/19-spring/Notes/11/02.html

Two main parts:

- lexical analysis
- context-free parsing (or simply parsing)

#### Lexical Analysis

The process of taking an input stream of characters and converting it into a sequence of distinct, meaningful symbols (called *tokens* or *lexemes*) is called *tokenizing*, *lexing* or *lexical analysis*.

e.g.

`(set! x (+ x 1)) -> tokenizer -> ( set! x ( + x 1 ) )`

#### Parsing: Rejecting Illegal Programs

The act of applying the rules of a grammar to determine which sequence of tokens are legal, and of classifying these groups, is called parsing.