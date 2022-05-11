# Module 1: Regular Expressions

## 1.1 Lexical Analysis: Language and Syntax

**Lexer** - checks the syntax of the code in a particular programming language.

Programming languages must have clearly defined syntax. Programmers can learn the syntax and learn what's allowed and what's not. Compiler writers can understand programs and enforce syntax.

Lexer input is a series of bytes, output a series of tokens (meaningful words). A **string** is defined as a finite sequence of symbols from a given alphabet.

We let S be a set of all symbols in an alphabet, then S* := set of all strings in S. A language L is a subset of S*.

## 1.2 Lexical Analysis: Regular Expressions

## 1.3 Lexical Analysis: Concatenation

+ dot: concatenation of strings (combine each string from each set)
+ Two sets A and B: A.B != B.A
+ Operators precedence: . has precedence over |
