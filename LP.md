# Lecture 3
1- **Constants**: Fixed values that represent specific objects or quantities.
- **Atoms**: Symbolic identifiers used to name objects or relationships. An atom can be:
    - A sequence of characters starting with a **lowercase letter** (e.g., `butch`, `big_kahuna_burger`, `playGuitar`).
    - An arbitrary sequence of characters enclosed in **single quotes** (e.g., `'Vincent'`, `'Five dollar shake'`, `'@$%'`).
    - A sequence of special characters (e.g., `:-`, `:`, `,`, `.`).
- **Numbers**: Numerical values, which can be **Integers** (e.g., `12`, `-34`) or **Floats** (e.g., `34573.3234`).
2- **Variables**: Dynamic placeholders used to query the knowledge base or stand in for unknown entities.
- A variable **must begin with an uppercase letter** or an **underscore** (e.g., `X`, `Y`, `Variable`, `Vincent`, `_tag`, `_123`).
- If a name starts with a lowercase letter, Prolog treats it as an atom, not a variable! For instance, `john` is an atom, while `John` is a variable.

- **Functor**: Must always be an atom (e.g., `date`, `loves`, `owns`).
- **Arguments**: Can be any Prolog term, including numbers, variables, atoms, or even other nested complex terms (e.g., `owns(john, book(Title, author(leguin, GivenName)))`).
- **Arity**: The number of arguments a complex term accepts is called its **arity**. In Prolog documentation, predicates are uniquely identified by their functor name and arity using the notation `functor/arity` (e.g., `happy/1`, `loves/2`).
Prolog treats two predicates with the same functor name but different arities as entirely distinct predicates!
- **Read Functions**:
- `readint(Var)`: Reads an integer entered by the user and binds it to `Var`.
- `readchar(Var)`: Reads a single character.
- `readreal(Var)`: Reads a floating-point (decimal) number.
- `readln(Var)`: Reads a full string up to a line break.
- **Write Function**:
- `write(Var)`: Outputs a variable or string of any type to the console.
all mathematical functions starts with a capital letter
## The Three Formal Rules of Unification
To evaluate whether any two terms $T_1$ and $T_2$ unify, follow these three rules:
1. **Constants**: If $T_1$ and $T_2$ are constants, they unify **if and only if** they are the exact same atom or the exact same number.
    - `mia` and `mia` $\rightarrow$ **Unify** (identical atom).
    - `mia` and `vincent` $\rightarrow$ **Fail** (different atoms).
2. **Variables**: If $T_1$ is a variable and $T_2$ is any term, $T_1$ and $T_2$ unify, and $T_1$ is **instantiated** to $T_2$ (and vice versa).
    - `mia` and `X` $\rightarrow$ **Unify** (instantiates $X = \text{mia}$).
    - `woman(Z)` and `woman(mia)` $\rightarrow$ **Unify** (instantiates $Z = \text{mia}$).
3. **Complex Terms**: If $T_1$ and $T_2$ are complex terms, they unify **if and only if**:
    - They have the **same functor** and the **same arity**.
    - **All corresponding arguments unify** sequentially.
    - The resulting variable instantiations are **compatible**
