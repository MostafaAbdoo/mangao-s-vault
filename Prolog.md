Supported objects include **Integer**, **Real**, **Char**, **String**, **Symbol**, **List**, **Tree**, and **Graph**. At any point during execution, an object state is categorized as either having a value or having no value.
- **Free Variables**: Variables that currently hold no value.
- **Bounded Variables**: Variables that have been bound to a specific value.
- **The Equal Sign (** **=** **)**: If you use `=` with a free variable, Prolog assigns a value to it. If you use `=` with a bounded variable, Prolog compares its existing value against the target value. 
- **Immutability**: A variable takes a value once and cannot be reassigned—_except_ when Prolog performs **backtracking**.
- **Naming & Scope**: Variable names must begin with a **capital letter** or an **underscore** (`_`). A variable's scope is strictly confined to the single rule in which it is defined.
## The Three Required Program Sections
- `predicates`: The declaration section. Here you define the signatures and data types for your facts and rules (e.g., `nondeterm boy(symbol)`).
- `clauses`: The implementation section. This holds the actual knowledge base (facts and rules). Clauses must be grouped by type, and every clause must end with a period (`.`).
- `goal`: The query section. This is the single question or prompt you want Prolog to solve. Execution begins at the goal, and it must end with a period (`.`).
### **Facts: Unconditional Truths**
A fact represents a statement that is always true in your domain.
- **Natural English**: "Ali is a boy" or "Ahmed likes Ali."
- **Prolog Syntax**:
```
boy(ali).
likes(ahmed, ali).

```
Here, `ali` and `ahmed` are values of type `symbol`.

### **Rules: Conditional Truths**
A rule defines a conclusion that is true only if specific conditions are met. In Prolog, rules are written in reverse: the result comes first, followed by the rule symbol `:-` (which reads as "if"), and then the required conditions.
- **Natural English**: "Ahmed likes X, if X is a boy and Ali likes X."
- **Prolog Syntax**:
```
likes(ahmed, X) :-
    boy(X),
    likes(ali, X).

```
- **Operators in Rules**:
    - A **comma (** **,** **)** acts as a logical **AND** (all conditions must be true).
    - A **semicolon (** **;** **)** acts as a logical **OR** (at least one condition must be true).
- _Ali studies whatever Ahmed studies_: `study(ali, X) :- study(ahmed, X).`
- _Any girl studies English_: `study(X, english) :- girl(X).`
- _Math can be studied by either girls or boys_: `study(X, math) :- girl(X); boy(X).`

To allow a predicate to backtrack and return multiple values for the same variable without triggering execution errors, you must declare the predicate with the `nondeterm` keyword in the `predicates` section.
```prolog
predicates 
	nondeterm girl(symbol, integer) 
	nondeterm boy(symbol, integer) 
	nondeterm section(integer, symbol) 

clauses 
	girl(sarah, 19). 
	girl(mona, 20). 
	boy(omar, 19). 
	boy(ahmed, 20). 
	boy(yasser, 21). 
	section(1, kareem). 
	section(1, X) :- boy(X, Y), Y = 20. 
	section(2, soad). 
	section(2, X) :- girl(X, Y), Y < 20. 
	section(2, X) :- boy(X, 19). 
	section(3, X) :- boy(X, 21); girl(X, 21). 

goal 
	section(3, X).
```
---
```prolog
father(X, Y) :- male(X), parent(X, Y).
brother(X, Y) :- male(X), parent(Z, X), parent(Z, Y), X <> Y. % x!= y
```
Consider this rule designed to display all fathers:
`run :- father(X, Y), write(X, " is the father of ", Y), nl, fail.`
It encounters `fail`, Prolog **backtracks** to the last decision point (`father(X, Y)`) and looks for the next solution
Because `run` ends in a failure, Prolog would report `No` or `False` at the console after listing all solutions. To ensure the top-level goal ultimately succeeds after printing everything, programmers add a second, empty `run` clause:
```
run :- father(X, Y), write(X, " is the father of ", Y), nl, fail. 
run.
```

