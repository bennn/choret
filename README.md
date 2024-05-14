# Choret

Choret is a set of Racket macros which implement choreographic programming. The current version only implements the communication primitive of *Simple Choreographies* as described in the book "Introduction to Choreographies" by Fabrizio Montesi. It currently uses Racket Threads (https://docs.racket-lang.org/reference/threads.html) to implement the actual execution of Choret programs.

## *Simple Choreographies*

A *Simple Choreography* is created using a macro of the following form:
```
(define-chor [PROCESS-LIST]
    TERM ...)
```
The `PROCESS-LIST` is a list of process names which will be used in the communication terms which describe the Choreography.

Each `TERM` has one of the following forms:
```
TERM := (com-> [SENDER EXPR] [RECIEVER ID])
      | (local-expr PROCESS EXPR ...)
```
The `com->` form implements communication between processes. The `SENDER` is a process name which evaluates the Racket expression `EXPR` and sends the resulting value to `RECIEVER` which stores the result in the variable denoted by `ID`.

The `local-expr` form simply evaluates a series of Racket expressions `EXPR ...` at the process `PROCESS`; no communication occurs.

## *Simple Networks*

An implementation of *Simple Networks* is also provided. It is a lower-level description of the communications between processes; it is more flexible in terms of the allowed communication patterns, but does not give the same correctness guarantees as *Simple Choreographies*. *Simple Choreographies* uses *Simple Networks* as its target backend.

*Simple Networks* is essentially its own language, separate from *Simple Choreographies*. A *Simple Network* is created using the following macro:
```
(define-network 
    TERM ...)
```
Inside a network, the `define process` macro can be used to actually define a "process". 
```
(define-process PROCESS
    TERM ...)
```
`PROCESS` is the name that the process can be referred to as. The terms `TERM ...` inside the process is the code that the process executes.

It should be noted that the term "process" is used loosely here; it simply refers to the abstract concept of a participant in a system of communications. Under the hood, *Simple Networks* uses Racket Threads (which are not real threads even) to implement processes.

To perform (synchronous) communications between processes, the `send` and `recv` macros are used:
```
(send PROCESS TYPE EXPR)
```
`EXPR` is evaluated and its resulting value is sent to the process `PROCESS`. `send` blocks until the receiving process actually receives the value. The `TYPE` form is not used for anything currently.

```
(recv PROCESS TYPE)
```
'recv' blocks until it receives a value from the process `PROCESS`. Again, the `TYPE` form is not used for anything currently.

## Resources related to Choreographies and Racket

1. **"Introduction to Choreographies" by Fabrizio Montesi.** This book is an excellent starting point for those who wish to learn about the theory of Choreographies.
2. **The Official Racket Documentation (https://docs.racket-lang.org/)**
3. **"Beau­tiful Racket: an intro­duc­tion to language-oriented
program­ming using Racket" by Matthew Butt­erick (https://beautifulracket.com/)**
4. **"Fear of Macros" by Greg Hendershott (https://www.greghendershott.com/fear-of-macros/)**
5. **Alexis King's Blog (https://lexi-lambda.github.io/)**