**Example 1: Single Complex Term Unification** Query: `?- k(s(g), Y) = k(X, t(k)).`
- Functors match (`k`) and arities match (`2`).
- Argument 1: `s(g)` unifies with variable `X` $\rightarrow X = s(g)$.
- Argument 2: variable `Y` unifies with `t(k)` $\rightarrow Y = t(k)$.
- **Result**: `X = s(g), Y = t(k)`. Success!
**Example 2: Variable Conflict and Unification Failure** Query: `?- mia = X, X = vincent.`
- First goal (`mia = X`): Variable `X` instantiates to `mia` ($X = \text{mia}$).
- Second goal (`X = vincent`): Because `X` is already bound to `mia`, Prolog attempts to unify `mia = vincent` under Rule 1.
- **Result**: **Fail**. `mia` and `vincent` are distinct atoms, so the overall query fails.
# Lecture 4
we might define variable $A$ as _"The car will start"_ or $B$ as _"The battery is dead"_. Compound statements are built using logical operators:
- **AND** ($\land$, $\&$, $\cap$): True only if both propositions are true.
- **OR** ($\lor$, $\cup$, $+$): True if at least one proposition is true.
- **NOT** ($eg$, $\sim$): Reverses the truth value of a proposition.
- **IMPLIES** ($\supset$, $\rightarrow$): Represents conditional rules (e.g., $A \rightarrow B$).
- **EQUIVALENT** ($\equiv$, $\leftrightarrow$): True if both propositions share the same truth value.
_"If students work hard (_$A$_), always come to lectures (_$B$_), and do all their homework (_$C$_), then they will get an A (_$D$_)."_ In Propositional Logic, we express this as: $A \land B \land C \rightarrow D$ 
	- _"Johan comes (_$p$_) just when Peter stays at home (_$q$_)":_ $p \leftrightarrow q$
		- _"Even though it is not cloudy (_$\sim p$_), it is still raining (_$q$_)":_ $\sim p \land q$
- **Universal Quantifier (**$\forall$**):** Meaning _"for all"_. (e.g., $\forall X \, \text{likes}(X, \text{mary})$ means _"everybody likes Mary"_).
- **Existential Quantifier (**$\exists$**):** Meaning _"there exists"_. (e.g., $\exists X \, \text{likes}(X, \text{mary})$ means _"someone likes Mary"_).
By combining quantifiers and logical connectives, we can express nuanced concepts. For instance: $\forall X (\text{likes}(X, \text{mary}) \land \text{nice}(\text{mary}) \rightarrow \text{nice}(X))$

Suppose our goal is to place block `b` onto block `a` (`put_on(b, a)`). To accomplish this, the robot arm must hold block `b` and ensure block `a` is clear:$\text{hand\_holding}(b) \land \text{clear}(a) \rightarrow \text{put\_on}(b, a)$

To generalize this for _any_ block $X$ and target block $Y$, we express the rule using quantifiers: $\forall X \, \exists Y (\text{hand\_holding}(X) \land \text{clear}(Y) \rightarrow \text{put\_on}(X, Y))$ 

---
1. **r(X, Y) :- !, f1(X), f2(Y).** The Cut executes immediately. Prolog commits to this clause rule right away, but it can still backtrack between `f1(X)` and `f2(Y)` to generate pairs like `(a, d)`, `(a, f)`, `(b, d)`, etc.
2. **r(X, Y) :- f1(X), !, f2(Y).** Prolog evaluates `f1(X)` and binds `X = a`. It then passes the Cut. The choice `X = a` is now **locked in**. Prolog will check `f2(Y)` to produce `(a, d)`, `(a, f)`, and `(a, k)`. Once `f2` is exhausted, Prolog **cannot** backtrack to `f1` to try `X = b`. Search ends
3. **r(X, Y) :- f1(X),  f2(Y), !.** Once a successful pair of `X` and `Y` is found and the execution reaches the cut at the end, Prolog **locks in that single success**.

`fail` acts as an explicit instruction to **force backtracking**
# Lecture 5
**Green Cuts**: A green cut is inserted purely for **efficiency**. Removing a green cut does not change the logical answers returned by the program; it only eliminates unnecessary backtracking. The clauses are already mutually exclusive by their conditions.
**Red Cuts**: A red cut alters the **declarative meaning** of the program. If a red cut is removed, the program produces incorrect or unintended answers. The correctness of subsequent clauses relies entirely on the cut stopping execution of preceding clauses.

```
not_(G) :- G, !, fail.
not_(_).
```

