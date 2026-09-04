# TinyL Compiler

A recursive descent compiler for the **tinyL** language, written in C. This project implements a complete compiler pipeline including lexical analysis, parsing, and code generation.

## Overview

This compiler translates programs written in the tinyL language into intermediate instructions. TinyL is a simple programming language designed for educational purposes, supporting:
- Variable assignments
- Arithmetic and bitwise operations
- Input/output operations
- Simple statement sequences

## Language Features

TinyL supports the following constructs:

### Grammar (CFG)
```
<program>   ::= <stmt_list> !
<stmt_list> ::= <stmt> <morestmts>
<morestmts> ::= ; <stmt_list> | ε
<stmt>      ::= <assign> | <read> | <print>
<assign>    ::= <variable> = <expr>
<read>      ::= ? <variable>
<print>     ::= % <variable>
<expr>      ::= + <expr> <expr>
              | − <expr> <expr>
              | ∗ <expr> <expr>
              | & <expr> <expr>
              | | <expr> <expr>
              | <variable>
              | <digit>
<variable>  ::= a | b | c | d | e | f
<digit>     ::= 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9
```

### Example Programs
```
a=+2+25;%a!           # Assign (2+25) to a, print a
a=|2&3|25;%a!         # Bitwise operations example
?a;b=*a3;%b!          # Read input, multiply by 3, print result
```

### Operators
- **Arithmetic**: `+` (addition), `-` (subtraction), `*` (multiplication)
- **Bitwise**: `&` (AND), `|` (OR)
- **I/O**: `?` (read), `%` (print)

## Architecture

### Components

1. **Compiler.c** - Main compiler implementation
   - Recursive descent parser (LL(1))
   - Virtual register allocation
   - Code generation using three-address instructions

2. **Instr.h** - Instruction definitions
   - OpCode enumeration
   - Instruction data structure

3. **InstrUtils.c/h** - Instruction utilities
   - Instruction printing and formatting

4. **Utils.c/h** - General utilities
   - Helper functions for error handling and logging

### Compilation Pipeline

1. **Lexical Analysis**: Tokenizes input, skipping whitespace
2. **Parsing**: Recursive descent parser validates grammar and builds computation structure
3. **Code Generation**: Emits three-address instructions with virtual registers

## Building

Compile with:
```bash
gcc -o compiler Compiler.c InstrUtils.c Utils.c
```

## Usage

```bash
./compiler <input_file>
```

**Example:**
```bash
./compiler program.tinyl
```

The compiler reads a tinyL source file and outputs intermediate code to `tinyL.out`.

## Output

The compiler generates a sequence of three-address instructions, where each instruction has:
- An opcode (LOAD, LOADI, ADD, SUB, MUL, AND, OR, STORE, READ, WRITE)
- Up to three operands (fields)
- Virtual register assignments for intermediate results

## Implementation Details

- **Virtual Registers**: Automatically allocated for intermediate expression results
- **Error Handling**: Descriptive error messages for syntax violations
- **Parser Type**: Predictive LL(1) recursive descent parser
- **Token Size**: Exactly one character per token

## Course Context

This project was developed for **CS314: Principles of Programming Languages** as a practical exercise in compiler design and implementation.

## License

Educational use. Created November 2024.