While `fail` forces Prolog to search everywhere, the **Cut operator** (`!`) does the exact opposite: it freezes choices made so far and prevents Prolog from backtracking past the point where the cut appears.
#### **Scenario A: Cut Placement After Price Filter**
```
buycar(M, C) :- car(M, C, P), P &lt;= 25000, !, color(C, nice).
buycar(M, C) :- car(M, C, _), color(C, wonderful).
```
1. Prolog tests `mazda`: `25000 &lt;= 25000` passes. It hits `!`. `color(green, nice)` fails. Entire rule fails.
2. No further searching is allowed because `!` was committed after `mazda` passed the price test.
#### **Scenario B: Cut Placement After Full Clause Validation**
```
buycar(M, C) :- car(M, C, P), P &lt;= 25000, color(C, nice), !.
buycar(M, C) :- car(M, C, _), color(C, wonderful).
```
1. `mazda`: `color(green, nice)` fails. Prolog backtracks _before_ hitting the cut to try the next car!
2. `fiat`: `color(black, nice)` fails.
3. `dogan`: `car(dogan, red, 24000)`. `24000 &lt;= 25000` is true. `color(red, nice)` is **true**!
4. Now Prolog hits `!`. It outputs `M = dogan, C = red` and stops, pruning any remaining searches.
![[Pasted image 20261102044215.png]]
---
The primary tool for generating output is the `write` predicate.
- **Syntax**: `write("abc", X, ...)`

Here are the core input predicates available:
- **readint(X)**: Reads an integer into variable `X`. Evaluates to **false** if the user input is not an integer.
- **readreal(X)**: Reads a floating-point number into variable `X`. Evaluates to **false** if the input is not a real number.
- **readchar(X)**: Captures a single keypress. Note that `readchar` does **not automatically print (echo)** the typed key to the screen; to display the entered character, you must explicitly pass it to `write(X)`.
- **readln(X)**: Reads an entire line of text as a string into variable `X`.
## if-else in prolog
- **Correct Placement (Cut after successful comparison)**:
```
max(X, Y, M) :- X &gt;= Y, !, M = X.
max(X, Y, M) :- M = Y.

```
- Query `max(5, 3, M)`: `5 &gt;= 3` succeeds, hits `!`, commits to rule 1, and sets `M = 5`. Redundant searching for rule 2 is prevented.
- Query `max(3, 5, M)`: `3 &gt;= 5` fails _before_ reaching the cut. Prolog cleanly falls back to rule 2 and sets `M = 5`.
---
## Recursion
- **No Return Values**: To capture a result, we must supply an additional argument—a variable—where Prolog will store the output (e.g., `factorial(X, Y)`, where `Y` holds the result).
- **No Inline Operations**: You cannot pass arithmetic expressions directly as arguments in recursive calls. Writing `factorial(X-1, Y)` is invalid in Prolog. Instead, you must pre-calculate the result into a new variable, such as `Z = X - 1`, before invoking `factorial(Z, Y)`.
```
predicates 
	fact(integer, integer) 

clauses 
	fact(0, 1) :- !. 
	fact(X, Y) :- 
		Z = X - 1, 
		fact(Z, NZ), 
		Y = NZ * X.
```
#### **Tail-Recursive Factorial**

```
predicates
    fact(integer, integer)
    fact_aux(integer, integer, integer)
clauses
    fact(X, F) :- fact_aux(X, F, 1).

    fact_aux(0, F, F) :- !.
    fact_aux(X, F, Acc) :-
        NAcc = Acc * X,
        NX = X - 1,
        fact_aux(NX, F, NAcc).

```

To calculate $X^N$, we multiply $X$ by itself $N$ times:
```
predicates
    power(integer, integer, integer)
    power_aux(integer, integer, integer, integer)
clauses
    power(X, N, P) :- power_aux(X, N, P, 1).
    
    power_aux(_, 0, P, P) :- !.
    power_aux(X, N, P, Acc) :- 
        NAcc = Acc * X,
        Z = N - 1,
        power_aux(X, Z, P, NAcc).
```

Tail-Recursive Fibonacci
```
predicates
    fib(integer, integer)
    fib_aux(integer, integer, integer, integer)
clauses
    fib(X, Res) :- fib_aux(X, Res, 1, 1).
    
    fib_aux(0, A1, A1, _) :- !.
    fib_aux(1, A2, _, A2) :- !.
    fib_aux(X, Res, A1, A2) :- 
        NX = X - 1,
        NA1 = A2,
        NA2 = A1 + A2,
        fib_aux(NX, Res, NA1, NA2).
```