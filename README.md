![PyCOOLC](misc/pycoolc_logo.png)

A (work in progress) compiler for **[COOL](https://en.wikipedia.org/wiki/Cool_(programming_language))** (**C**lassroom **O**bject **O**riented **L**anguage), targeting the MIPS 32-bit Architecture and written entirely in Python 3.

**COOL** is a small statically-typed object-oriented language that is type-safe and garbage collected. It has mainly 3 primitive data types: Integers, Strings and Booleans (`true`, `false`). It supports conditional and iterative control flow in addition to pattern matching. Everything in COOL is an expression! Many example COOL programs can be found under the [/examples](/examples/README.md) directory.

A formal treatment of **COOL**'s Context-Free Grammar can be found at [/docs/CFG.md](/docs/CFG.md).

------------------------------

## CONTENTS

  * [Overview](#overview).
  * [Development Status](#dev-status).
  * [Requirements](#requirements).
  * [Usage](#usage).
    + [Standalone](#standalone).
    + [Python Module](#python-module).
  * [Language Features](#language-features).
  * [License](#license)

------------------------------

## OVERVIEW

### Architecture:

PyCOOLC follows classical compiler architecture, it consists mainly of the infamous two logical components: Frontend and Backend.

The flow of compilation goes from Frontend to Backend, passing through the stages in every component.

Compiler Frontend consists of the following three stages:
 
 1. Lexical Analysis (see: [`lexer.py`](/pycoolc/lexer.py)): regex-based tokenizer.
 2. Syntax Analysis (see: [`parser.py`](/pycoolc/parser.py)): an LALR(1) parser.
 3. Semantic Analysis (see: [`semanter.py`](/pycoolc/semanter.py)).

Compiler Backend consists of the following two stages:

 * Code Optimization.
 * Code Generation: MIPS 32-bit assembly code. Simulates an SRSM (Single-Register Stack Machine).

### Example Scenario:

A typical compilation scenario would start by the user calling the compiler driver (see: [`pycoolc.py`](/pycoolc/pycoolc.py)) passing to it one or more COOL program files. The compiler starts off by parsing the source code of all program files, lexical analysis, as a stage, is driven by the parser. The parser returns an Abstract Syntax Tree (see: [`ast.py`](/pycoolc/ast.py)) representation of the program(s) if parsing finished successfully, otherwise the compilation process is terminated and errors reported back the user. The compiler driver then initiates the Semantic Analysis stage, out of which the AST representation will be further modified. If any errors where found during this stage, the compilation process will be terminated with all errors reported back. The driver goes on with compilation process, entering the Code Optimization stage where the AST is optimized and dead code is eliminated, after which the Code Generation stage follows, emitting executable MIPS 32-bit assembly code.

## DEV. STATUS

Each Compiler stage and Runtime feature is designed as a separate component that can be used standalone or as a Python module, the following is the development status of each one:

| Compiler Stage     | Python Module                         | Issue                             | Status          |
|:-------------------|:--------------------------------------|:----------------------------------|:----------------|
| Lexical Analysis   | [`lexer.py`](/pycoolc/lexer.py)       | [@issue #2](https://git.io/vr1gx) | :white_check_mark: **done**        |
| Parsing            | [`parser.py`](/pycoolc/parser.py)     | [@issue #3](https://git.io/vr12k) | :white_check_mark: **done**        |
| Semantic Analysis  | [`semanter.py`](/pycoolc/semanter.py) | [@issue #4](https://git.io/vr12O) | *in progress*   |
| Optimization       | -                                     | [@issue #5](https://git.io/vr1Vd) | -               | 
| Code Generation    | -                                     | [@issue #6](https://git.io/vr1VA) | -               |
| Garbage Collection | -                                     | [@issue #8](https://git.io/vof6z) | -               |


## REQUIREMENTS

 * Python >= 3.5.
 * SPIM - MIPS 32-bit Assembly Simulator: [@Homepage](http://spimsimulator.sourceforge.net), [@SourceForge](https://sourceforge.net/projects/spimsimulator/files/).
 * All Python packages listed in: [`requirements.txt`](requirements.txt).


## USAGE

### Standalone

```bash
./pycoolc.py hello_world.cl
```

### Python Module

```python
from pycoolc.lexer import make_lexer
from pycoolc.parser import make_parser

lexer = make_lexer()
lexer.input(a_cool_program_source_code_str)
for token in lexer:
    print(token)
    
 parser = make_parser()
 parsing_result = parser.parse(a_cool_program_source_code_str)
 print(parsing_result)
```

## LANGUAGE FEATURES

  * Primitive Data Types:
    + Integers.
    + Strings.
    + Booleans (`true`, `false`).
  * Object Oriented:
    + Class Declaration.
    + Object Instantiation.
    + Inheritance.
    + Class Features and Methods.
  * Strong Static Typing.
  * Pattern Matching.
  * Control Flow:
    + Switch Case.
    + If/Then/Else Statements.
    + While Loops.
  * Automatic Memory Management:
    + Garbage Collection.

## LICENSE

This project is licensed under the [MIT License](LICENSE).

All copyrights of the files and documents under the [/docs](/docs) directory belong to their original owners.