1. If goal `G` succeeds: the cut (`!`) commits to this clause, and `fail` forces the goal `not_(G)` to fail.
2. If goal `G` fails: Prolog moves to the second clause, which immediately succeeds.
**Cut-Fail as the "Except" Pattern**
```
% Step 1 & 2: Write the exception first, followed by cut-fail
love(pasta) :- !, fail.

% Step 3: Write the general rule last
love(X) :- food(X).
```
![[Pasted image 20261109061813.png]]

- **Tautology**: A statement that is **always true** under every interpretation 
	(e.g., $p \lor \neg p$).
- **Contradiction**: A statement that is **always false** under every interpretation 
	(e.g., $p \land \neg p$).
- **Contingency**: A statement whose truth value depends on the truth values of its component variables (e.g., $p \rightarrow q$).
- **De Morgan's Laws**: $\neg(p \land q) \equiv \neg p \lor \neg q$  ---- $\neg(p \lor q) \equiv \neg p \land \neg q$ 
- **Implication Law**: $p \rightarrow q \equiv \neg p \lor q$
- **Double Negation**: $\neg\neg p \equiv p$
- **Biconditional**: $p \leftrightarrow q \equiv (p \rightarrow q) \land (q \rightarrow p)$ 

- **Converse** ($q \rightarrow p$): _"If the road is wet, it rained."_
- **Inverse** ($\neg p \rightarrow \neg q$): _"If it does not rain, the road is not wet."_
- **Contrapositive** ($\neg q \rightarrow \neg p$): _"If the road is not wet, it did not rain."_
A conditional statement is **always equivalent to its contrapositive** 

- **Deduction (General** $\rightarrow$ **Specific)**: Derives specific, certain conclusions from general rules. If premises are true, the conclusion _must_ be true. Prolog uses deductive resolution.
    - _Example_: All humans are mortal $\rightarrow$ Socrates is human $\rightarrow$ Socrates is mortal.
- **Induction (Specific** $\rightarrow$ **General)**: Infers general rules from specific repeated observations. The conclusion is probable but not guaranteed.
    - _Example_: Swan 1 is white, Swan 2 is white $\rightarrow$ All swans are white.
- **Abduction (Observation** $\rightarrow$ **Best Explanation)**: Works backward from an outcome to infer the most likely cause.
    - _Example_: The grass is wet $\rightarrow$ It probably rained.

The **Find-S algorithm** finds the most specific hypothesis $h$ that fits all **positive** training instances in an attribute-based dataset.
## Step-by-Step Example: Identifying Poisonous Seeds

Consider learning whether seeds are poisonous based on four attributes: `[COLOR, TOUGHNESS, FUNGUS, APPEARANCE]`.

1. **Initialization**: $h_0 = \langle \emptyset, \emptyset, \emptyset, \emptyset \rangle$
2. **Example 1** `[GREEN, HARD, NO, WRINKLED]` (Label: **YES**):
    - Update $h_1 = \langle \text{GREEN}, \text{HARD}, \text{NO}, \text{WRINKLED} \rangle$
3. **Examples 2 & 3** (Label: **NO**):
    - Ignore negative instances. $h_2 = h_1$.
4. **Example 4** `[ORANGE, HARD, NO, WRINKLED]` (Label: **YES**):
    - Color mismatches (`GREEN` vs `ORANGE`) $\rightarrow$ generalize to `?`.
    - $h_3 = \langle ?, \text{HARD}, \text{NO}, \text{WRINKLED} \rangle$
5. **Example 5** `[GREEN, SOFT, YES, WRINKLED]` (Label: **YES**):
    - Compare with $h_3$: Toughness (`HARD` vs `SOFT`) $\rightarrow$ `?`, Fungus (`NO` vs `YES`) $\rightarrow$ `?`.
    - $h_4 = \langle ?, ?, ?, \text{WRINKLED} \rangle$

**Final Hypothesis**: $h = \langle ?, ?, ?, \text{WRINKLED} \rangle$ (i.e., _"Any seed with a wrinkled appearance is poisonous"_).