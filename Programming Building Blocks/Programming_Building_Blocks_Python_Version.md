# Table of Contents

- [Statement Sequence](#statement-sequence)
- [Branching](#branching)
  - [Branching Statements](#branching-statements)
    - [Basic Concepts](#basic-concepts)
    - [Examples](#examples)
  - [Expression Evaluation](#expression-evaluation)
    - [Boolean Expressions as Conditions](#boolean-expressions-as-conditions)
    - [Side Effect of Expressions](#side-effect-of-expressions)
    - [Expression Evaluation Order](#expression-evaluation-order)
      - [Operator Precedence](#operator-precedence)
      - [Operator Associativity](#operator-associativity)
      - [Operand Evaluation Order](#operand-evaluation-order)
      - [Further Reading(Optional)](#further-readingoptional)
    - [Short-Circuit Evaluation](#short-circuit-evaluation)
    - [Applying Mathematical Logic](#applying-mathematical-logic)
      - [De Morgan’s Laws](#de-morgans-laws)
      - [Distributive Laws](#distributive-laws)
      - [Absorption Laws](#absorption-laws)
      - [Exclusive OR (XOR)](#exclusive-or-xor)
  - [Writing Good Branching Code](#writing-good-branching-code)
    - [Invert Complicated Conditions](#invert-complicated-conditions)
    - [Treat Overlapping Conditions Carefully](#treat-overlapping-conditions-carefully)
    - [Reordering Branching Code](#reordering-branching-code)
      - [Basic Ideas](#basic-ideas)
      - [Moving Likely Branches Closer to the Top](#moving-likely-branches-closer-to-the-top)
      - [Moving Likely Expressions Closer to the Left](#moving-likely-expressions-closer-to-the-left)
      - [Putting Simplest or Cheapest Checks First](#putting-simplest-or-cheapest-checks-first)
    - [Use Guard Clauses to Flatten Nested Branches](#use-guard-clauses-to-flatten-nested-branches)
  - [Recommended Leetcode Problems](#recommended-leetcode-problems)
- [Loop/Iteration](#loopiteration)
  - [The Idea of Repetition](#the-idea-of-repetition)
  - [Loop Statements](#loop-statements)
  - [Methodical Loop Analysis](#methodical-loop-analysis)
    - [Elements of a Loop](#elements-of-a-loop)
    - [Loop Goal](#loop-goal)
    - [Loop Premise](#loop-premise)
    - [Loop Initialization](#loop-initialization)
    - [Loop Conditions](#loop-conditions)
    - [Loop Termination](#loop-termination)
    - [Iteration Goal](#iteration-goal)
    - [Loop Body](#loop-body)
    - [Edge Cases](#edge-cases)
    - [Loop Variant](#loop-variant)
    - [Loop Invariant](#loop-invariant)
      - [Definition](#definition)
      - [Importance](#importance)
      - [Loop Variable Semantics](#loop-variable-semantics)
      - [Good Questions to Ask](#good-questions-to-ask)
      - [More Examples](#more-examples)
  - [Methodical Loop Implementation](#methodical-loop-implementation)
    - [From Analysis to Implementation](#from-analysis-to-implementation)
    - [Bubble Sort](#bubble-sort)
      - [High-Level Design](#high-level-design)
      - [Mid-Level Design](#mid-level-design)
      - [Outer Loop Implementation](#outer-loop-implementation)
      - [Inner Loop Implementation](#inner-loop-implementation)
      - [Complete Implementation](#complete-implementation)
      - [Property-Based Testing](#property-based-testing)
      - [Define Variables Wisely](#define-variables-wisely)
      - [Optimization](#optimization)
    - [Insertion Sort](#insertion-sort)
      - [High-Level Design](#high-level-design)
      - [Mid-Level Design](#mid-level-design)
      - [Outer Loop Implementation](#outer-loop-implementation)
      - [Inner Loop Implementation](#inner-loop-implementation)
      - [Complete Implementation](#complete-implementation)
      - [Alternative Implementation and Optimization](#alternative-implementation-and-optimization)
        - [Alternative Search while Shifting](#alternative-search-while-shifting)
        - [Adjacent Swapping](#adjacent-swapping)
        - [Linear Search then Shifting](#linear-search-then-shifting)
        - [Binary Search then Shifting](#binary-search-then-shifting)
    - [Three-Way Partitioning](#three-way-partitioning)
  - [Recommended Leetcode Problems](#recommended-leetcode-problems)
- [Recursion](#recursion)
- [Documentation](#documentation)
  - [Block/Loop Documentation](#blockloop-documentation)
  - [Function/Method Documentation](#functionmethod-documentation)
  - [Reference Materials](#reference-materials)
- [Master Your Arsenal](#master-your-arsenal)
  - [Built-In Library & Functions](#built-in-library-functions)
  - [Classic Algorithms & Data Structures](#classic-algorithms-data-structures)
  - [Build Your Own Templates](#build-your-own-templates)

A statement is a complete instruction that performs some action. In Python (an imperative programming language), statements are the basic unit of execution. Common types of statements include declarations, assignments, and function calls.

A **statement sequence** (also called sequential execution) means the computer executes statements one after another, in the exact order they appear, from top to bottom. This is the default behavior of a program: without any control flow, it simply runs each line in sequence.

In practice, it’s difficult to implement anything complex or interesting using only statement sequences without control flow. Such purely sequential code usually only works for very simple, short tasks. For example:

# Statement Sequence

```java
def statement_sequence_example():
    # Suppose we have two variables and we want to exchange their values
    x = 5
    y = 7
    print(f"x: {x}, y: {y}")

    # Swap using a temporary variable
    temp = x
    x = y
    y = temp
    print(f"x: {x}, y: {y}")

    # Pythonic way to swap variables
    x, y = y, x
    print(f"x: {x}, y: {y}")

statement_sequence_example()
```

Program control mechanisms such as branching, loops and recursion interrupt or modify this basic sequential flow, as we will discuss below.
1. **Sequential Execution**: Python runs statements line by line, from top to bottom — this is the default behavior.
2. **Branching**: Choose which block of statement sequence to execute based on some condition.
3. **Looping**: Repeat a block of statements multiple times.
4. **Recursion**: A function calls itself with new input values.

Other control flow constructs (such as `goto`) exist, but they are generally discouraged and rarely needed in competitive programming, so we won’t cover them here.

# Branching

## Branching Statements

### Basic Concepts

Branching is about making decisions in a program: perform different actions under different conditions, meaning the program can execute different blocks of code depending on the conditions. Branching lets a program react dynamically to various inputs and values. This concept is also known as **Conditional Statements** or **Selection**.

In Python, branching is done using `if`, `elif`, and `else` statements. Some languages support `switch` statements, but Python does not have `switch`; instead, more recent versions (3.10+) support `match`, which we won’t cover here. Branching statements can be combined and nested to form complex decision logic. For example:

```python
if condition_1:
    # Statements_1 to be executed if condition_1 is true
elif condition_2:
    # Statements_2 to be executed if condition_2 is true and all previous conditions are false
elif condition_3:
    # Statements_3 to be executed if condition_3 is true and all previous conditions are false
elif condition_n:
    # Statements_N to be executed if condition_n is true and all previous conditions are false
else:
    # Default statements to be executed in case all above conditional expressions are false
```

For complicated branching statements, you could use a **decision tree** to visualize it. Below is an example decision tree, and each decision node (often drawn as a diamond in flowcharts) could be further broken into more branches if conditions involve multiple parts or variables. For instance, if `condition_1` is composed of $N$ boolean expressions and involves $M$ variables, that single decision can be expanded into a sub-tree exploring all combinations of those sub-expressions.

<img src="img/branching_statement_as_decision_tree.png" alt="branching_statement_as_decision_tree" style="width: 70%; height: 70%; display: block; margin-left: auto; margin-right: auto;">

From a mathematical perspective, **set theory** can help determine which branch executes for a given set of variable assignments, and **Venn diagrams** can illustrate how those sets overlap. For example, consider a Venn diagram for the branching logic above:

<img src="img/venn_diagram_for_branching_statement.png" alt="venn_diagram_for_branching_statement" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

In this diagram, there are nine distinct regions, formally called *atomic equivalence classes*, representing different combinations of variable assignments for the branching code. 

Notice that the **order of the conditions** matters. In Python, conditions in a chain of `if` / `elif` / `else` are evaluated top to bottom, and only the first `True` condition triggers its block. The rest are skipped. This behavior is called **short-circuit** evaluation.

- Variable assignments corresponding to A1, A5, A4, A7 will cause Statement_1 to execute.
- Variable assignments corresponding to A2, A6 will cause Statements_2 to execute.
- Variable assignments corresponding to A3 will cause Statements_3 to execute.
- Variable assignments corresponding to A8 will cause Statements_N to execute.
- Variable assignments corresponding to A9 will cause the default statements to execute.

Different programmers might implement the same branching logic in different but logically equivalent ways, which is one of the reasons why another programmer's implementation of the same algorithm may look drastically different than yours. Understanding how branching works not only helps you write better code but also makes it easier to read and interpret other people’s code.

### Examples

**Example 1**

```python
# Code Block 1
if condition_1:
    if condition_2:
        # Statements to be executed
```

```python
# Code Block 2
if condition_1 and condition_2:
    # Statements to be executed
```

Do you think the above two branching code blocks are equivalent? If so, which one do you think is better?
<details>
<summary>Click to show the answer</summary>
Yes, they are always functionally equivalent, even if the conditions have side effects or involve method calls.<br/>
In this case, <code>Code Block 2</code> is preferable for clarity and conciseness.<br/>
But in practice, you might want to execute some code whenever <code>condition_1</code> is true and regardless of <code>condition_2</code>. In this case you could insert code inside the outer <code>if</code> but outside the inner <code>if</code>, so <code>Code Block 1</code> gives you more flexibility.<br/>
Also notice that due to short-circuit evaluation, in <code>Code Block 2</code>, if <code>condition_1</code> evaluates to false, then <code>condition_2</code> won't be evaluated.
</details>

**Example 2**
```python
# Code Block 3
if condition_1:
    if condition_2:
        # Statements_A to be executed
    if condition_3:
        # Statements_B to be executed
```

```python
# Code Block 4
if condition_1 and condition_2:
    # Statements_A to be executed
if condition_1 and condition_3:
    # Statements_B to be executed
```

What about the above pair of code blocks?
<details>
<summary>Click to show the answer</summary>
They are equivalent <b>only if</b> evaluating <code>condition_1</code> has no side effects. If <code>condition_1</code> does have side effects, then they are <b>not</b> equivalent.<br/>
Even when they are logically equivalent, <code>condition_1</code> will always be evaluated twice in <code>Code Block 4</code>, so if the evaluation of <code>condition_1</code> is expensive, then <code>Code Block 3</code> is preferred.<br/>
In <code>Code Block 4</code>, even though <code>condition_1</code> is evaluated in the fist <code>if</code>, the program does not store that result and will evaluate it again in the second <code>if</code>.
</details>

**Example 3**

```python
# Code Block 5
if condition_1:
    if condition_2:
        # Statements_A to be executed
    elif condition_3:
        # Statements_B to be executed
```

```python
# Code Block 6
if condition_1 and condition_2:
    # Statements_A to be executed
if condition_1 and condition_3:
    # Statements_B to be executed
```

Do you think the above pair of code blocks are equivalent?
<details>
<summary>Click to show the answer</summary>
No, they are not equivalent.<br/>
In <code>Code Block 5</code>, at most one branch can execute, whereas in <code>Code Block 6</code>, both branches might execute. For example, if both <code>condition_2</code> and <code>condition_3</code> are true, <code>Code Block 5</code> will execute only Statements_A (since the <code>elif</code> prevents the second check when the first succeeds), but <code>Code Block 6</code> will execute both Statements_A and Statements_B.<br/>
Additionally, in <code>Code Block 6</code>, <code>condition_1</code> will always be evaluated twice.
</details>

**Example 4**

```python
# Code Block 7
if condition_1:
    # Statements_A to be executed
elif condition_2:
    # Statements_A to be executed
```

```python
# Code Block 8
if condition_2:
    # Statements_A to be executed
elif condition_1:
    # Statements_A to be executed
```

```python
# Code Block 9
if condition_1 or condition_2:
    # Statements_A to be executed
```

```python
# Code Block 10
if condition_2 or condition_1:
    # Statements_A to be executed
```

```python
# Code Block 11
if condition_1 | condition_2:
    # Statements_A to be executed
```

```python
# Code Block 12
if condition_2 | condition_1:
    # Statements_A to be executed
```

Which pairs of the above code blocks are equivalent? Why?
<details>
<summary>Click to show the answer</summary>
<code>Code Block 7</code> and <code>Code Block 9</code> are always equivalent to each other. They are equivalent to the other versions only when both conditions have no side effects.<br/>
<code>Code Block 8</code> and <code>Code Block 10</code> are always equivalent to each other. They are equivalent to the other versions only when both conditions have no side effects.<br/>
For <code>Code Block 11</code> and <code>Code Block 12</code>, both conditions will be evaluated (because they use a bitwise OR operator <code>|</code>, which does not short-circuit). Whether they are equivalent depends on whether they have side effects and their nature. If they have no side effects, then they are equivalent. Otherwise, the execution order between them matters and can change the outcome. (For example, adding 5 then multiplying by 5 yields a different result than multiplying then adding 5.)<br/><br/>
To conclude, if both conditions have no side effects, all the code blocks produce the same logical result, but <code>Code Block 11</code> and <code>Code Block 12</code> will still evaluate both conditions. If side effects are present, you have to analyze each case separately. This complexity is one reason refactoring code can be tricky: if a boolean-returning function has side effects and you change those effects, you must review every place that calls it.
</details>

**Practical Examples**
One simple use of branching is determining whether an integer is odd or even:

```java
def isOdd(num):
    return num % 2 != 0

def isEven(num):
    return not isOdd(num)

def oddityExample():
    print(f"Is 5 odd: {isOdd(5)}")
    print(f"Is 10 even: {isEven(10)}")


oddityExample()
```

Not all branching programs are as straightforward as the one above.
The complexity of a conditional block depends on several factors — such as the complexity of each predicate, the number of branches, and the depth of nesting. For instance:

```python
# What is even happening?
if (
    ((not a or b) and c and not (d and e)) or
    ((f and (not g or h)) and not (i and j and not k)) or
    l
):
    doSomething()
```


The potential for an **exponential explosion of paths** in complex conditions can make branching code difficult to write and reason about. For example, tax software like Sprintax or TurboTax needs to handle tons of complex branching to calculate taxes and refunds accurately.

In competitive programming practice, you will definitely encounter branching logic that is not so obvious or simple. As an exercise, consider implementing a function to check whether a given date (day, month, year) is valid. Note that this problem could be solved using only branching without loop or recursion.

```java
def isValidDate(day, month, year):
    # TODO: Add branching logic to validate day/month/year
    return True

# Test cases
print(isValidDate(29, 2, 2020))  # true (leap year)
print(isValidDate(29, 2, 2021))  # false
print(isValidDate(31, 4, 2022))  # false (April has 30)
print(isValidDate(31, 12, 2023)) # true
print(isValidDate(0, 1, 2024))   # false
print(isValidDate(15, 13, 2024)) # false
```

## Expression Evaluation

### Boolean Expressions as Conditions

The behavior of a branching statement depends on its conditions, which are boolean expressions that evaluate to either `True` or `False`. These boolean expressions can take several forms:
- Boolean literals `True` and `False` used directly in boolean expressions. For example, some loops start with `while True`.
- Boolean variables that hold boolean value. For example, `v = True`.
- Comparison operators such as `==`, `!=`, `<`, `>`, `<=`, `>=` that take two operands and evaluates to a boolean value.
- Logical Operators such as `and`, `or`, `not`, `^`, `&`, `|` to combine one or more boolean expressions.
- Function calls that return boolean value.

You are supposed to be familiar with the meanings and usage of the above operators.

It's worth noting that boolean expressions are used not just as branching conditions, but in many places in code for making decisions:
- `if`, `elif`, `while`, `for`
- `assert` (used for debugging/testing). Python does not have a built-in switch statement

They also determine when loops continue or stop .

Python has an inline if-else expression, similar to the ternary operator in other languages.
`value_if_true if condition else value_if_false`
- If `condition` is `True`, Python evaluates and returns the first expression.
- If `condition` is `False`, it returns the second expression.

The inline if-else expression is often used for concise conditional assignments. For example:
```python
max_val = n1 if n1 > n2 else n2
```

### Side Effect of Expressions

**Side Effects** are an important concept that goes beyond boolean expressions, and extend to all expression evaluation. In the context of **expression evaluation**, a side effect is **any observable change in state** that occurs in addition to producing a value. This includes modifying variables, changing an object’s state, performing I/O, etc.
- Expressions with no side effects are pure: they just compute and return a value without altering any program state.
- Expressions with side effects modify state (memory, I/O, etc.) or trigger something beyond just computing a value.

Side Effects stem from modification operations that write or mutate variables. "Read-only" operations are expressions that access state but do not change it, and thus are free of side effects.

```python
x = 5;
result = (x > 0)   # just evaluates a boolean, no state change
```

```python
def method1(x):
    return x + 1

x = 0
result = (method1(x) > 0)  # method1 has no side effect
```

```java
def method2(lst):
    lst[0] = 1
    return lst[0]

nums = [0, 1]
result = (method2(nums) > 0)  # method2 has side effects
```

```python
x = 0
nums = [0, 1]

# do_something() is assumed to be a placeholder for some side-effectful action
def do_something():
    print("Action triggered")

if method1(x) > 0 or method2(nums) > 0:
    do_something()

if method2(nums) > 0 or method1(x) > 0:
    do_something()
```

Side Effects matter in expression evaluation because they could introduce bugs if not handled carefully:
- **Reordering**: If the compiler or programmer changes the evaluation order, side effects might occur in a different order.
- **Short-Circuiting**: as shown above, side effects may or may not occur because of short-circuiting, so you should avoid relying on side effects in short-circuited expressions.
To keep program behavior predictable and code clear, it’s often best to avoid embedding important side effects in complex boolean expressions. If a method call or operation has a significant side effect, consider doing it in a separate statement. 

### Expression Evaluation Order

**Expression Evaluation Order** is determined by several factors, mainly **operator precedence**, **operator associativity** and **operand/subexpression evaluation order**. It can be tricky and confuse a lot of people. To conclude:
- Operator precedence and associativity in Python (as in many languages) determine how expressions are grouped — that is, how the interpreter builds the abstract syntax tree (AST). However, they do not specify the exact runtime order in which subexpressions are evaluated and when side effects (such as `x += 1`) happen.
- Python evaluates function arguments and operands from **left to right**, and side effects (such as `x += 1` or printing) occur as each part is evaluated. This order is clearly defined in the Python language specification and applies consistently across most constructs.

#### Operator Precedence

Operator Precedence determines which operator binds first when two different operators appear together without parentheses. For example, `*` and `/` take precedence over `-` and `+`. You can find Python's operator precedence table here: https://docs.python.org/3/reference/expressions.html#operator-precedence

If you are unsure of operator precedence, you can always use parentheses to make the order explicit. For boolean expressions, using parentheses to remove ambiguity is a good practice for readability. For instance:


```python
# When there is only one type of logical operator, there is no ambiguity.
if E_1 and E_2 and E_3 and E_4:
    # Statements
```

```python
# If you don't remember the precedence between 'and' and 'or', then you can't be sure how this boolean expression will evaluate.
if E_1 or E_2 and E_3 or E_4：
    # Statements
```

```python
# Use parentheses to eliminate ambiguity and make your code more readable.
if ((E_1 or E_2) and (E_3 or E_4)):
    # Statements
# OR
if (((E_1 or E_2) and E_3) or E_4):
    # Statements
# OR other ways
```

#### Operator Associativity

**Operator Associativity** is about how operators of the same precedence are grouped in the absence of parentheses. When an expression has multiple operators with the same precedence, associativity (either left-to-right or right-to-left) tells us how to group them.

For example, `100 / 2 / 5` is interpreted as `(100 / 2) / 5` (left-to-right associativity for division) rather than `100 / (2 / 5)`. Similarly, `100 * 2 / 5` is treated as `(100 * 2) / 5` not `100 * (2 / 5)`, since `*` and `/` share the same precedence and are left-associative.

Most Python operators are left-to-right associative, but the assignment operators (`=`, `+=`, `-=`, etc) are right-to-left associative. For example, the expression `x = y = z = 5` is grouped as `x = (y = (z = 5))`, due to right-to-left associativity.

#### Operand Evaluation Order

Operator precedence and associativity determine how Python groups operands and operators, but they do not determine the order in which operands are evaluated. By determining grouping, they in turn constrain the order of execution, but only to the extent required by the grouping. So grouping constrains evaluation order only partially — structurally, not in terms of actual timing or sequencing.

For example, consider the expression `a[A()] + B() + C()`. Even though the grouping is `(a[A()]) + (B() + C())`, Python still evaluates `A()` first, then `B()`, then `C()` — from left to right.

For another example, `result = 2 + 3 + 4 * 5`, due to operator precedence and associativity:
- `*` has higher precedence -> grouped as `2 + 3 + (4 * 5)`
- `+` is left-associative -> grouped as `((2 + 3) + (4 * 5))`

You could replace the literals with method calls or complex expressions. Even in such cases, grouping constrains the structure of evaluation, but does not explicitly dictate the exact runtime order. That order is defined by the Python language specification, which guarantees left-to-right evaluation of operands and function arguments. In contrast, some languages like C or C++ do not specify operand evaluation order.

**Operand/Subexpression Evaluation Order** determines the order in which operands or subexpressions are evaluated.

In Python, the evaluation order for most expressions — including function arguments and operands — is guaranteed to be left-to-right. However, due to short-circuit evaluation, not all subexpressions may be evaluated when logical operators like `and` and `or` are used.

```python
a = A(B(), C(), D())  # Python evaluates arguments left to right

# Short-circuiting applies to logical expressions.
# If B() returns True, then C() and D() won't be evaluated at all.
if B() or C() or D():
    do_something()
```

There are many other details about expression evaluation order, but a general rule of thumb is: **do not rely on the programming language to resolve ambiguous evaluation order for you**. If your program's correctness depends on certain expressions being evaluated in a specific order, it's better to make that order explicit — for example, by breaking the expression into smaller parts and using temporary variables.

For example, if in the earlier example you really need `B()` to execute before `C()`, and `C()` before `D()`, then you should write:
```python
b = B()
c = C()
d = D()
a = A(b, c, d)
```

Notice that Python guarantees left-to-right evaluation for most expressions, but it's still good practice to write code that is clear and unambiguous. In contrast, some other languages like C and C++ deliberately leave evaluation order undefined to allow for optimizations.

#### Further Reading(Optional)

The nuances of operator precedence, associativity, and evaluation order can get very complex. For a deeper understanding in Python, you can refer to the official Python documentation:
- [Python Operator Precedence](https://docs.python.org/3/reference/expressions.html#operator-precedence)
- [Python Language Reference on Evaluation Order](https://docs.python.org/3/reference/expressions.html#evaluation-order)

While Python's rules are generally simpler and more predictable than those in Java or C/C++, you should still avoid writing convoluted expressions that combine many side effects or rely on subtle evaluation behavior.

Consider the following tricky Python expression:
```python
a = [4, 4]
b = 1
a[b] = (b := 0)
```
The result of the above line is that `a[1]` becomes `0`. It takes some careful thought to understand why: although the right-hand side updates `b` to `0`, the original `b` (which was `1`) is still used as the index on the left-hand side before the reassignment. This demonstrates how mixing assignment and indexing in a single line can produce surprising results.

The takeaway is to avoid writing code that relies on subtle evaluation order. Don't combine mutations and indexing in ways that make the evaluation order unclear.

For example, avoid code like:
```python
a[j] = a[j + 1]; j += 1  # ambiguous and hard to reason about
```
or:
```python
a[j] = (j := j + 1)  # unclear when j is updated
```

Instead, prefer writing code in multiple clear steps:
```python
j += 1
a[j - 1] = a[j]
```

### Short-Circuit Evaluation

In Python, logical operators `and` and `or` perform **short-circuit evaluation**. This means they may skip evaluating the right-hand side if the left-hand side is already sufficient to determine the result. Short-circuiting helps improve performance and avoid unnecessary side effects or runtime errors (e.g., accessing attributes of `None`).

Specifically:
- For `and`, if the left-hand side evaluates to `False`, the entire expression is `False`, so the right-hand side is **not evaluated**.
- For `or`, if the left-hand side evaluates to `True`, the entire expression is `True`, so the right-hand side is **not evaluated**.


In contrast, bitwise operators `&` (bitwise AND) and `|` (bitwise OR) **do not short-circuit**:
- For `&`, **both operands are always evaluated**, even if the first is false.
- For `|`, **both operands are always evaluated**, even if the first is true.


You might use `&` and `|` when working with integers at the bit level, or in rare cases where you explicitly want both sides to be evaluated (e.g., for their side effects).


Python's short-circuit behavior is predictable and safe. For example, the idiom: `if x is not None and x.do_something():` is safe — if `x` is `None`, the second part (`x.do_something()`) is never evaluated, which prevents an AttributeError. This pattern is common when traversing data structures like linked lists or trees, where you must check if an object exists before calling methods on it.

```java
def is_true():
    print("is_true called")
    return True

def is_false():
    print("is_false called")
    return False

if is_true() or is_false():
    print("Result: True")  # only is_true() will be called

if is_true() | is_false():
    print("Result: True")  # both will be called
```

Short-circuit evaluation is exactly why some earlier code block pairs were equivalent and others were not.

```python
# Code_Block_1
if condition_1:
    if condition_2:
        # Statements to be executed
```

```python
# Code_Block_2
if condition_1 and condition_2:
    # Statements to be executed
```

For example, `Code_Block_1` and `Code_Block_2` are equivalent, that is the statements inside will only be executed if both conditions evaluate to `True`. And due to the reason we put `condition_1` before `condition_2` and connect them with `and` in `Code_Block_2`, if `condition_1` evaluates to `False`, then `condition_2` won't be evaluated. This matters if `condition_2` has side effects, such as modifying the values of some variables: those side effects will never occur unless `condition_1` evaluates to `True`.

### Applying Mathematical Logic

A bit of knowledge about mathematical logic can help you simplify boolean expressions and make the code more readable.

#### De Morgan’s Laws

According to De Morgan's Laws:

$$
\neg (A \lor B) \equiv (\neg A) \land (\neg B)
$$

$$
\neg (A \land B) \equiv (\neg A) \lor (\neg B)
$$

Thus in programming languages (such as in Python), we have the following:
- `not (A and B and C)` is equivalent to `not A or not B or not C`, and vice versa
- `not (A or B or C)` is equivalent to `not A and not B and not C`, and vice versa

#### Distributive Laws

According to De Morgan's Laws:

$$
A \land (B \lor C) \equiv (A \land B) \lor (A \land C)
$$

$$
A \lor (B \land C) \equiv (A \lor B) \land (A \lor C)
$$


Thus in programming languages, we have the following:
- `A and (B or C)` is equivalent to `(A and B) or (A and C)`, and vice versa
- `A or (B and C)` is equivalent to `(A or B) and (A or C)`, and vice versa

#### Absorption Laws

According to Absorption Laws:

$$
A \lor (A \land B) \equiv A
$$

$$
A \land (A \lor B) \equiv A
$$


Thus in programming languages, we have the following:
- `A or (A and B)` is equivalent to `A`, and vice versa
- `A and (A or B)` is equivalent to `A`, and vice versa

#### Exclusive OR (XOR)

According to the definition of Exclusive OR (XOR):

$$
(A \land \neg B) \lor (\neg A \land B) \equiv A \oplus B
$$


Thus in programming languages, we have the following:
- `(A and not B) or (not A and B)` is equivalent to `A ^ B`, and vice versa

## Writing Good Branching Code

When implementing branching, there are some good practices and common pitfalls to keep in mind. These optional tips can improve performance, clarity, and maintainability. We’ll revisit some of these tricks later as well.

### Invert Complicated Conditions

You must have noticed that for the `else` branch, you don't have to explicitly specify the branching condition: it automatically handles "everything else". This follows the complement idea: all the conditions covered by `if` and `elif` clauses form one set of possibilities (of variable assignments), and the `else` covers the complement of that set.

This is particularly useful when we only have a pair of `if-else`, meaning we’re covering exactly two possibilities (a binary decision). That’s essentially a binary partition: **the predicate and its complement**. If the predicate is very complex or cumbersome to write, sometimes it’s easier to write the negation of that predicate for one of the branches, rather than the predicate itself. In binary search, you can see a lot of applications of this technique.

For example, suppose we have six colors: `RED`, `BLUE`, `YELLOW`, `GREEN`, `WHITE`, `BLACK`. And we just want to know if an object is black or not. We could write it two ways:

```python
def is_black_v1(color):
    if color in ['RED', 'BLUE', 'YELLOW', 'GREEN', 'WHITE']:
        return False
    else:
        return True
```

```python
def is_black_v2(color):
    if color == 'BLACK':
        return True
    else:
        return False
```

In general, if the branching structure has only two branches, then there are two predicates/boolean expressions. In such cases, we can use the simpler predicate in the `if` branching condition, leaving the `else` implicitly handle the other (complementary) case. This can lead to clearer and less error-prone code.

### Treat Overlapping Conditions Carefully

Ideally the branching conditions should be **mutually exclusive** and cover all possibilities, because then the order of the branches doesn't matter and the logic is less error-prone. But sometimes it's difficult or expensive to achieve this goal. You need to weigh the clarity vs. complexity trade-off.

Consider the previous Venn diagram example, `condition_1`, `condition_2` and `condition_3` overlap, and if we want to make all branches mutually exclusive, we need to break them into many sub-conditions, like 7 mutually exclusive conditions corresponding to A1~A7. That could become unwieldy.

In addition, sometimes we can utilize the overlap between the conditions to improve performance.

<img src="img/grading_number_line.png" alt="venn_diagram_for_branching_statement" style="width: 50%; height: 10%; display: block; margin-left: auto; margin-right: auto;">

Consider the above example, we want to write a program that determines the grade (`A`, `B`, `C`, `D`, `F`) of a student using the score as input. If we insist on making the branching conditions mutually exclusive, we would have something similar to the following:

```python
def get_grade(score):
    if score >= 90:
        return 'A'
    elif score < 90 and score >= 80:
        return 'B'
    elif score < 80 and score >= 70:
        return 'C'
    elif score < 70 and score >= 60:
        return 'D'
    else:
        return 'F'
```

You may think the code is clear and more readable, but the trade-off is redundancy, which is why it's not always encouraged. For example, whenever the program evaluates `score < 90 and score >= 80`, `score < 90` must be true, because the branching condition `score >= 90` has already been evaluated to false. Utilizing the intersection of the conditions we could refactor it into something like the following, which is considered more efficient:

```python
def get_grade(score):
    if score >= 90:
        return 'A'
    if score >= 80:
        return 'B'
    if score >= 70:
        return 'C'
    if score >= 60:
        return 'D'
    return 'F'
```

Or equivalently:
```python
def get_grade(score):
    if score < 60:
        return 'F'
    if score < 70:
        return 'D'
    if score < 80:
        return 'C'
    if score < 90:
        return 'B'
    return 'A'
```

If the branching conditions are not mutually exclusive, the order of the branches makes a difference, and remember to make the more specific condition come **before** the more general one.

Consider the following example:
```python
if (x is divisible by 2):
    # Statement_A to be executed
elif (x is divisible by 4):
    # Statement_B to be executed
```
The condition `x is divisible by 4` is more specific than `x is divisible by 2`. From the perspective of set theory, the first condition is a super set of the second condition, so the second branch will never get a chance to execute.

You could refactor it to the following:
```python
if (x is divisible by 4):
    # Statement_B to be executed
elif (x is divisible by 2):
    # Statement_A to be executed
```

Sometimes this will make more sense, but remember when reading branching code, read all the conditions from top to bottom and understand that by the time you get to a certain branch, earlier conditions have failed.

In this case you should realize that although the condition says `x is divisible by 2`, but only when `x is divisible by 2 not not divisible by 4` will go into this branch.

### Reordering Branching Code

#### Basic Ideas

If you know that certain conditions are more likely than others, consider ordering your branches (and even the parts of complex boolean expressions) to check the likely cases first. This can provide a performance benefit by short-circuiting checks that are unlikely to succeed.

**For branches (if/elif)**: Since the evaluation order of the branching conditions is from top to bottom, we can move the most likely branches (the ones with the highest probability) closer to the top to improve performance. Notice that when doing so, we need to consider the overlap between branching conditions to avoid bugs.

**For boolean expressions**: Since the evaluation order in Python is left to right, you can place the most likely expressions first — for example, those most likely to evaluate to `False` if using `and`, or `True` if using `or` — to benefit from short-circuiting. Notice that when doing so, we need to consider the side effects of the expressions to avoid bugs.

If you have no idea about the likelihood or they’re roughly equal, you can instead order by simplicity or cost—check simpler or cheaper conditions first. For example, if two conditions have similar probability, but one involves a quick variable check and the other calls an expensive function, do the quick check first.

#### Moving Likely Branches Closer to the Top

```python
# Before (Less efficient)
if user.is_admin():                 # rarely true
    admin_dashboard()
elif user.is_guest():               # somewhat frequent
    guest_dashboard()
elif user.is_regular_user():         # most frequent
    user_dashboard()
```

```python
# After (More efficient)
if user.is_regular_user():           # most frequent, moved first
    user_dashboard()
elif user.is_guest():               # second most frequent
    guest_dashboard()
elif user.is_admin():               # rarely true, moved last
    admin_dashboard()
```

In the before version, every regular user login will go through two additional branch conditions that rarely evaluate to true. If the evaluation of those two expression is expensive, then the after version improves performance drastically.

#### Moving Likely Expressions Closer to the Left

The following example is for `and`, but for `or` the idea is similar.

```python
# Before (Less efficient)
if user.has_premium_account() and user.is_logged_in():
    access_premium_content()
```

```python
# After (More efficient)
if user.is_logged_in() or user.has_premium_account():
    access_premium_content()
```

In this case, most users aren't logged in, so `user.is_logged_in()` is more likely false than premium account status. In the after version, if the user isn't logged in, the premium account check isn't performed unnecessarily.

#### Putting Simplest or Cheapest Checks First

```python
if database.has_record(user.id()) and user.id() != None and is_valid(user.id()):
    process_record(user.id())
```

```python
if user.id() != None and is_valid(user.id()) and database.has_record(user.id()):
    process_record(user.id())
```
Check for `None` or invalid values before making expensive database calls.

### Use Guard Clauses to Flatten Nested Branches

A **guard clause** is a check at the top of a function or loop that returns early if a condition fails. Using guard clauses can reduce nesting by handling "bad" or edge conditions immediately, so the rest of the code can assume the preconditions are satisfied. 

Deeply nested branching (especially many levels of `if` inside `if`) can make code hard to read. Guard clauses allow you to handle error cases or preconditions upfront and then proceed with the main logic in a flatter structure, thus avoid deeply nested branches.


Use guard clauses when:
- There are multiple validations that must all pass before executing the main logic.
- Handling error cases or edge cases that should immediately stop processing.
- The main action should clearly stand out at the bottom.

Consider the below example. An order will only be processed if a series of logical checks are passed, and if any of them fails, we log the reason.
This program is hard to read and mainly due to deeply nested branches and in reality it could go even worse.

```python
def process_order(order, user):
    if order is not None:
        if order.is_valid():
            if user is not None:
                if user.is_logged_in():
                    if not user.is_banned():
                        order.process()  # Actual processing happens here
                    else:
                        print("User is banned.")
                else:
                    print("User is not logged in.")
            else:
                print("User object is null.")
        else:
            print("Order is invalid.")
    else:
        print("Order is null.")
```

Fortunately, we can use guard clauses to flatten the branches.
```python
def process_order(order, user):
    if order is None:
        print("Order is null.")
        return

    if not order.is_valid():
        print("Order is invalid.")
        return

    if user is None:
        print("User object is null.")
        return

    if not user.is_logged_in():
        print("User is not logged in.")
        return

    if user.is_banned():
        print("User is banned.")
        return

    order.process()
```

## Recommended Leetcode Problems

Now it's time to test your understanding and mastery of writing branching code. The following leetcode problems are the ones that involve relatively complicated branching logic.

The following problems don't involve using data structures or algorithms:
- [13. Roman to Integer](https://leetcode.com/problems/roman-to-integer/)
- [844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/)
- [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
- [71. Simplify Path](https://leetcode.com/problems/simplify-path/)
- [334. Increasing Triplet Subsequence](https://leetcode.com/problems/increasing-triplet-subsequence/)
- [665. Non-decreasing Array](https://leetcode.com/problems/non-decreasing-array/)
- [1071. Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/)
- [65. Valid Number](https://leetcode.com/problems/valid-number/)

The following problems somehow involves data structures such as stack, binary tree:
- [1763. Longest Nice Substring](https://leetcode.com/problems/longest-nice-substring/)
- [99. Recover Binary Search Tree](https://leetcode.com/problems/recover-binary-search-tree/)
- [117. Populating Next Right Pointers in Each Node II](https://leetcode.com/problems/populating-next-right-pointers-in-each-node-ii/)
- [227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/)
- [236. Lowest Common Ancestor of a Binary Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [549. Binary Tree Longest Consecutive Sequence II](https://leetcode.com/problems/binary-tree-longest-consecutive-sequence-ii/)
- [678. Valid Parenthesis String](https://leetcode.com/problems/valid-parenthesis-string/)
- [735. Asteroid Collision](https://leetcode.com/problems/asteroid-collision/)
- [930. Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/)
- [951. Flip Equivalent Binary Trees](https://leetcode.com/problems/flip-equivalent-binary-trees/)
- [971. Flip Binary Tree To Match Preorder Traversal](https://leetcode.com/problems/flip-binary-tree-to-match-preorder-traversal/)
- [1676. Lowest Common Ancestor of a Binary Tree IV](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree-iv/)
- [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)

# Loop/Iteration

## The Idea of Repetition

Repetition has great power, as it allows a finite set of instructions (our code) to handle infinite number of problems. When dealing with collections of arbitrary size or problems of arbitrary scale, repetition is necessary. In programming, there are two types of repetition in programming: loops (iteration) and recursion. In fact, loops and recursion are interchangeable in the sense that any loop can be rewritten recursively and vice versa.

If we need to do something for more than once or an unknown number of times to achieve some goal, we should consider loop or recursion. When designing an algorithm, we need to identify steps that might need to repeat many times in the process/algorithm, and those steps will likely be expressed as a loop or recursion.

In this section, we’ll focus on loops (iteration) and discuss systematic ways to implement and reason about them.

## Loop Statements

Loops allow a program to repeat a block of code multiple times until a condition is met. Python supports two main types of loops: `while` loops and `for` loops.

You should know about the following types of loops and their structures:
- `while` loop is a type of **condition-controlled** loop and is often used when you don’t know in advance how many iterations you’ll need.
- `for` loop is a type of **counting** loop and is often used when you know how many iterations are required, such as iterating over arrays, lists and strings of known length. In Python, a `for` loop inherently behaves like a "for-each" loop in other languages.

Python does not have a `do-while` loop. In other languages, it's a loop that runs at least once before checking the condition.


You should also be familiar with the following loop control statements:
- `break` immediately terminates the loop, and it's used for early exit when a desired condition is met.
- `continue` skips the rest of the current iteration and moves on to the next one.
- `return` inside loop exits not just the loop but the entire function, which is common in search problems where a result can be returned immediately.

The most basic loop structure is the `while` loop structure, and all other loop mechanisms could be converted to a `while` loop. For example, `for` loop can be easily converted to `while` loop, but it's often easier to write and read than the `while` loop counterpart. In fact, in competitive programming, you’ll likely use `for` loops a lot (for counting or iterating collections) and `while` loops for more open-ended repetition.

```python
# while loop structure
while condition:
    # Body: do something while condition holds
```

```python
# for loop structure (with index)
for i in range(start, end):
    # do something with current index i
```

```python
# for-each loop structure (Python)
for element in collection:
    # process current element
```

```python
# do-while equivalent in Python
while True:
    # do something at least once
    if not condition:
        break
```

```python
# infinite while loop with manual exit
while True:
    do_something()
    if should_exit():
        break
    do_something_else()
```

## Methodical Loop Analysis

### Elements of a Loop

When analyzing or designing a loop, especially for algorithmic problems, it helps to break it down into fundamental elements. These elements are interrelated, and considering each of them will guide you to a correct and efficient loop design. The core elements include:
- **Loop Goal**: the high-level objective of the loop, what it aims to accomplish.
- **Loop Premise**: for this loop to operate correctly, what assumptions or conditions must be true.
- **Loop Initialization**: setup code that prepares any necessary state before the loop begins. This may include index variables (e.g., for `while` loops) or other tracking variables.
- **Loop Condition**: the conditions that determines whether the loop should continue or terminate.
- **Loop Termination**: what we know after the loop terminates and whether the variables have correct and useful values.
- **Iteration Goal**: the purpose or effect of each individual iteration relative to the overall goal.
- **Loop Body**: the block of code executed in each iteration of the loop.
- **Edge Cases**: initialization, termination and corner/special cases.
- **Loop Variant**: a value (such as a counter or remaining distance) that changes consistently each iteration, ensuring the loop eventually stops.
- **Loop Invariant**: useful properties of variables used inside the loop that are true before and after each iteration and can help reason about the correctness of the loop.

Relationships Between Elements:
1. **Loop Premise** holds initially -> **Loop Initialization** sets variables -> **Loop Invariant** established before first iteration.
2. **Loop Condition** is checked -> If true: **Loop Body** transforms state from iteration $i-1$ to $i$, achieving **Iteration Goal**.
3. **Loop Variant** progresses via updates -> **Loop Invariant** maintained.
4. **Loop Termination** when condition fails or early exit occurs.
5. **Loop Goal** achieved after termination, if we have maintained **Loop Invariant**.
6. **Edge cases** ensure correctness at boundaries (first, last, zero iterations, etc.).

In addition, we can also mention the definition of variables used in the loop, such as loop control variables and other state variables, which is optional but provides more information in the documentation. In Python, loops often iterate directly over items in a collection (like a list), so explicit control variables may not always be needed. When using `while` loops or `range`, such variables can still be useful.

To write a correct loop, all these elements should be considered. In the following sections, we will delve into each of them in detail. We will focus on single-threaded, algorithmic loops (typical for competitive programming or algorithm exercises).

**Loop Invariant** is arguably the most valuable and important thing about a loop that helps programmer implement and reason about the loop. In a coding interview, if you cannot explain the loop invariant of a particular loop, the interviewer may think you don't understand your code and cannot prove its correctness.

Asking yourself the following questions and perhaps documenting them in the code comment would be helpful:
- What's the loop invariant? Why it's important to maintain it?
- What's the loop variant? Why does it guarantee the termination?
- What's the meaning of each variable/loop?
- What's the goal/objective of each iteration?
- What happens after each iteration?
- What do we know after the loop terminates?

### Loop Goal

**Definition**: The loop goal is the high-level objective that the loop seeks to accomplish by the time it finishes.

**Relation**: It’s equivalent to the postconditions of a function, as we want the goal to be true after the loop terminates. 

**Importance**: It establishes correctness criteria for the loop.

**Notes**:
- A lot of things could happen within a loop, so there could be other side effects besides the loop goal after termination, especially if you are not careful with you code. Remember to check your code to see if there are unwanted side effects aside from the loop goal.

**Concept Examples**:
- Find the sum of all elements.
- Determine if an array contains any duplicate values.
- Find the target value in a sorted array.

**Code Example**:
```python
# Loop goal: Find the sum of all elements in the list nums.
sum = 0
for num in nums:
    sum += num
# After the loop terminates, sum is the total of elements.
```

### Loop Premise

**Definition**: The loop premise is any assumption or precondition required for the loop to operate correctly.

**Relation**: It's equivalent to the preconditions of a function.

**Importance**: Prevents runtime errors and logical mistakes.

**Notes**:
- To make loop premises true, we often need to do some work before the loop. For example, if we want to use binary search on an array, a premise is that the array must be sorted first. Such preparation (sorting, in this case) is done before the loop and not part of the loop itself.
- Loop premises are established outside the loop. Do not confuse it with loop initialization.
- Loop premises are often checked explicitly using guard clauses.
- Sometimes loop premise are obvious and not explicitly mentioned. For example, iterating through a collection that is guaranteed to be non-null.

**Concept Examples**:
- The array is non-null. This premise is common all loop that read or write an array.
- The array is sorted in ascending order. This premise is common for many algorithms, for example, using binary search on an unsorted array doesn't make sense.

**Code Example**:
```python
# Loop premise: nums is not None and not empty
if nums is None or len(nums) == 0:
    return -1  # guard clause to enforce premise

max_val = float('-inf')  # Loop initialization (unrelated to premise)

for i in range(len(nums)):
    if nums[i] > max_val:
        max_val = nums[i]
```

### Loop Initialization

**Definition**: This is the setup code that initializes any variables needed before the loop, such as counters, pointers, or state trackers. In Python, this may include index variables (e.g. for while loops) or window boundaries.

**Relation**: Takes place before the main loop body. How the variables should be initialized should be determined by the loop variants. It also ensures loop invariants hold before first iteration. 

**Importance**: Guarantees loop starts correctly, establishing loop invariant initially.

**Notes**:
- Logically, loop initialization is part of the loop’s overall process, but it's physically outside the main loop structure. In fact, a loop will run indefinitely if it only access variables declared inside the loop. If you forget to initialize properly, your loop might run indefinitely or produce incorrect results.
- Loop Initialization is not about satisfying the loop premise. It’s about preparing variables for use inside the loop. 
- It could be simple or complicated. For example, in many sliding window algorithms, we need to initialize the window variable before going into the main loop, and that initialization itself involves a mini-loop.
- Initialization is important because not everything we need to do should be included inside the loop. Only those that are exactly same in nature requires repetition. Instead, if something only needs to happen once, do it outside, either before or after the loop.
- Sometimes the first case(s) work differently from the rest, and it's a good practice to handle them in the initialization phase rather than complicating the loop logic to account for them. Proper initialization often handles any first case(s) or set up so the loop body can focus on the repetitive step.

**Concept Examples**:
- Loop counters, e.g., `i = 0`
- Two pointers, e.g., `left = 0, right = len(nums) - 1`
- Collection initialization, e.g., `list = []`

**Code Examples**:
```python
# Initialization for two-pointer approach to reverse array.
left, right = 0, len(nums) - 1
while left < right:
    temp = nums[left]
    nums[left] = nums[right]
    nums[right] = temp
    left += 1
    right -= 1
```

```python
# Initialization for a collection that stores all positive numbers in an array.
new_list = []
for num in nums:
    if num > 0:
        new_list.append(num)
```

```python
max_val = float('-inf')  # This is part of loop initialization
for i in range(len(nums)):
    if nums[i] > max_val:
        max_val = nums[i]
```

### Loop Conditions

**Definition**: The boolean expressions determining whether to continue or terminate the loop.

**Relation**: Provides inferred knowledge after the loop terminates as we know continuation condition doesn't hold or one of the exit condition is true. It's also closely connected to the loop variants because it involves the loop control variable which moves toward termination.

**Importance**: Controls execution flow, ensures termination, prevents infinite loops and contributes to loop goal.

**Notes**:
- No loop should run forever, so use loop conditions that can guarantee the termination. 
    - Notice that there are practical long running programs that are supposed to have no exit condition and run forever until the program is shut down from outside. But those are outside our scope, as our discussion is confined to algorithmic programs.
- Loop conditions include when the loop should continue as well as when the loop should terminate. To avoid confusion:
    - **Continuation Condition**: This refers to the condition that appears in the loop header, such as in a while loop or as the iterable in a for loop.
        ```python
        while condition:
            statements
    
        # or a for loop over an iterable like:
        for item in collection:
            ...
        ```
    - **Exit Condition**: This refers to any condition inside the loop body that causes an early exit. For example:
        ```python
        if condition:
            break
        ```
    - In our discussion, the term "loop condition" specifically refers to the continuation condition defined in the loop header (e.g., `while condition`). We’ll explicitly mention "exit condition" when referring to early termination logic like `break` or `return` inside the loop.
    - Thus, conditions that affect loop control can appear either at the beginning of each iteration (in the loop header) or within the loop body as part of an internal check.
- Always ensure loop conditions are clear and capable of eventually terminating the loop. Due to the `if-else` structure, sometimes the exit condition is not explicitly defined. Make the conditions explicit or implicit according to your needs.
- Loop conditions should be determined by loop variants. For example, when using a loop index i, the loop range is directly tied to the loop condition. In Python, if we want i to vary from $0$ to $n - 1$, we typically write:
    ```python
    for i in range(n):
        ...
    ```
This implicitly ensures that `i < n` throughout the loop. Besides loop indices, other loop variants may include shrinking ranges, decreasing counters, or consuming elements from a collection. In all cases, the loop condition should reflect that the variant is progressing toward its target (e.g., $i$ eventually reaching $n$).

**Concept Examples**:
- Index bound checks $i$ < $n$. For example, in `for` loops, the loop condition could be that the value of the loop index must be within a certain range.
- Pointer crossing checks `left <= right`. For example, in binary search, which is often implemented as a `while` loop, the loop condition is that the subarray delimited by $[left...right]$ is not empty.
- Remaining elements in the collection: In Python, this is typically written as `while collection:` which continues the loop as long as the collection is not empty.

**Code Example**:

```python
left = 0
right = len(nums) - 1

# Continuation condition for binary search: continue while the search space is not empty
while left <= right:
    mid = left + (right - left) // 2

    if nums[mid] < target:
        left = mid + 1
    elif nums[mid] > target:
        right = mid - 1
    else:
        return mid  # Exit condition: target found

# After the loop terminates, we know that left > right (i.e., target not found).
# The other exit condition (nums[mid] == target) would have returned earlier.
```

### Loop Termination

**Definition**: Describes the point at which and reason why the loop stops running, and what is true once it has stopped.

**Relation**: Closely connected with loop initialization, loop variants and loop conditions. Given all that information, we can deduce properties of variables after the loop ends.

**Importance**: Critical for reasoning about final outcomes. 

**Notes**:
- The termination describes precisely when and how the loop stops, and what conditions hold true after termination.
- If there are $n$ conditions for the loop to terminate, and there are $2^n$ possibilities of their combinations after the termination. In addition, we need to combine the knowledge of how the loop is initialized and what are the loop variants to reach useful conclusion. It's common that after the loop terminates, we use branching to handle different combinations of outcomes.
- For a correct loop implementation, loop invariant and loop goal should be true after termination. We need to be very clear about what properties about what variables we have after the loop ends.

**Concept Examples**:
- If the only loop condition is `i < n`, with $i$ initially being $0$ and incrementing by 1 for each iteration, and $n$ is constant and initially positive, we know that after the loop terminates, $i = n$.
- If the only loop condition is `while collection:`, we know that after the loop terminates, the collection is empty.

**Code Example**:
```python
# Search for a value
found = False  # Initialization

for num in nums:
    if num == target:  # Exit condition
        found = True
        break  # Early exit when target is found

# After termination: 
# If found is True, then target exists in nums.
# Otherwise, the loop ran to completion and the target was not found.
```

### Iteration Goal

**Definition**: The per-iteration objective is supposed to accomplish as a step toward the loop’s overall goal.

**Relation**: Iteration goal is defined in terms of loop invariants, and achieved through the loop body.

**Importance**: Provides logical clarity per iteration, ensures correctness and progress.

**Notes**:
- The iteration goal describes what each individual iteration aims to accomplish. Typically, by using the loop invariant, we can determine the state at iteration `i - 1`, and use that information to deduce what should be accomplished in iteration `i`.
- Iteration goal typically involves modification on some variables and changing some states, because there is no way to terminate a loop if we only read but not modify variables.
    - Notice that this is only true for single-thread programs. In a multi-threading setting, the variables we access in a loop could be modified by other threads. In this case, the loop relies on an external factor to break it, which is unusual in algorithmic contexts.
- Loop could often be an inductive process, where each iteration takes the state from the previous iteration and advances it one step closer to the final goal.

**Concept Example**:
- During iteration $i$, find the maximum among the elements in the range $[0..i]$.
- After iteration $i$, ensure that the elements in `nums[0..i]` are sorted in non-decreasing order (i.e., the prefix up to index $i$ is in order).

**Code Example**:
```python
# Loop goal: find the max element in nums[0..n-1], where n is len(nums)
# Iteration goal: find the max element in nums[0..i]

current_max = nums[0]

for i in range(1, len(nums)):
    if nums[i] > current_max:
        current_max = nums[i]
    
    # Iteration goal achieved: current_max now holds max(nums[0..i])
```

### Loop Body

**Definition**: The block of code executed each iteration.

**Relation**: It achieves the iteration goal, utilizes and maintains loop invariants, and update loop variants to ensure progress.

**Importance**: Performs the real work, transitions from one iteration to the next, core of the repetition.

**Notes**:
- **Iteration Dependency**: Depending on the concrete code, iterations may be *independent* or *dependent*.
  - *Independent iterations* have no reliance on previous iterations. Example: printing each element in a list.
  - *Dependent iterations* build upon results from earlier iterations. Examples include dynamic programming (DP) and prefix sum computations.
- For dependent iterations, the loop body's role is to take the state from iteration $i-1$ and compute the state for iteration $i$.
- **Idempotent Loops**: Some loops have bodies that can execute multiple times without changing the program state — this is known as *idempotent* behavior. Such loops may be useful in scenarios like polling or retry logic, though they are less common in pure algorithmic design.

**Code Examples**:
```python
# Loop goal: compute Fibonacci sequence up to n
# Dependent iteration — the Fibonacci number at step i depends on the two previous numbers
a = 0
b = 1
for i in range(n):
    next = a + b  # depends on previous two values
    a = b
    b = next
```

```python
# Independent iteration — each iteration does not depend on previous ones
for num in nums:
    print(num)

```

### Edge Cases

**Definition**: Edge cases are the special or extreme scenarios that might need explicit handling outside the normal loop logic, often handled at the boundaries (initialization and termination).

**Importance**: Prevents subtle bugs, especially off-by-one errors.

**Notes**:
- Both initialization and termination (the moments before the first iteration and after the last iteration) are edges cases. Corner cases are also edge cases, but it's highly case-by-case.
- Different algorithms have different edge cases, but typically if the collection we need to deal with have 0 or 1 element, then it's an edge case.
- Typically the first and the last iteration of the loop would introduce some problems. The first iteration is about initialization and the last iteration is about termination.
- Some algorithms define **best case** and **worst case** scenarios, which you can consider as edge cases in terms of performance or structure.
- Edge cases should usually be considered when writing code and often documented in comments or tests. We can find edge cases by making the assumption of our procedure FALSE or by observing out-of-bound cases of the code.

**Concept Examples**:
- Empty array, e.g., `len(nums) == 0`
- One-element array, e.g., `len(nums) == 1`
- Loop condition is false from the start (loop body does not execute at all)
- Loop runs exactly once (often sensitive to off-by-one errors)

**Code Example**:

```python
if not nums:
    return -1  # handle empty list explicitly
```

### Loop Variant

**Definition**: A loop variant is a concrete measure that is often numeric and shows how close a loop is to termination. It's a quantity that:
- Changes in each iteration, often in a sense of decrement.
- Moves consistently toward a condition that stops the loop.
- Guarantees that the loop won't run forever.

**Relation**: Directly related to the loop control variables, how they should be initialized and updated in loop body, and thus closely connected with loop initialization, loop conditions and loop termination.

**Importance**: Guarantees loop termination by ensuring measurable progress. It’s essential for reasoning about a loop’s performance and termination.

---

**Meaning and Role of Loop Variant**
- The loop variant is a measure of how close the loop is to termination.
    - In single-thread algorithmic programs, a loop is not supposed to run forever, and any loop should eventually terminate. 
    - For a loop destined to terminate in a deterministic way, it must have a finite number of iterations, and the loop variant acts like a counter of the number of remaining iterations, going from a non-negative integer to 0.
- Another way to describe loop variant: You can think of the loop variant as a countdown timer toward loop termination.
    - Each iteration ticks the timer closer to zero or some termination boundary.
    - When the variant reaches a termination point, the loop stops.
    - It's also equivalent to define loop variant as some numeric measure that strictly increases toward a limit with each iteration.
- To be more specific, we can put loop variant under the context of programming elements.
    - An explanation is to treat the loop body as a function, all the variables accessed(read or write) in the loop body as parameters of that function, and the output value should decrease for every iteration. 
    - Indeed, the variables accessed in the loop body can be divided into several categories and we will discuss that later. The ones that are directly relevant to loop variant are called **loop control variables**.
- Although we typically assume a loop should always terminate due to some loop condition, there are exceptions especially when we consider multi-threading and event loop programs. 
    - For example, in a producer-consumer pattern multi-threaded program, the consumer could be long-running until the system stops it. 
- If you take a look at the wikipedia page of loop variant, you will see some rather formal academic definitions, so here we try to somehow simplify it but still keep the essence.
- Both loop variant and loop invariant are connected with meaning and definition of variables accessed in the loop. 
    - There are variables accessed in the loop that are not considered as loop control variables and thus have little to do with loop variant, so it's normal for you to feel confused. 
    - We will discuss their distinction and more about loop variant, loop control variables in the section of loop invariant.

---

**Loop Control Variables**
- Loop Control Variables are variables used in loop conditions. Their values could be modified inside the loop body, and they could take many forms. 
- For example, in the Python loop `for i in range(n):`, the variable `i` is a **loop index variable**, which is a common type of loop control variable. It changes automatically with each iteration, and its values determine how the loop progresses.
- Loop control variables directly control whether the loop continues or terminates, and thus they collectively map to an abstract measure of how close the loop is to termination, which is exactly the loop variant.
- **Loop (variable) range** is closely related to loop control variables, which means the valid range of values that loop control variables could take and are allowed to go into the loop body. Typically, when those variables go outside of their valid range, the continuation condition will evaluate to false and the loop terminates.
- Loop control variables also typically appear in the loop invariants. Due to their duel roles in the loop code, the choice of their definitions can impact the complexity of reading and writing the loop.
- We care about how loop control variables are initialized outside loop, how they are updated inside the loop body, and whether they can lead to successful termination. 
    - For example, if we use a loop index as the control variable, we must ensure that:
        - The **start value** should be such that the resulting range is non-empty, so that the loop actually runs.
        - The **update rule** (e.g., increment or decrement) pushes the variable toward the **termination boundary**.
        - The loop makes **measurable progress** in each iteration — meaning the control variable must move toward the condition that will eventually become false.
    - In Python, such reasoning is often implicit in how we use `range()`:
      ```python
      # This loop starts at 0, increases by 1, and stops when i == n.
      for i in range(0, n):
          ...
      ```
      Here, `i` is initialized to `0`, updated by `+1` on each iteration, and the loop stops when `i >= n`. It guarantees termination in `n` steps.
    - In reverse:
      ```python
      # This loop starts at a and decrements until i <= b.
      for i in range(a, b, -1):
          ...
      ```
      This behavior prevents accidental infinite loops in for-loops using range().
    - However, when using a while loop with a manually updated control variable, it is your responsibility to ensure that the update rule moves the variable toward satisfying the termination condition. Otherwise, the loop may run infinitely.
      ```python
      # Potential infinite loop if i is never decremented
      i = 5
      while i < 10:
          print(i)
          # i is not updated → infinite loop
      ```
- We sometimes use **loop variables** to refer to loop control variables for brevity in later sections.

---

**Loop Update**
- Loop Update means concrete code that modifies loop control variables.
- It is a concrete progress mechanism that changes the loop control variables, while loop variant is an abstract measure of progress.
- For example `i += 1`, `left += 1`, `right -= 1` are loop update (the actual code modifying loop variables); while the size of search space, number of unprocessed nodes, etc. are loop variants.
- The correctness and successful termination of a loop depends on loop update. For each iteration, at least one of the loop control variables should be assigned a new value. Otherwise the loop will run indefinitely.
- In addition, the way we update the loop variables have significance to how the loop works. 
    - **Iteration Order**: 
        - **Order-sensitive Loops**: Some loops depend on iteration order (stateful accumulations), e.g., DP, prefix sums.
        - **Order-agnostic Loops**: Some loops don’t depend on iteration order (map-reduce style parallel loops), e.g., summing independent values.
    - **Iteration Direction**: 
        - Typically when traversing a list/array, iterating from left to right and right to left are equivalent, although we may need some conversion on how the loop variables are initialized and updated. 
        - Sometimes one direction would be easier than the other, and for some algorithms only one of the directions work.

---

**Loop Variants vs. Loop Control Variables**
- Loop variants are represented by loop control variables, but they are not identical. 
- A loop variant is typically derived from loop variables. It is an abstract, logical measure that maps clearly to the variables.
- Loop control variables are concrete variables modified each iteration and appear in loop conditions.
- For example in binary search 
    - We have the $left$ and $right$ pointers of an array, which are loop control variables. 
    - But the loop variant is the size of the subarray delimited by $[left:right+1]$. 
    - A correct implementation of a loop ensures that loop control variables are updated after each iteration, moving the loop toward termination. 
    - Here the size of the subarray $[left:right+1]$ must decrease for each iteration, and when it reaches 0, the loop terminates.

---

**Loop Variant and Time Complexity**
- Each loop could have multiple loop variants and the net effect is to take the minimum of them. 
    - For this reason, loop variants are closely tied to the time complexity of the loop.
    - For example, a loop could have loop variant 1, which makes the loop terminates in 20 iterations, but the loop could also have another loop variant 2, which makes the loop terminates in 10 iterations. 
    - The net effect of the two loop variants, by taking the minimum, is that the loop will terminate in 10 iterations.
- The time complexity of running a loop is directly tied to loop variant: 
    - The loop variant's rate of change impacts time complexity: how the loop control variables are initialized, how they are updated and when the loop terminates. So loop variant helps analyze loop performance from design.
    - Variant decreases by $1$ -> $O(n)$
    - Variant halves each time -> $O(\log(n))$

---

**Identify Loop Control Variables and Loop Variant**:
1. Identify the loop condition. 
    - For example, `while left <= right: ...`.
2. Extract variables in that condition change during the loop.
    - Here, $left$ and $right$ (both potentially modified). These are your loop control variables.
3. Define a variant from those variables:
    - Usually a single numeric measure directly derived from these variables.
    - Must consistently approach a termination boundary.
    - Example for binary search: Variant = $right - left + 1$ (size of search space), which decreases every iteration for a correct implementation.

However, this only tells you how to identify them, not how to choose them for your loop.

---

**Concept Examples**:
- A loop index `i` can either increment (`i += 1`) or decrement (`i -= 1`), which causes the number of unexamined elements in the array to shrink. For example, If iterating forward, the remaining elements are `nums[i:n]`. If iterating backward, the remaining elements are `nums[0:i+1]`.
- Size reduction of search space ($right - left$ decreases for each iteration)

**Code Example**:
- See the examples in the section of loop invariant, where we will discuss the loop variants and invariants of typical algorithms together.

### Loop Invariant

#### Definition

A **loop invariant**:
- Is a non-trivial logical condition/statement/proposition about variables accessed in the loop
- Is true before the loop starts
- Remains true before and after every iteration
- Helps reason about correctness of the loop, as it's true after loop terminates
- Ultimately supports the achievement of the loop goal

Loop invariants are connected with most loop elements and are the key to understanding why the loop works, especially for more complex loops. It is like a promise that you make before the loop starts, and you must keep this promise through every iteration until the loop ends.

---

```python
max_val = float('-inf')  # Loop invariant: max_val is the maximum of nums
n = len(nums)
for i in range(n):
    if nums[i] > max_val:
        max_val = nums[i]
```

In the above code example, the loop invariant can be stated as:
> At the start of iteration `i`, `max_val` is the maximum element in `nums[0:i]` — that is, all elements before index `i`.

- At the start of iteration `i = 0` — which is before the loop begins — `max_val` is initialized to `float('-inf')`. This is a reasonable initial value, as the maximum of an empty set is often treated as negative infinity (`-∞`). If the list `nums` is empty, the loop body never executes, and `max_val` remains `float('-inf')`, which conforms to the invariant. Note that at this point, `nums[0:0]` is an empty sublist.
- During the loop, the `if` condition maintains the loop invariant: If `max_val` correctly holds the maximum of `nums[0:i]` at the start of iteration `i`, then after comparing and possibly updating it with `nums[i]`, it will hold the maximum of `nums[0:i+1]` at the end of that iteration. After `i` increases, the invariant remains valid for the next iteration.
- After the loop ends, we have `i == n`, which corresponds to the start of a (non-existent) final iteration. By substituting `i = n` into the invariant, we conclude that `max_val` is the maximum of `nums[0:n]`, which is the entire list. Therefore, the invariant confirms that `max_val` contains the correct maximum value after the loop terminates.

---

<img src="img/loop_invariant_on_a_line.png" alt="loop_invariant_on_a_line" style="display: block; margin-left: auto; margin-right: auto;">

**Notes**:
- Graphically, if you imagine the loop's execution timeline, the invariant holds at those boundary points between iterations.
- We typically state the loop invariant as something that is true at the start of every iteration.
- When we talk about the "start of an iteration",
    - In both `while` and `for` loops, the start of an iteration means the moment just before the evaluation of the loop condition, but after the update of the previous iteration. 
    - As for the end of an iteration, in `for` loops, we consider it the moment after the update part in loop header executes, in the above example it's `i += 1`; in `while` loops, we consider it the moment after the last statement inside the loop body.
    - So either way, the end of an iteration is identical to the start of the next iteration, and you can see this from the above image, which corresponds to the red part.
- Loop invariant in the first and the last iteration:
    - Before the first iteration: the moment at which we check the loop invariant just prior to the first iteration is immediately after the whole loop initialization, which includes the initial assignment to the loop variables, and just before the first loop condition evaluation in the loop header.
    - After the last iteration: the loop invariant is true before the loop condition evaluation, but since the loop terminates at this point, just after the termination, the loop invariant is still true. But those variables defined in the loop won't be accessible anymore. Notice that if the code doesn't go into the loop body, we don't consider it as an iteration.
- Loop invariants are highly case-by-case, but they should be non-trivial, dynamic, and structurally relevant properties that 
    - Depend on loop-executed changes, e.g., updated variables
    - Are necessary to prove the loop's correctness
    - Would not obviously be true unless carefully maintained during each iteration.
- In the above example, the statement "$n$ is the length of $nums$" is always true (before, during, and after loop), never changes, does not depend on the loop, and is not useful for proving correctness of the loop's behavior. So we don't consider it a loop invariant in the algorithmic sense.
- In general, a loop invariant must hold at the start of each iteration (before the loop body executes), and it must be re-established by the end of that iteration (just before the next condition check or loop exit). But it may not hold during execution of the loop body, which corresponds to the blue part in the image, especially in complex or multi-step loops. This is OK as long as it’s restored by the end of the iteration.
- In complex loops, you may have multiple invariants tracking different states. They must all hold simultaneously at the start of each iteration.

---

**Variant vs Invariant**:
- Loop variant = "The progress meter": A numeric measure explicitly moving toward termination every iteration.
- Loop invariant = "The truth that stays true": A logical condition or meaning that holds true every iteration.
- Some people are confused about the difference between loop variant and invariants, but they are not opposite, instead they complement each other, serving distinct but interlocking roles in ensuring loop correctness.
- Two Pillars of Loop Correctness
    - Variant ensures the loop stops. A correct loop that never terminates is useless.
    - Invariant ensures the loop does the right work. A terminating loop that gives wrong results is also useless.
- Applied at Different Phases
    - The loop variant tracks progress between iterations. At the end of each iteration, the variant must have decreased.
    - The loop invariant ensures correctness within each iteration. At the start of each iteration, the invariant holds.
- Loop control variables often serve a dual purpose:
    - As part of the loop variant, they represent how much work is left and they drive termination through loop conditions.
    - As part of the loop invariant, they help describe how much work has been done, acting as parameters.
    - This dual role makes loop control variables the bridge between loop variant and invariant.
- Some people say the loop variant represents the future of the loop and the loop invariant represents the past of the loop.

Variant vs. Invariant: Complementary Roles
| Concept                 | Loop Variant                              | Loop Invariant                           |
| ----------------------- | ----------------------------------------- | -----------------------------------------|
| **Purpose**             | Guarantees termination                    | Guarantees correctness of computation    |
| **Tracks**              | How close the loop is to finishing        | What property is consistently maintained |
| **Changes?**            | Must strictly decrease (well-founded)     | Must not change (logically preserved)    |
| **Failure to Maintain** | Risk of infinite loop                     | Risk of logical error                    |
| **Analogous to**        | A countdown timer                         | A safety contract                        |

---

**Exception**:
- There are special cases where loop invariant is false after loop termination. This is because the invariant could be temporarily violated inside the loop body, and if we use `break` or `return` to early exit the loop before restoring the loop invariants, they could be false after the loop terminates.
    - Natural loop termination, where the invariant can be assumed to hold afterward.
    - Abrupt loop termination (via break/return), where you must verify manually whether the invariant holds at the exit point.
        - `break/return` after re-establishing invariant, like `Example_2`, then the invariant holds after the loop.
        - `break/return` while invariant is broken, like `Example_1`, then you have to reason about what happen to the variables used in invariants.
- You could either redefine the invariant or restructure the loop to avoid code like `Example_1`. But even if this is unavoidable, the loop invariant is still true at the start of the last iteration, and using that information along with how the variables related to loop invariants are modified before the loop exits, you could still properly reason about the correctness of the loop.
- This means even in special cases, loop invariants still play an important role in the reasoning, but it's just that you can't directly use it after the termination and need to do some extra work.

```python
# Example_1
while True:
    do_something_that_violates_loop_invariants()
    if should_exit():
        break
    do_something_that_restores_loop_invariants()
```

```python
# Example_2
while True:
    do_something_that_violates_loop_invariants()
    do_something_that_restores_loop_invariants()
    if should_exit():
        break
    do_something_irrelevant_to_loop_invariants()
```

---

**Conceptual Examples**:
- At the start of iteration `i`, `nums[0:i]` consists of the original elements in that range, but now sorted in non-decreasing order.
- At the start of iteration `i`, all elements in `nums[0:i]` have already been printed to the console.
- At the start of iteration `i`, the sub-array `nums[0:i]` does not contain the `target` value.

#### Importance

**Correctness**: 
- Loop invariants are fundamental for proving that a loop is correct, typically using induction. If you can establish a loop invariant and show it holds for every iteration, and that combined with loop termination it implies the goal, you’ve essentially proven your loop is correct.
- Loop invariants are the cornerstone of formal correctness proofs for loops. On the other hand, deviation from loop invariant, or equivalently, failure to maintain loop invariant, implies incorrect loop implementation.
- The reasoning of a loop is basically mathematical induction to prove when the loop invariant for $i-1$ iteration is true, how the loop body makes it also true for the current $i$ iteration. So that at the beginning of the iteration we have the properties of the previous loop and at the end of the iteration we need to arrive at something useful for the next iteration, which is exactly defined by the loop invariant.
- This is essentially a proof by induction and loop invariant acts as the inductive hypothesis.:
    - Base case $P(0)$: Prove the invariant holds before the first iteration of the loop (after initialization).
    - Inductive step $P(i)$: Assume it holds at the start of the `i-th` iteration. Prove that after the loop body executes, it still holds at the start of the `(i+1)-th` iteration.
    - Termination $P(n)$: When the loop ends, the invariant combined with loop conditions implies the loop goal.

```plain
                                        [Initialization]
                                                ↓
                                      Base Case: P(0) holds
                                                ↓
                                         [Loop Entry]
                                                ↓
                                Inductive Step: If P(i), show P(i+1)
                                                ↓
                                        [Condition Fails]
                                                ↓
                                Termination: P(n) + ¬Condition ⇒ Goal
```

- Think about the following questions when using or identifying loop invariants:
    - Before the loop begins: Is the invariant true?
    - During each iteration: Does the loop body preserve the invariant?
    - After the loop ends: Does the invariant, combined with the loop condition being false, help prove the loop goal?
- Loop invariants are less important for some loops.
    - Not every loop needs a complex invariant analysis, especially if iterations are independent.
    - For example in Python's `for item in collection:` loops, the iterations are independent, and loop invariant doesn't tell us much useful information. Typically loops like this doesn't require complicated analysis.
    - For another example, a simple loop that just prints each element doesn’t have an interesting invariant beyond trivial statements.
    -  But for complex loops, especially those where each iteration builds on the last, identifying a solid invariant is crucial to understanding and verifying correctness.
- If you want to see a classic detailed example of proving the correctness of an algorithm using loop invariant, you can refer to the insertion sort chapter of *Introduction to Algorithms*, which gives a perfect demonstration. 

---

**Design and Implementation**: 
- Defining a clear loop invariant can also guide you in loop design and implementation.
- Writing or stating an invariant before coding can:
    - Clarify what the loop must maintain
    - Help programmer decide what variables are needed
    - Prevent incorrect updates or misplaced statements
- While coding, we need keep the invariants in mind:
    - When implementing the loop body, we can assume the loop invariant is true at the start of the loop, and use this piece of information as the basis to come up with the code.
    - We also must ensure the loop body re-establishes the invariant at the end of the iteration. This requirement shapes what operations we perform and in what order.
- Because the invariants must hold initially before the first iteration, so they give us clear clue about how to set up loop initialization, how to initialize the variables with what proper initial values.
- Overall, the invariant connects the loop’s beginning, each iteration’s logic, and the loop’s end in one unifying statement.
- Loop invariants are about how current iteration connects with all work that has been done in all previous iterations. 
    - Implementing and understanding a loop requires us to be clear what we want to go from the `i-th` iteration to the `(i+1)-th` iteration, and break it down into multiple smaller task. 
    - During the process, we need to generalize the loop goal as $P(n)$ and find out what $P(i)$ means, which is typically closely connected with loop invariants.
- We will see some examples on how to utilize loop invariants in solving programming problems.

---

**Documentation**: 
- Including the loop invariant in comments or documentation can greatly help readers understand the logical structure and guarantees of the loop.
- Code is read more often than written. And not just for other people, but also for the author of the code. Because no one could remember everything about the code they write. Proper documentation makes maintaining and understanding code much easier. 
- And when writing documentation or comment for loop, loop invariant is arguably the most important thing to mention when the logic isn’t obvious.
- Stating the invariant explicitly as a comment or docstring:
    - Helps readers understand the loop's structure
    - Makes code easier to maintain and review
    - Especially helpful for complex logic or nested loops
- When reading code of a loop, focus on the loop invariants:
    - If the loop invariants are clearly documented, remember to inspect how it's maintained across iterations
    - If it's not written, hypothesize the invariant and use it to explain each line in the loop
- Over time, as you gain experience, thinking in terms of invariants becomes second nature, because it's like writing pre/post conditions, but inside the loop.

---

**Debugging**: 
- Loop invariants can also be a powerful debugging tool. They could help check internal assumptions during development.
- When a loop doesn't produce the expected result:
    - Think about what should be true at the start of each iteration
    - Insert assertions or logging to validate the invariant during runtime
    - Quickly isolate the point where state diverges from expectation
- You can add runtime assert statements or print checks to catch invariant violations. This can help catch the moment when the loop’s logic breaks.
- We will see examples of property-based testing(PBT) utilizing loop invariants later.

```python
# Code could be simplified if you raise error inside the loop invariant check
while check_loop_invariant() and condition:
    loop_body()
```
OR
```python
if not check_loop_invariant():
    raise RuntimeError("Invariant broken before loop start")

while True:
    if not check_loop_invariant():
        raise RuntimeError("Invariant broken at iteration start")
    if condition:
        break
    loop_body()

if not check_loop_invariant():
    raise RuntimeError("Invariant broken after loop end")
```

---

#### Loop Variable Semantics

A key idea that cannot be over-emphasized: you should always be clear about the meaning/definitions of the variables that you declare and use in the program, as each of them serves a purpose and could be subject to some constraints. A clear and accurate understanding of every variables used in the program is crucial, while misunderstanding a variable’s role is a common source of bugs.

We discussed about variables accessed in the loop briefly in the section of loop variant, and now is a good time to scrutinize them. We don't need to concern ourselves with the variables not accessed in the loop, because they have no impact on how the loop operates.

For the variables that are accessed in the loop, there are two categories.

**Contextual Variables**: variables that are read-only inside the loop.
- They remain constant during the loop, but are still semantically essential.
- They define boundaries and contexts for iteration and they're referenced constantly but aren't updated. For the loop, they often act as parameters or inputs.
- They may not participate in control flow progression, and even though they’re not "control" or "state" variables, they form the logical framework in which loop computation takes place.
- They may appear in the loop condition (e.g., $i < n$), but since they do not change over time, they do not drive the termination. The actual progress is made by Loop Control Variables.
- They may appear in the loop invariants (e.g., $nums$), but they don't carry stateful information, only provides the context of State-Carrying Variables.

**Stateful Variables**: variables that could change during the loop. They represent the evolving state and can be further split into three distinct categories:

1. **Loop Control Variables**: stateful variables that are read in the loop conditions, including the exit conditions. 
- They directly affect the loop's control flow by controlling whether the loop continues or terminates.
- They collectively map to an abstract measure of how close the loop is to termination, which is the loop variant.
- They must change for each iteration, meaning at least one of them should be assigned a new value. Otherwise the loop will run indefinitely.
- They are typically also part of the loop invariants and they help describe how much work has been done. In fact, you can hardly analyze or describe a loop without reference to the loop control variables.

2. **State-Carrying Variables**: stateful variables that accumulate or evolve state through the loop.
- They often embody intermediate product of the loop.
- They represent state being built, accumulated, or transformed across iterations.
- They are directly tied to loop invariants, which describe their meaning per iteration.
- They have values that hold semantic meaning tied to the loop's purpose, even though they don't necessarily affect loop flow.
- They may or may not appear in loop variants or loop conditions.

3. **Incidental Variables**: all other stateful variables. Although they also have many kinds, we don't subdivide them further.
- They are modified inside the loop but neither drive the loop's control logic nor carry meaningful role in loop invariants.
- They mainly support secondary purposes such as side effects or auxiliary operations that are unrelated to loop control or algorithmic correctness. 
- They include any variables defined inside the loop. These variables are short-lived and temporary because they don't carry information across iterations.
- They include any variables that serve external purposes, such as logging, I/O, metrics collection etc. They are part of the loop's side effects but not tied to its main purpose.
- They include any variables that are part of the legacy or misplaced code. They could be never used and don't affect anything, serving no functional purposes. 

The above classification of variables are based on their access pattern and role in control or logic.
- It's possible for an variable to be both Loop Control Variable and State-Carrying Variable, and this often means it's part of both the variants and invariants.
- For Stateful Variables, we say a variable could be modified when there is at least one statement in the loop that modifies its value. In the actual execution, its value may stay unchanged due to branching, but as long as there is possibility it could be modified, we don't consider it a constant.

---

When we analyze a loop, the meaning of the Contextual Variables are often straightforward. But for the Stateful Variables, many people struggle to properly define their meanings. Let’s revisit a classic example:

```python
nums = [...]              # Contextual variable: input list
n = len(nums)             # Contextual constant: defines loop range
total = 0                # State-Carrying Variable
for i in range(n):       # Loop Control Variable
    total += nums[i]     # state evolves
```

For **Contextual Variables** we can describe their role in loop control logic and how they provide context. In the example, `n = len(nums)` determines the range of the loop and stays constant throughout. The list `nums` provides the data source being operated on and remains unchanged, serving as context for computing the state-carrying variable `total`.

For a **Loop Control Variable**, we can combine the initialization, termination and update of the control variable to describe/define it, and stress its connection to the loop variant/invariant. In the above example,
- **Meaning of `i`**: it is the loop control variable. Specifically, it's the index we use to access elements of the list `nums` one by one. In Python, the loop `for i in range(n):` initializes `i` to `0` and increases it by `1` per iteration. The loop stops automatically when `i == n`, ensuring that the values of `i` cover the range `[0, n - 1]` — exactly once for each element.
- **Connection to Loop Variants**: the loop variant in this case is the number of unprocessed elements, which is the size of the sublist `nums[i:n]`. As i increases, the number of elements left to process shrinks. When `i == n`, the sublist is empty and the loop terminates.
- **Connection to Loop Invariants**: `i` tracks how much work has been completed. At the start of iteration `i`, the first `i` elements (`nums[0:i]`) have already been processed. The loop invariant is often stated in terms of `i` to describe the current state of progress.
- **Oversimplification**: A common oversimplification is to say: “In each iteration, `i` points to an element that gets added to `total`.” While technically true, this misses the broader role of `i` — it not only selects elements, but also tracks progress and defines the loop’s structure. Understanding `i` in terms of loop control and invariants provides a more complete picture.


- **Oversimplification**: when new programmers are asked about the meaning of `total`, some say "it's the sum of all elements in the integer array `nums`". 
	- They are not entirely wrong about the purpose, but they are wrong about the timing: they forget that `total` is a stateful variable, not a constant. Its value changes during the loop, and it only becomes the total sum after termination. So if we access it within the loop, we cannot assume it stores the sum of all elements.
	- The biggest problem about this definition is that they don't see that `total` is modified by the loop for every iteration. Thus we should define the meaning of `total` with regard to the iterations of the loop, that is to contextualize its meaning per iteration, which often leads to an invariant.
- Normally, you will see that loop invariants are often the properties or definitions of some State-Carrying Variables, but Loop Control Variables are often used as parameters in loop invariants.

For a **State-Carrying Variable**, we describe how it is initialized, how it connects to the loop invariant, how it is updated to maintain that invariant, and what it tells us after the loop ends. In the example:
- **Meaning of `total` and the Loop Invariant**: `total` accumulates the running total of elements in `nums`. At the start of iteration `i`, `total` holds the total sum of the sublist `nums[0:i]` (i.e., elements before index `i`). This invariant captures how much progress has been made in the accumulation process, and it depends on both the state of `total` and the loop control variable `i`.
- **Initialization, Maintenance, and Termination**: `total` is initialized to `0`, which correctly reflects the sum of an empty sublist `nums[0:0]`. In each iteration, the element `nums[i]` is added to `total`, thus preserving the invariant that `total == sum(nums[0:i+1])` at the end of iteration `i`. When the loop terminates (i.e., when `i == n`), all elements have been processed and `total` contains the total of all values in the list.
- **Oversimplification**: A common mistake is to say: “`total` is the sum of all elements in `nums`.” While this is eventually true, it is **not** true throughout the loop. `total` is a stateful variable — its value evolves with each iteration. Assuming it represents the total sum at any point inside the loop is incorrect. The proper way to understand `total` is to define it **in terms of the current iteration**, which naturally leads to the loop invariant. In practice, loop invariants often describe the evolving meaning of state-carrying variables like `total`, while loop control variables like `i` are used as parameters that define the current “position” or “progress” of the loop.

There are not **Incidental Variables** in this example, but we will see some in later ones.

---

**How to Define Stateful Variables**:
- Clearly states initialization (e.g. $0$).
- Clearly states how it's modified in the loop body. 
	- The update step (e.g. increment by $1$) for Loop Control Variables.
	- The maintenance step for State-Carrying Variables.
- Clearly states termination
	- The termination condition for Loop Control Variables (e.g. `i == len(nums)` ).
	- The useful properties we can get from State-Carrying Variables after termination. (e.g. $total$ is the total sum of the array).
- Other useful information
	- For Loop Control Variables, we can explicitly states coverage of the range and its guarantee to termination (e.g. it ensures that each element is processed exactly once).

**Summary**:
| Category                     | Role                                  | Access    | Commonly Tied to | Affects Control Flow? | Example                     |
| ---------------------------- | ------------------------------------- | --------- | ---------------- | --------------------- | --------------------------- |
| **Contextual Variables**     | Define fixed boundaries or parameters | Read only | Loop Condition   | No                    | `len(nums)`           |
| **Loop Control Variables**   | Drive loop condition and update       | Modified  | Loop Variant     | Yes                   | `i`, `left`, `right`        |
| **State-Carrying Variables** | Carry intermediate or final results   | Modified  | Loop Invariant   | No                    | `total`, `prefix`, `result` |
| **Incidental Variables** | Side effects or Auxiliary operations   | Modified  | Other Purposes   | No                    | `logger`, `tmp` |

We will see more examples in later sections.

#### Good Questions to Ask

What Makes a Good Invariant:
- It connects meaningfully to the loop goal.
- A non-trivial property that involves at least one variable modified in the loop.
- It's always true during loop execution, and is preserved across all iterations.
- It's strong enough to help prove the loop works correctly, and must be true for the loop to behave correctly.

Summary of Key Questions a Programmer Should Learn to Ask:
- What is the loop trying to accomplish?
- What must be true before and after every iteration?
- What variables or data structures capture this truth?
- How does this help me prove the loop is correct?
- What happens to the invariant when the loop ends?

How to Identify Loop Invariant?
- Is this property something that needs to be preserved by the loop body? Or is it just always true and unaffected?
- If the loop must act to preserve the property, then it's likely a loop invariant.
- If the property is just always true regardless of the loop, it’s not a meaningful loop invariant.

How to Classify Stateful Variables?
- Does this variable determine whether the loop stops? -> Loop Control
- Does this variable accumulate a result or represent evolving state? -> State-Carrying
- Is this variable modified but unrelated to correctness or flow? -> Incidental

#### More Examples

In this section, we will go through some simple but typical algorithms and list their loop variants and invariants. To make full use of these examples, you can look at the code first, think about what are the loop variants and invariants, and then go on toe see the answers.

In the Methodical Loop Implementation section, things will go wild and we will use variants and invariants to solve more complicated problems.

---

**Printing to Console**

```python
i = 10
while i > 0:
    print(i)
    i -= 1
```

- **Contextual Variables**: None.
- **State-Carrying Variables**: None.
- **Loop Control Variable**: `i` (appears in the condition and is modified each iteration)
- **Loop Variant**: `i` itself (initially `10`, decreases by `1` each time, loop stops when `i == 0`)
- **Loop Invariant**: At the start of each iteration, the integers from `i+1` to `10` have already been printed to the console.
- **Loop Termination**: The loop stops when `i == 0`.

---

**Finding Max**

```python
max_val = nums[0]
for i in range(1, len(nums)):
    if nums[i] > max_val:
        max_val = nums[i]
```
- **Contextual Variables**: `nums` — the input list (assumed non-empty).
- **Loop Control Variable**: `i`, ranging from `1` to `len(nums) - 1`.
- **State-Carrying Variable**: `max_val`.
- **Loop Variant**: `len(nums) - i`, representing the number of elements remaining — i.e., the size of `nums[i:]`.
- **Loop Invariant**: At the start of iteration `i`, `max_val` holds the maximum element in `nums[0:i]`.
- **Loop Termination**: When the loop ends (`i == len(nums)`), `max_val` contains the maximum element in the entire array.
- Note: This version assumes that `nums` is not empty. A more general version is:

```python
max_val = float('-inf')
for i in range(len(nums)):
    if nums[i] > max_val:
        max_val = nums[i]
```

---

**Summing an Array**

```python
n = len(nums)
total = 0
for i in range(n):
    total += nums[i]
```

- **Contextual Variables**: `nums`
- **Loop Control Variable**: `i`
- **State-Carrying Variable**: `total`
- **Loop Variant**: `n - i`, which is the number of unprocessed elements in `nums[i:n]`
    - Initially: `n - 0 = n`
    - Decreases by 1 each iteration
    - When `n - i == 0`, i.e., `i == n`, the loop terminates
- **Loop Invariant**: At the start of iteration `i`, `sum` holds the total of `nums[0:i]`
- **Loop Termination**: `total` is the total sum of the array
- Next we take a look at several variants of this program.
---

**Summing an Array Variant 1**

```python
bound = 100  # example threshold
total = 0
for i in range(len(nums)):
    if total >= bound:
        break
    total += nums[i]
```

- Contextual Variables: `bound` and `nums`.
- Loop Control Variables: `i` and `total`.
    - The loop range is defined using i, and its progression (from $0$ to `len(nums)`) implicitly determines termination.
    - The variable are modified within each iteration of the loop.
- State-Carrying Variables: $total$.
- Loop Variants: 
    - The loop’s progress can be measured by the minimum of two variants:
        - Number of elements remaining in the array: $len(nums) - i$
        - Remaining allowed sum before exceeding bound: $bound - total$
    - In each iteration, one or both decrease:
        - `len(nums) - i` decreases by $1$.
        - If all elements in `nums` are non-negative, then `bound - total` decreases by `nums[i]`.
    - This ensures that the loop cannot run forever.
- Loop Invariants: 
    - At the start of the `i-th` iteration, $total$ is the sum of the subarray `nums[0:i]` (i.e., elements with indices `0` through `i-1`).
    - $total < bound$ is not a loop invariant, although it appears in the loop condition, but it can be false immediately after the loop ends, which disqualifies it as an invariant.
- Loop Termination:
    - The loop ends when either $i = len(nums)$ or $total \ge bound$, or both.
    - Because the loop uses an early break, we cannot tell from the final values alone whether the loop ended due to reaching the end of nums or because total exceeded the bound.
    - Loop invariant is maintained after termination.
- Notes:
    - When a variable (like $total$) directly affects loop termination, it indeed becomes part of the loop control mechanism and loop variant. And it could also be part of loop invariant. This is the case where the same variable is both a Loop Control Variable and State-Carrying Variable.
- Notice that the above code is equivalent to the following:

```python
bound = 100
total = 0
for i in range(len(nums)):
    if total >= bound:
        break  # early exit
    total += nums[i]  # modifies state
```
- For this implementation, even when the early exit takes place, the loop invariant is still maintained after termination.

---

**Summing an Array Variant 2**

```python
bound = 100  # example threshold
total = 0
for i in range(len(nums)):
    total += nums[i]  # modifies state
    if total >= bound:
        break  # early exit
```
- Contextual Variables: `bound` and `nums`.
- Loop Control Variables: `i` and `total`. (similar to Summing an Array Variant 1).
    - `i` controls the iteration index and defines the loop range.
    - `total` appears in the `if` condition, influencing whether the loop exits early via `break`. It is updated inside the loop, making it part of control flow.
- State-Carrying Variables: `total`. It accumulates the running sum of elements from `nums`.
- Loop Variant: 
    - The loop’s progress can be measured by the smaller of:
        - The number of remaining elements: `len(nums) - i`
        - The remaining allowed sum: `bound - total`
    - In each iteration, one or both of these values decrease:
        - `len(nums) - i` decreases by 1
        - If `nums[i]` is non-negative, then `bound - total` decreases by `nums[i]`
- Loop Invariant: At the **start** of the `i-th` iteration, `total` is the sum of the subarray `nums[0:i]`
    - This invariant is violated inside the iteration after `total += nums[i]` executes.
    - If the loop **continues**, the increment of `i` at the next iteration restores the invariant.
    - But if the loop **breaks** immediately after the update, the invariant no longer holds at loop termination, because `total` now reflects the sum of `nums[0:i+1]`.
- Loop Termination:
    - The loop ends either when all elements have been processed (`i == len(nums)`) or when the `total` exceeds or equals the `bound`
    - Because of the `break`, we cannot infer from post-loop values alone which condition caused termination.
    - Also, if the loop exits immediately after updating `total`, the invariant does **not** hold after the loop, unless we redefine it to include `nums[0:i]` instead of `nums[0:i-1]`.


Notice that the above code is equivalent to the following:

```python
total = 0
i = 0
while i < len(nums):
    total += nums[i]  # modifies state
    if total >= bound:
        break
    i += 1
```
---

**Binary Search**

```python
def binary_search(nums, target):
    left = 0
    right = len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2  # integer division
        if nums[mid] > target:
            right = mid - 1
        elif nums[mid] < target:
            left = mid + 1
        else:
            return mid
    return -1  # not found
```

- Contextual Variables: $target$ and $nums$.
- Loop Control Variables: $left$ and $right$.
- State-Carrying Variables: None.
- Incidental Variables: $mid$.
- Loop Variant: size of elements in subarray $nums[left:right+1]$ (the search space), which is $right - left + 1$
    - It strictly decreases each iteration, ensuring termination.
    - In fact the midpoint calculation and the update on $left$ and $right$ is non-trivial, and incorrect implementation could cause infinite loop.
- Loop Invariant: if the $target$ is in the array, it's in the interval $nums[left:right+1]$.
- Loop Termination:
    - The loop ends when either $left > right$ or $target$ is located in the array.
    - We can use the return value to determine which case is true: $-1$ if it's the former case and a non-negative integer number for the latter case.

---

**Searching for the First Element Satisfying a Condition**

```python
for num in nums:
    if num % 2 == 0:
        return num
return -1
```

- Contextual Variables: $nums$.
- Loop Control Variable: `num`, iterating directly over `nums`.
- State-Carrying Variables: None.
- Loop Variant: the number of unprocessed elements, i.e., the remaining suffix of `nums` starting from the current `num`.
- Loop Invariant: all elements seen so far (i.e., before `num`) are not even numbers.
- Loop Termination:
  - Normally, the loop reaches the end of `nums`, in which case the invariant implies that no even number exists in the list.
  - Or it exits early via `return` as soon as an even number is found.
- Notes:
  - The invariant still helps explain the correctness — but does **not** necessarily hold after the loop if exited early. And that’s OK because the postcondition is separately ensured by the `return`.

---

## Methodical Loop Implementation

### From Analysis to Implementation

- Loop analysis is essential for reading and understanding looping code, and it serves as the foundation for effective loop implementation. In fact, you can only learn to write robust loop constructs after you can methodically read and reason about them.
- Loop implementation is closely tied to the techniques introduced in loop analysis. The primary work in implementing a loop is correctly determining each of the loop's elements.
- In this section, we will explore a systematic approach to implement looping code methodically, focusing on clear code structure and meaningful documentation.
- Typically, when solving a coding problem you must design an algorithm and then implement it. However, because this section emphasizes implementation, we provide the high-level design for each algorithm. You are encouraged to implement the algorithm using that design on your own first, and then continue reading for the detailed explanation.
- Depending on the problem, some algorithms are harder to design while others are harder to implement. This section concentrates on methodical implementation, assuming we already have the high-level algorithm idea in place, which itself can be the more challenging part in practice.
- We will present several case studies. The first case study of Bubble Sort will be discussed in verbose detail, and later case studies will be explained more concisely.

### Bubble Sort

#### High-Level Design

Sorting algorithms are among the most classic in computer science. **Bubble Sort** is one of the simplest sorting algorithms and serves as a good learning example. For our discussion, assume we need to sort an array in non-decreasing order.

*Note:* This section is not a comprehensive treatment of sorting algorithms or even Bubble Sort specifically. We are only focusing on how to methodically implement it.

**High-Level Design Perspective**:
- Bubble Sort is a simple comparison-based sorting algorithm that repeatedly traverses the array, comparing adjacent elements and swapping them if they are out of order (i.e., the left element is greater than the right). This process continues until the array is fully sorted.

**High-Level Procedures**: Break the sorting process into the following steps
- Start from the end of the array.
- Compare each pair of adjacent elements.
- If the left element is greater than the right one, swap them.
- Repeat this process until you reach the end of the array, leaving the smallest unsorted element at the far left.
- On the next pass, repeat the process again, but ignore all elements to the left of the last smallest element inclusively.
- Continue until no more passes are needed, that is when the array is sorted.

This version of Bubble Sort selects the minimum element on each pass. Conversely we can also select the maximum element, and the whole process just mirrors the above steps.

<img src="img/bubblesort_visualization.png" alt="bubblesort_visualization.png" style="width: 30%; height: 60%; display: block; margin-left: auto; margin-right: auto;">

**Algorithm Visualization**
- The image above provides a visual representation of this procedure.
- The grey portion doesn't participate in the later passes.
- Such visualizations can help us understand the algorithm's flow when writing code. 
- In fact, experienced programmers often visualize these steps mentally for simpler problems, quickly pinpointing key details and coding the solution. With sufficient practice, you will be able to do the same.

Before continuing, please **implement Bubble Sort on your own** following the steps outlined above. As you do so, annotate your loop control structure and specify what invariants you're trying to maintain. We will later go into mid-level design and then implement the algorithm based on these steps as well.

**Selection Sort** is another sorting algorithm very similar to Bubble Sort. After finishing the Bubble Sort case study, you are encouraged to implement Selection Sort yourself and analyze its loop elements to reinforce these concepts.

#### Mid-Level Design

Compared to high-level design, which describes the algorithm's conceptual flow, mid-level design translates that concept into program-level procedures without writing actual code, which is the low-level implementation.

**Applying Repetition**:
- Comparing the adjacent elements in the array repeatedly strongly suggests using loop/iteration. 
- According the step-by-step description, adjacent comparisons are performed repeatedly. Yet, even after we finish comparing all adjacent pairs of elements, which is one full pass, we only move the smallest element to the leftmost position, which is still far from sorting the entire array. 
- However, by repeating this entire process (a repetition of repetition), the sorted portion of the array grows while the unsorted portion shrinks. Eventually, the sorted portion becomes the entire array and the unsorted portion is empty.
- During each repetition, we make sure each time we extend the sorted subarray, only one element will be appended to the end without disrupting the already sorted prefix. That element is the minimum of the unsorted array, but is also no less than the maximum of the sorted subarray. This guarantees all remaining elements in the unsorted subarray are still no less than all elements in the sorted subarray. That is `min(unsorted subarray) >= max(sorted subarray)`.
- This analysis suggests that **two layers of loops** are required: the first repetition is the inner loop and the second is the outer loop.
- *Note:* when describing an array or subarray as "unsorted" here means we are not sure if it is sorted. It might already be sorted, but we cannot be certain without examining its elements.

**Mid-Level Design** of Bubble Sort:
- Bubble Sort partitions the entire array into two sections, a sorted subarray followed by an unsorted subarray. With each iteration of the algorithm, the sorted subarray increases in size by 1 while the unsorted subarray decreases by 1. This involves two layers of loops.
- In the outer loop, it handles the unsorted subarray, shrinking its size by 1 each iteration, and accordingly, growing the size of the sorted subarray by 1.
- In the inner loop, it iterates through the unsorted subarray and compare each adjacent pair, swapping them if they are in the wrong order. After the loop terminates, the smallest element of the unsorted subarray will be in its correct final position, which is like "bubbled" to the front of that subarray.

```python
def loop_for_each_unsorted_subarray():
    loop_for_pair_exchange()
```

The pseudocode above captures the conceptual design. We still need to translate it into actual code by declaring variables, using statement sequence, branching and looping to realize this idea. Let's go through this example to see how we can methodically implement two layers of loops.

#### Outer Loop Implementation

**Primitive Analysis**:
- For the outer loop, the **Loop Goal** is identical to the intent of the entire method, that is to sort the input array $arr$ in non-decreasing order. 
- The **Loop Premise** is the same as the method's precondition: the array $arr$ is not None.
- Obviously, an array of length 0 or 1 is already sorted and requires no operations. These are edge cases you might handle with a quick check and return. For example:

```python
def bubble_sort(arr):
    if arr is None or len(arr) <= 1:
        return
    # rest of sorting code
```

- In an actual interview, you should explicitly ask the interviewer whether whether `None` inputs are possible. For brevity here, we assume it's not the method's responsibility to handle null or invalid input, instead the precondition of the method states that the input array can not be None, and it's the caller's responsibility to honor that contract. So we don't consider this edge case in our implementation.
- The check for `len(arr) <= 1` is simple in this context. In general cases, handling edge cases might take a few extra lines of code. My recommendation : it's acceptable to recognize the edge cases before or after you finish the main logic, but **do not rush to implement code that specifically handle edge cases too early**. The reason is simple: with minor or no tweaks, the code for general logic can often handle edge cases gracefully.

In the following discussion, let's use $n$ to denote $len(arr)$ for brevity.

<img src="img/bubblesort_outer_loop.png" alt="bubblesort_outer_loop.png" style="width: 30%; height: 70%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the outer loop does.
- We can see how how the State-Carry Variable $arr$ changes from iteration to iteration.
- The number of iterations is predetermined by $n$: the unsorted subarray shrinks from the whole array to empty while the sorted subarray goes through an opposite process. 
- This leads to a natural conclusion that we can use a `for` loop as the outer loop, where the number of iteration required is known. We can use $i$ as the Loop Control Variable in this case.

**Definition of the Loop Control Variable**:
- Next we determine the meaning/definition of $i$ and its range of values. 
- This is where things get interesting because we have multiple choices here and although all of them can lead to correct implementation, some are more convenient than others. 

- **Counting-Based Definition**:
    - You may think that we can count the number of iteration and use $i$ as an iteration counter. 
    - Suppose the number of iterations is $X$, and in the `for` loop header we can let $i$ go from $1$ to $X$, or equivalently from $0$ to $X-1$, from $X$ to $1$, and from $X-1$ to $0$, etc. You can choose one based on your coding habit and preference. But what is $X$ exactly?
    - Take a look at the graph, and note the size of the unsorted sub-array over time (or the sorted subarray because they complement each other). 
    - Initially it's $n$, for each iteration it decreases by $1$, and eventually it's $0$ when the loop terminates. So the last valid value should be $1$. From $n$ down to $1$ gives $n$ iterations, so $X=n$. 
        - However, if you take a closer look, we don't need to perform any comparisons or swaps when the subarray has only 1 element, so it's ok to leave the last valid value to be $2$, which is effectively using $X=n-1$ instead.
    -  In practice, it’s normal to adjust the loop range after writing the loop body. 
        - For example, in the `for` loop header, you may go back and forth when writing the loop to change the initial value of $i$ or its final value. 
        - Because before you finish the loop body, you only have an abstract overview of what the algorithm does without much concrete details. After you finish it, you can use the techniques of loop analysis to see if there are mistakes and correct them accordingly. - That being said, if you are uncertain about the exact loop range, like the start and end value for $i$, you can proceed to finish the loop body first and then revisit the loop bounds to decide the proper values.
    - However, most of the time, counting the number of iterations and use it as the loop range is not the best choice. 
        - It may not be obvious in this case, but in a lot of algorithms, it would make your code harder to read and write.
        - Because it doesn't closely reflect the evolving **loop variant** or the work done in the loop body.
- **Variant-Based Definition**:
    - Take a look at the image and think: what is the loop variant?
    - It's the size of the unsorted subarray, or equivalently the size of the sorted subarray, but since they complement each other, either would work. This prompts us to consider defining $i$ in terms of the loop variant, the property that changes each iteration.
    - You could define $i$ to be the size of the unsorted subarray. 
        - Initially it's $n$, and after the loop terminates it's $0$. So the loop range could be from $n$ to $1$. 
        - But in fact as we have analyzed, in the last iteration we only need to handle an unsorted subarray of 2 elements, so we can also let $i$ go from $n$ to $2$.
    - In this scheme, the loop control variable directly tracks the loop variant: they are identical. 
        - As you will see, in the inner loop, we need to examine all pairs of adjacent elements in the unsorted subarray, which is delimited by the start and end indices. Its end index is fixed as $n-1$, and what is the start index for a particular $i$? 
        - Defining $i$ strictly as the unsorted subarray size would require translating that into array indices. We need to use equation to determine the value of the start index. 
        - Suppose the start index is $s$, then $n-1-s+1$, which is the length/size of the unsorted subarray, should be equal to $i$, then we have $s=n-i$. So the start index is $n-i$. It’s doable but not as straightforward.
    - Using this definition would work, but in hindsight we have a better choice. 
        - Because loop control variable is not only used in loop variant to determine the number of iterations, it's also used in the loop body to delimit the subarray range that we need to perform work on in the inner loop, so we should consider implementation convenience too. 
        - We need to strike a balance between these two different requirements. Notice that loop variant is more for correctness analysis, so we can put implementation convenience first when writing the code.
- **Index-Based Definition**:
    - A more intuitive choice is to let $i$ be the start index of the unsorted subarray in each iteration, which is my personal pick.
    - In the visualization, this is the clearest thing changing each time: on each outer loop iteration, the unsorted section starts one position further to the right. Using the end index of the sorted section would be similar but off by one.
    - This definition isn't identical to the loop variant but they are directly connected: as $i$ increases and moves toward the end of the array, the size of the unsorted subarray decreases correspondingly.
    - It also simplifies the inner loop, because inside the outer loop, the unsorted subarray is fixed as $arr[i:n]$ (or simply $arr[i:]$), which is simple and clear. We can write the inner loop bounds clearly without needing to compute indices like $n-i$, which reduces the chance of off-by-one errors.
    - Although using Counting-Based or Variant-Based definitions also works, as you can always set up equations to solve for the start index, they are not as straightforward as directly using the start index. 
    - It's going to take longer time for the reader to understand and be convinced, and you also need more documentation to explain the code. You may be worrying about questions like "is it $n-i$ or $n-i-1$?".
- By now we've considered three definitions of  $i$:
    - **Index-Based**: `i = start index of unsorted subarray`
    - **Variant-Based**: `i = size of unsorted subarray`
    - **Counting-Based**: `i = iteration count`
- The choices of the loop control variables vary from algorithm to algorithm, and different algorithms may favor different choices. 
    - For example, in binary search, the number of iterations depends on the input array, which is unknown to the programmer when writing the code, so Counting-Based definition is not workable at all. 
- Index-Based definition is a subset of Range-Based definition, and in practice it's often the most intuitive and convenient choice when implementing a loop, though ultimately it can be a matter of personal preference.

Coding is often a **non-linear thought process**. Unlike some tasks, you may not simply go top-down without revisiting earlier decisions.
- For instance, choosing $i$ to be the start index of the unsorted subarray requires thinking ahead about how the inner loop operates and uses $arr[i]$.
- You have to think about the inner loop when implementing the outer loop because they are interlocked and correlated.
- So in general, an overview of the entire loop structure before you coding is helpful. And if you can't see things clear from the start, it's absolutely fine to write what you can, and go back and forth to adjust you code to make the whole structure coherent.

Now, using an index-based definition for Bubble Sort’s outer loop, we have the framework of the outer loop:
```python
for i in range(len(arr)):
    # inner loop
```

**Loop Elements Analysis**. Let's summarize the the outer loop element we have determined so far:
- **Loop Goal**: sort $arr[0:n]$ in non-decreasing order.
- **Loop Premise**: $arr$ is not `None`.
- **Loop Initialization**: initialize $i=0$ in the `for` loop header.
- **Loop Condition**: the loop continues as long as the unsorted subarray is not empty. 
    - In Python, writing `for i in range(n):` automatically enforces the condition $i < n$.
- **Loop Termination**: we terminate the loop when unsorted subarray is empty. 
    - The loop terminates after the last iteration with $i = n-1$, at which point the entire array is sorted.
- **Edge Cases**: $n=0$
    - Using the loop condition we picked, no iteration is required and correctness is guaranteed. No additional code is required.
- **Loop Variant**: the size of the unsorted subarray, which is $n-i$. 
    - We decrease it by $1$ each iteration, handled automatically by range(), ensuring the loop will terminate.
- **Loop Control Variable**: $i$ is the loop control variable acting as the partition boundary.
    - It's the start index of the unsorted subarray that we handle in `i`-th iteration, while the end index is always $n-1$. 
    - At the star of an iteration, the sorted subarray is $arr[0:i]$, while the unsorted subarray is $arr[i:n]$.
    - Its initial value is $0$, which corresponds to the entire array, and in Python it is automatically incremented by the $for i in range(n):$ construct. 
    - The loop continues until $i$ reaches $n$, ensuring that the value of $i$ covers the range of $[0 … n-1]$, inclusive of both ends, making sure each iteration we move only one element from unsorted portion to sorted portion.

*Side Note:* using `for i in range(n-1):` as the loop condition is also ok. 
- Doing so reduces one unnecessary iteration when the remaining subarray has only $1$ element, but makes the elements a little different and complicated:
- **Loop Condition**: the loop continues as long as the unsorted subarray has more than one element, which is equivalent to $i < n-1$. In Python, this is naturally written as `for i in range(n-1):`.
- **Loop Termination**: we terminate the loop when unsorted subarray has less than one element. 
    - When $n \ge 1$, the last executed iteration has $i=n-2$, after which the array is fully sorted.
    - When $n=0$, after the loop terminates $i=0$, the array is trivially sorted.
- **Edge Cases**: $n=0$ or $n=1$, using the loop condition we picked, no iteration is required and correctness is guaranteed. No additional code is required.

Now let's figure out the rest of the outer loop's implementation. 
- Since **Loop Body** is just the code itself, and in our case it's the inner loop, because the outer loop mainly sets the bounds of the unsorted subarray, so no more breakdown is required. 
- **Iteration Goal** is determined by **Loop Invariant**, so the implementation of the loop body eventually falls upon our definition and understanding of the loop invariant. 
- Determining the loop invariant for Bubble Sort is not trivial. Consider pausing to think about it.
- Recall that loop invariants are about the unchanging truth of the changing components. 
    - For each iteration, *the sorted subarray grows by one element, but still remains sorted*, which is a truth across all iterations and helpful in sorting the entire array, because eventually the sorted subarray covers the entire array. So it's naturally part of the loop invariant.
- There is more to the loop invariant than simply stating that the sorted subarray grows by one and still sorted. 
    - According to the steps of the algorithm, after each inner loop terminates, the first element(the leftmost one) of the unsorted subarray will be the minimum of the unsorted subarray and it also becomes the last element(the rightmost) of the sorted subarray. 
    - For this transformation to work, the following statement must be true: *the minimum of the unsorted subarray is no less than the maximum of the sorted subarray*.

**Loop Elements Analysis(cont.)**. Now we can state the outer loop invariant and iteration goal clearly:
- **Loop Invariant**: at the start of `i`-th iteration, $arr[0:i]$ is sorted in non-decreasing order, and max($arr[0:i]$) <= min($arr[i:n]$).
- **Iteration Goal**: push min($arr[i:n]$) to $arr[i]$ by swapping adjacent elements, making $arr[0:i+1]$ sorted and $arr[0:i]$ unchanged.

With all the outer loop elements defined, we can proceed to implement the loop body, the inner loop. But before we do that, let's briefly verify **how loop invariant ensures correctness**. This step is not required in the implementation of the algorithm, but serves as another good exercise of loop analysis.

**Loop Invariant Analysis**:
- **Initialization**: initially before any iteration, $i = 0$. The invariant states $arr[0:0]$ is sorted. Notice that this is an empty subarray, so the invariant is trivially true (we can think of max($arr[0:0]$) as $- \infty$). The first iteration will move the minimum of the whole array to $arr[0]$, making max($arr[0:1]$) <= min($arr[1:n]$) true. 
- **Maintenance**: Assume the invariant holds at the start of `i`-th iteration. The inner loop will move the minimum of the unsorted subarray $arr[i:n]$ to index $i$. This new element is at least as large as all previously sorted elements by our invariant, so at the end of the iteration, max($arr[0:i+1]$) <= min($arr[i+1:n]$) and thus the invariant is maintained for the next iteration.
- **Termination**: when the loop terminates, $i=n$, so the sorted subarray is $arr[0:n]$, which is the entire array, accomplishing the loop goal.

#### Inner Loop Implementation

<img src="img/bubblesort_inner_loop.png" alt="bubblesort_inner_loop.png" style="width: 40%; height: 80%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the inner loop does: it compares adjacent elements and swaps them if needed.
- The inner loop is relatively straightforward, because from the outer loop analysis, we already know the range of elements we need to examine($arr[i:n]$) and the operation we need to perform (adjacent swaps of out-of-order pairs).
    - The loop goal of the inner loop is essentially a subset of the outer loop's iteration goal, without the context of the sorted subarray because the inner loop doesn't concern itself with elements before index $i$ and it only focuses on the unsorted subarray. 
    - No particular loop premise, because the subarray won't be empty according to the outer loop conditions.
    - For the edge cases, the inner loop don't need to worry about empty subarray, but need to handle subarray of only one element.
    - The start and end indices of the unsorted subarray is clear: $arr[i:n]$, so it's natural for us to use a `for` loop and choose a loop control variable, say $j$, to cover all possible adjacent pairs of elements and perform the swap/exchange if necessary. 
    - In addition, after the termination of the inner loop, $j$ is useless for the outer loop, because we won't use other exit conditions in the loop, so it would always end up as a fixed-value, which is another reason for us to use `for` loop and declare it in the `initialization` part of the header.
    - The loop variant is obvious: the remaining number of pairs of adjacent elements in the unsorted subarray. As we move 
- **Iteration Direction**: 
    - In the inner loop implementation, the **iteration direction** is more important than the other elements and it determines what $j$ represents: from $i$ to $n-1$ or from $n-1$ to $i$.
    - Since we can only exchange adjacent elements, iterating from the end of the subarray to its start (from $n-1$ to $i$) can work but not the opposite direction.
    - This is because we need to move the minimum element in the subarray to the leftmost position, which could only be done if we iterate from right to left. 
    - This has to do with the loop invariant: if you iterate from left to right, at the `j`-th iteration, only $arr[j]$ could be exchanged, and all elements in $arr[i:j]$ won't be touched, so if $arr[i]$ is not already the minimum of $arr[i:n]$, which is totally possible, you can't achieve your loop goal after the loop terminates.
    - *Note:* On the other hand, iterating from left to right moves the maximum element in the subarray to the rightmost position. So you are implementing the BubbleSort by selecting the maximum for each iteration, you need to use this direction.
- **Definition and Range of $j$**:
    - As for the meaning of $j$, we again have many choices.
    - Using $j$ as the counter of iterations works. You can definitely count the number of iterations, then let $j$ represent the first iteration ($j = 0$), the second iteration ($j = 1$), and so on. You could also use the reverse order of counting, from $j = n - i - 1$ down to $1$.
    - You could also directly use the loop variant. The number of elements in the subarray is $n-i$, so there are $n-i-1$ pairs in total.
    - Similar to the outer loop analysis, *the indices of the pairs of elements to compare* is most obvious in the image. The two indices in a pairs makes not much difference, so we can choose $j$ as the index of the element with larger index.
    - By doing so, for each iteration the pair we examine is $(arr[j-1],arr[j])$.
- Once the iteration direction and the meaning of $j$ are settled, most of the other elements are also clear.

**Loop Elements Analysis**. Now we have figure out the following elements:
- **Loop Goal**: push min($arr[i:n]$) to $arr[i]$ by swapping adjacent elements.
- **Loop Premise**: none.
- **Loop Initialization**: initialize $j=n-1$ using `for j in range(n-1, i, -1):`.
- **Loop Condition**: the loop continues as long as there are more pairs to examine. 
    - Combined with loop termination and edge case analysis, we see that $j>i$ is a proper expression for loop condition.
- **Loop Termination**: we terminate the loop when there are no more pairs to examine. 
    - The last pair to examine is $(arr[i],arr[i+1])$, and this is when $j=i+1$. 
    - So after the loop terminates $j=i$, and $arr[i]$ is the minimum in the unsorted subarray.
- **Edge Cases**: 
    - When the size of the unsorted subarray is $1$, no work is required; 
    - When $j=i$, that is when $j$ points to the first element of the unsorted subarray, we don't need to compare $(arr[i-1],arr[i])$, because $arr[i-1]$ is not in the range that inner loop cares about.
    - For both cases, no additional code is required.
- **Loop Variant**: the number of unexamined adjacent pairs in the unsorted subarray, which is $j-i$. 
    - We decrease it by 1 for each iteration, which in Python is handled automatically by the `range(n-1, i, -1)` construct.
- **Loop Control Variable**: $j$ is the loop control variable. 
    - We use it to scan all adjacent pairs of elements in the unsorted subarray from right to left, and for the `j`-th iteration it points to the pair $(arr[j-1],arr[j])$. 
    - Its initial value is $n-1$, which corresponds to the last pair $(arr[n-2],arr[n-1])$, and we decrement it by $1$ for each iteration. 
    - The loop continues until $j$ reaches $i$, ensuring that the value of $j$ covers the range of from $i+1$ to $n-1$, making sure we examine all pairs and move the minimum to $arr[i]$.

Now we have the framework of the inner loop:
```python
for i in range(n):
    for j in range(n-1, i, -1):
        # loop body of inner loop

```

Now for the rest of the elements:
- The **Loop Body** of BubbleSort's inner loop is in fact very simple: swap the two adjacent elements if they are in the wrong order.
- The wrong order in this case is the order the violates non-decreasing order. The correct order is $arr[j-1] \le arr[j]$, so its negation $arr[j-1]>arr[j]$ is the branching condition for swapping.
- In Python, the most natural way is to use tuple unpacking for swapping: `arr[j-1], arr[j] = arr[j], arr[j-1]`.
- Then what is the loop invariant? How does it relate to our loop goal? Why does swapping adjacent elements move the minimum to the leftmost position? These are good questions that you should pause to think about.
- The key to understand this is that as we move $j$ from right to left and swapping pairs in the wrong order, we always keep the minimum of the examined subarray at the leftmost position. 

**Loop Elements Analysis(cont.)**. Now we have both the loop invariant and iteration goal:
- **Iteration Goal**: make sure $arr[j-1] \le arr[j]$, exchange them if not, and ensure $arr[j]$ is the minimum of $arr[j:n]$.
- **Loop Invariant**: at the start of the `j`-th iteration, $arr[j]$=min($arr[j:n]$).

**Loop Invariant Analysis**:
- **Initialization**: initially, $j$ is $n-1$, so according to the invariant, $arr[j]$=min($arr[n-1:n]$), which is trivially true.
- **Maintenance**: For all later iterations, since we only swap $arr[j-1]$ and $arr[j]$ when they are in the wrong order, that is when $arr[j-1]>arr[j]$, so we have two cases
    - If $arr[j-1]>arr[j]$, then they will be swapped, so at the end of the iteration, $arr[j-1]$ is the minimum in the unsorted subarray, because now it hold the originally value of $arr[j]$. 
    - If $arr[j-1] \le arr[j]$, no swapping, and since $arr[j]$ is already the minimum in the subarray $arr[j:n]$ and $arr[j-1]$ is no greater than it, we can conclude that $arr[j-1]$ is the minimum in the subarray $arr[j-1:n]$. 
    - In both cases, at the start of the next iteration, `j--` has been executed, so the invariant is maintained.
- **Termination**: after the loop terminates, $j=i$, so $arr[i]$=min($arr[i:n]$), accomplishing the loop goal.

#### Complete Implementation

Based on our design, below is the complete implementation of Bubble Sort.

```Python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        for j in range(n - 1, i, -1):
            if arr[j - 1] > arr[j]:
                arr[j - 1], arr[j] = arr[j], arr[j - 1]
```

If you look closer at the code, you could see how the use of index could be messed up:
- Programmer could use $range(n, i, -1)$ instead of $range(n - 1, i, -1)$, leading to $IndexError$ because $arr[n]$ is out of bounds.
- Programmer could write the inner loop condition incorrectly, e.g. $for j in range(n - 1, i - 1, -1)$ which includes $j == i$, causing an unnecessary comparison and possible $IndexError$ when accessing $arr[j-1]$ at $i=0$.
- This is why methodical analysis and implementation could help minimize bugs.


Table for Loop Element Summary
| Element                 | Outer Loop (i)                                              | Inner Loop (j)                       |
| ----------------------- | ----------------------------------------------------------- | ------------------------------------ |
| **Goal**                | Sort the array                                              | Push min element left                |
| **Invariant**           | $arr[0:i]$ is sorted, max($arr[0:i]$) <= min($arr[i:n]$)    | $arr[j]$ = min($arr[j:n]$)           |
| **Variant?**            | $n - i$ (size of unsorted)                                  | $j - i$ (remaining adjacent pairs)   |
| **Control Variable**    | $i$ (start of unsorted subarray)                            | $j$ (index of right element of pair) |

It's a good idea to document your code with the essential loop elements. The extent of documentation is a personal preference, but including the loop invariant is generally very helpful.

```python
def bubble_sort(arr):
    n = len(arr)
    # Loop Goal: sort arr[0:n] in non-decreasing order.
    # Loop Control Variable i: the start index of the current unsorted subarray arr[i:n].
    # Loop Range: i goes from 0 to n-1.
    # Loop Invariant: at the start of i-th iteration, arr[0:i] is sorted and max(arr[0:i]) <= min(arr[i:n]).
    # Iteration Goal: push min(arr[i:n]) to arr[i], making arr[0:i+1] sorted and arr[0:i] unchanged.
    # Edge Cases: n=0 or n=1, no iteration will execute.
    for i in range(n):
        # Loop Goal: push min(arr[i:n]) to arr[i] by exchanging adjacent elements.
        # Loop Control Variable j: current pair to examine is (j-1, j), scanning arr[i:n] from right to left.
        # Loop Range: j goes from n-1 down to i+1.
        # Loop Invariant: at the start of j-th iteration, arr[j] = min(arr[j:n]).
        # Iteration Goal: ensure arr[j-1] <= arr[j], swap if not.
        # Edge Cases: when subarray has size 1, inner loop won't execute.
        for j in range(n - 1, i, -1):
            if arr[j - 1] > arr[j]:
                arr[j - 1], arr[j] = arr[j], arr[j - 1]
```

As complexity analysis is not the focus here, we briefly mention it here.
- Bubble Sort is in-place, with constant extra space, so the space complexity is $O(1)$.
- Due to the two layers of loop, the time complexity is $O(n^2)$. The following table summarizes the complexity of comparisons and swaps. The best case corresponds to the case where the input array is already sorted in non-decreasing order, and the worst case is where the input array is sorted but in non-increasing order.

| Metric      | Best Case    | Average Case | Worst Case |
| ----------- | ------------ | ------------ | ---------- |
| Comparisons | $O(n^2)$     | $O(n^2)$     | $O(n^2)$   |
| Swaps       | $O(1)$       | $O(n^2)$     | $O(n^2)$   |
| Overall     | $O(n^2)$     | $O(n^2)$     | $O(n^2)$   |

#### Property-Based Testing

In one of the previous section, we mentioned Property-Based Testing(PBT) and its relation to loop invariant. Now it's a good time to see it in action. On top of the complete implementation of Bubble Sort, we can insert method calls that check the properties of the loop at runtime, in this case the loop invariant. You can modify the code to see the output to console and you can also try other test cases.

Indeed, we can also insert method calls that check whether the loop variant decreases each iteration. In Bubble Sort it's too straightforward, but in binary search it would be insightful.

```java
def bubble_sort(arr):
    n = len(arr)
    # Outer loop
    for i in range(n - 1):
        check_outer_invariant(arr, i)
        # Inner loop
        for j in range(n - 1, i, -1):
            check_inner_invariant(arr, i, j)
            if arr[j - 1] > arr[j]:
                swap(arr, j - 1, j)
        # after finishing inner loop, check invariant at j=i
        check_inner_invariant(arr, i, i)
    # final check
    check_outer_invariant(arr, n)


def swap(arr, i, j):
    arr[i], arr[j] = arr[j], arr[i]


def check_outer_invariant(arr, i):
    # invariant: arr[0:i] is sorted
    if not is_sorted(arr, 0, i - 1):
        print(f"Outer Loop Invariant Broken, i={i}")
        print("Current array:", arr)
        return False
    return True


def check_inner_invariant(arr, i, j):
    # invariant: arr[j] is the minimum in arr[j:n]
    if not is_minimum(arr, arr[j], j, len(arr) - 1):
        print(f"Inner Loop Invariant Broken, i={i}, j={j}")
        print("Current array:", arr)
        return False
    return True


def is_sorted(arr, low, high):
    if high < low:  # empty range is trivially sorted
        return True
    for k in range(low + 1, high + 1):
        if arr[k] < arr[k - 1]:
            return False
    return True


def is_minimum(arr, val, low, high):
    for k in range(low, high + 1):
        if arr[k] < val:
            return False
    return True


def bubble_sort_test():
    arr = [5, 2, 0, 3, 9, 1, 6, 3, 1, -2, 4]
    print("Input array:", arr)
    bubble_sort(arr)
    print("Sorted array:", arr)


bubble_sort_test()
```

#### Define Variables Wisely

- Earlier we noted that using **Counting-Based** definition for a loop control variable is often not ideal. Here is one such example.
- Suppose we want to implement Bubble Sort by swapping and selecting the maximum instead of the minimum as we did above, and we choose to use the number of iterations as the loop control variable $i$. We already know in the outer loop there will be $n$ iteration, so let's make $i$ go from $0$ to $n-1$.
- So in this case, the relative position of the unsorted subarray and sorted subarray is reversed: the unsorted subarray is on the left and the sorted subarray on the right.
- Now in the inner loop, we need to push the maximum of the unsorted subarray to its rightmost position. What is the range of the unsorted subarray? Its start index is fixed as $0$, but what about the end index?
- We have to setup equations to solve it. When $i=0$, the length of the unsorted subarray is $n$. As $i$ grows, the length of the unsorted subarray decreases, and their relation is $l=n-i$ (because the number of iterations is exactly have many elements in the unsorted subarray have been removed to the sorted subarray). Now suppose the end index of the unsorted subarray is $e$, we have $e-0+1=l=n-i$, then we have $e=n-i-1$.
- This is why using the number of iterations is not the best practice. This calculation is not straightforward and error-prone. You definitely want to avoid such complication in a coding interview. The corresponding code is as below. The part `j < len(arr) - 1 - i` will likely stun the interviewer and make he or she confused.

```python
def bubble_sort_max(arr):
    for i in range(len(arr)):
        for j in range(len(arr) - 1 - i):
            if arr[j + 1] < arr[j]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

- In fact, if we want to select the maximum instead of the minimum in BubbleSort, a better choice of the definition of $i$ is *the end index of the unsorted subarray*, which uses the **Index-Based** definition. In this case we can iterate from $n-1$ to $0$, leading to a much cleaner and easier code.
- We better define the loop index as something directly connected to what changes from iteration to iteration

```python
def bubble_sort_max(arr):
    for i in range(len(arr) - 1, -1, -1):
        for j in range(i):
            if arr[j + 1] < arr[j]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

You are encouraged to implement Bubble Sort using other Loop Control Variable definition, for example, the Variant-Based definition and you will gain more insight into how the choice of loop variable can impact the clarity and difficulty of the implementation.

#### Optimization

How to optimize Bubble Sort? 
- Recall that we partition the entire array into a sorted subarray and an unsorted subarray. As part of the outer loop invariant, we are certain that the sorted subarray is indeed sorted. 
- However, it’s possible that at some point during execution, the unsorted portion becomes sorted as well, because we have done work to fix the pairs in the wrong order, or perhaps it's originally partially sorted. In our graphical example, the whole array is already sorted when $i=3$, but continued until until $i=5$.
- Recall the second part of the outer loop invariant: max($arr[0:i]$)<=min($arr[i:n]$). This implies that if the unsorted subarray is sorted, then the entire array is sorted, and we can safely terminate the algorithm without executing the rest of the loop, which will swap no elements in the unsorted subarray anymore.
- So the key to optimization falls upon whether we can easily identify that the unsorted subarray is already sorted. If you take a look at the method `is_sorted()` in the Property-Based Testing section, you will likely notice it's strikingly similar to the inner loop of Bubble Sort: *it checks for any adjacent pair out of order. If none are found, the subarray is sorted*.
- Based on the above observation, we can introduce and keep track of an additional boolean variable `swapped` which acts as a flag of whether any swapping takes place in the inner loop, and if it's false after the inner loop terminates, according to our analysis, we can safely terminate the algorithm.
- Although on average and worst the time complexity is the same as the original version, in the best case where input array is already sorted, the complexity is $O(n)$. In practice it would run faster when the input array is partially sorted.

```python
def bubble_sort_optimized(a):
    for i in range(len(a)):
        swapped = False
        for j in range(len(a) - 1, i, -1):
            if a[j - 1] > a[j]:
                a[j], a[j - 1] = a[j - 1], a[j]
                swapped = True
        if not swapped:
            return
```

### Insertion Sort

#### High-Level Design

Insertion Sort is another classic sorting algorithm, and it shares some similarity with Bubble Sort. We will compare them from time to time. For the following discussions, assume we want to sort an array in non-decreasing order. 

*Note:* This section is not a comprehensive treatment of sorting algorithms or even Insertion Sort specifically. We are only focusing on how to methodically implement it.

**High-Level Design Perspective**:
- Insertion Sort is a comparison-based sorting algorithm that builds the sorted array one element at a time. The algorithm works by inserting each new element into an already sorted subarray, whose initial size is $1$, until the whole array is sorted.

**Definition of Correct Insert Position**:
- Determining where to insert a new element in the sorted subarray is key. How to define the correct insert position for an element?
- If the array contains duplicate values, there may be multiple valid insert positions for a given element, because the order between equal elements doesn't affect sortedness.
- The choice we pick is the position that is not only correct, but also requires minimum number of data movement. This makes the desired insert position for an element in an already sorted subarray unique if it could only grow to one direction (e.g. left or right).
- In the following discussion, we use the term **entry** to refer to the element each time we need to insert into a sorted subarray.

<img src="img/best_insert_position.png" alt="best_insert_position.png" style="width: 50%; height: 50%; display: block; margin-left: auto; margin-right: auto;">

**High-Level Procedure**:
- Start from the second element of the array and treat the first element as a trivially sorted portion.
- For each remaining element, insert it into the sorted portion:
    - Compare it with the elements to its left.
    - Shift all elements greater than it one position to the right to make room.
    - Insert the entry into its correct position in the sorted portion.
- Repeat until all elements have been inserted into their correct positions.
- According to our analysis of the correct insert position, it should points to the first element larger than the entry.

**Analogy**: sorting playing cards
- A classic analogy of Insertion Sort is how you might sort playing cards in your hand. 
- You take the cards one by one from a pile and insert each card into the correct position among those already sorted in your hand.
- Before and after you insert the new card in your hand, the cards are sorted. You repeat this for each new card drawn.

<img src="img/insertionsort_playing_cards.png" alt="insertionsort_playing_cards.png" style="width: 30%; height: 30%; display: block; margin-left: auto; margin-right: auto;">

**Variants of Insertion Sort**:
- There many variations of Insertion Sort, and each version could be implemented in many ways. So don't be surprised when you see an unfamiliar implementation that still accomplishes insertion sort.
- The version we describe here builds the sorted subarray from left to right. Conversely, one could implement it from right to left, and the whole process is a mirror version of the above steps. Notice that in this reversed direction, the best insert position points to the first element smaller than the entry.
- Shifting all elements *greater than or equal to* the entry is also correct but introduce redundant data movement. In addition, doing so breaks the **stability** of the sorting algorithm (equal elements could get out of their original order), so people generally don't consider this option. 

The below is a graphical representation of the procedure. The grey portion is the sorted subarray involved in current insertion.

<img src="img/insertionsort_visualization.png" alt="insertionsort_visualization.png" style="width: 30%; height: 40%; display: block; margin-left: auto; margin-right: auto;">

Before continuing, please implement Insertion Sort by yourself according to the algorithm steps above. In addition, annotate your loop structure and explain what invariants you’re trying to maintain. From this case study, we will skip verbose details of the thought process of implementing the algorithms, but focus on key points.

#### Mid-Level Design

**Applying Repetition**:
- Insertion Sort's high-level description heavily suggests looping/iteration, just like Bubble Sort.
- We need to perform the insertion operations for each element in the array. Repetition of the same process for each element suggests using loop. 
- By repeating this process, the sorted part of the array grows and the unsorted part of the array shrinks. Eventually the sorted part will be the whole array and the unsorted part will be an empty array. 
- The insertion operation itself requires comparing they entry to potentially multiple existing elements in the subarray, and shifting those elements to the right, which is also repetitive and requires loop.
- This analysis concludes that we need **two layers of loops**: for each element in the array, and for each insertion into a subarray.

Insertion Sort conceptually divides the array into a sorted subarray followed by an unsorted subarray. The main difference between Insertion Sort and Bubble Sort is that the operation of insertion mainly involves the sorted subarray, as we only access one element in the unsorted subarray at a time, leaving the rest untouched. Whereas Bubble Sort's primary operations happen within the unsorted subarray.

We are pretty close to the mid-level design of the algorithm: 
- Two nested loops, and the outer loop simply moves the boundary between the sorted and unsorted subarrays, and effectively, it picks the next element to insert. 
- The only lingering question about the design is how exactly do we insert the entry into a sorted subarray in the inner loop? 

**Element Insertion**:
- Unlike "exchange two adjacent elements if they are in the wrong order", the insertion operation could be implemented in multiple ways. Although all of them achieve the same goal, there are distinction in terms of efficiency:
- **Adjacent Swapping**: We could reuse the swapping operation defined in the Bubble Sort to exchange the entry with elements in the sorted subarray, starting from the last/rightmost one, if they are in the wrong order, i.e. left element > right element. The exchange continues until the entry is in the correct position, and at this point the sorted subarray maintains sorted with one more element.
- **Search while Shifting**: Instead of swapping the entry all the way to the correct position, we can adopt an shifting approach. We start from the end of the sorted subarray, and for each element larger than the entry, shift them one position to the right (overriding that position with the value of the element) until we reach the insert position. In the end, we override the value at the insert position with the value of the entry.
- **Search then Shifting**: We can decouple the operations of array element shifting and comparison by first comparing the entry with the elements in the sorted subarray, starting from the last/rightmost one (you can also do this in the opposite direction), until we find the insert position. And then we shift all elements in the range starting from the insert position to the end of the sorted subarray one position to the right. As a final step, we override the value at the insert position with the value of the entry.
- Other approaches of insertion exist too, but the above are the primary patterns.

**Search while Shifting** and **Search then Shifting** are very similar in nature. For the rest of the section, we will go with **Search while Shifting**, but here we also mention how to implement **Search then Shifting** for completeness. 

**Mid-Level Design** of Insertion Sort:
- Insertion Sort partitions the entire array into two sections, a sorted subarray followed by an unsorted subarray, and it iteratively increase the size of the former while decreasing that of the latter. It has two layers of loops.
- In the outer loop, it iterates through all elements in the array, picking the next element to insert.
    - This makes the size of the unsorted subarray decrease by 1, and the size of the sorted subarray increase by 1 for each iteration.
    - Depending on whether we choose to decouple element comparison and shifting, the loop body will differ.
- **Search while Shifting**: if we don't decouple comparison and shifting, then we have one inner loop.
    - The inner loop iterates through elements in the sorted subarray from right to left, compare them with the entry and shift the old element one position to the right if it's greater than the entry. The process continues until an element no less than the entry is found or all elements are greater than it. Finally we insert the entry to its correct insert position.
- **Search then Shifting**: if we decouple comparison and shifting, then we have two inner loops.
    - In the first inner loop (comparison loop), it iterates through elements in the sorted subarray from right to left, until an element no less than the entry is found or all elements are greater than it. This loop determines the insertion index.
    - In the second inner loop (shifting loop), we shift all elements in the subarray that are greater than the entry one position to their right, and we perform the shifting from right to left to avoid data loss. Finally we insert the entry to its correct insert position.

```python
# Search while Shifting
for each element:
    for comparison and movement:
        ...
```
OR
```python
# Search then Shifting
for each element:
    for comparison:
        ...
    for movement:
        ...
```

This design leverages the incremental nature of the algorithm: each new element is placed into an already sorted context, preserving sorted order as we go. Next we translate the mid-level design into low-level code.

#### Outer Loop Implementation

<img src="img/insertionsort_outer_loop.png" alt="insertionsort_outer_loop.png" style="width: 30%; height: 60%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the outer loop does conceptually.
- It's strikingly similar to Bubble Sort in terms of how sorted and unsorted subarrays change from iteration to iteration. 
    - The difference is that in Bubble Sort, the work (swapping) is done within the unsorted subarray, whereas in Insertion Sort, it's done on the sorted subarray. 
    - In both algorithms,  each iteration takes one element from the unsorted subarray and incorporates it into the sorted subarray.
- We don't go into lengthy discussion like in Bubble Sort anymore. For example, Loop Goal, Premise and Edge Cases are almost identical.
- The outer loop usually starts from i = 1. The reason is that with $i=1$ as the initial value, we treat $arr[0]$ as initially sorted and begin inserting from $arr[1]$.
- It should be evident that a `for` loop is suitable for the outer loop, and defining $i$ as the start index of the current unsorted subarray, which is equivalent to the index of the element we want to insert, is a good choice.

In the following discussion, we will use $n$ to replace $len(arr)$ for brevity.

**Loop Elements Analysis** of the outer loop:
- **Loop Goal**: sort $arr[0:n]$ in non-decreasing order.
- **Loop Premise**: $arr$ is not `None`.
- **Loop Initialization**: initialize $i$ to $1$ in the `for` loop header.
  - In fact both $0$ and $1$ work. We choose $1$ because a subarray of only one element is trivially sorted, so we don't have to insert into an empty sorted subarray. Thus the first element we need to insert is $arr[1]$.
  - If you are uncertain about this, and you can just use either value temporarily here and come back to adjust it after you finish the inner loop.
- **Loop Condition**: the loop continues as long as the unsorted subarray is not empty. 
  - Combined with loop termination and edge case analysis, we can use $i<n$ as loop condition.
- **Loop Termination**: the loop terminates when unsorted subarray is empty. 
  - So after the loop terminates, $i=n$, and the whole array is sorted.
- **Iteration Goal**: insert $arr[i]$ into $arr[0:i]$, making $arr[0:i]$ sorted.
- **Edge Cases**: $n=0$ or $n=1$
  - Using the loop condition we picked and the initialization choice, no iteration is required and correctness is guaranteed. This also means we don't need additional code to specifically handle them.
- **Loop Variant**: the size of the unsorted subarray, which is $n-i$. 
  - we decrease it by 1 for each iteration, and in Python the for loop automatically increments $i$.
- **Loop Invariant**: at the start of the i-th iteration, $arr[:i]$ is sorted in non-decreasing order.
  - Notice that unlike Bubble Sort, we have no information between the elements in the sorted subarray and the elements in the unsorted array.
  - **Initialization**: the invariant is initially true for the first iteration: $arr[0:1]$ is trivially sorted.
  - **Maintenance**: if the iteration goal is accomplished, the sorted order is maintained, while sorted subarray grows by 1, then we maintain the invariant.
  - **Termination**: after the loop terminates, $i$ would be $n$, which means $arr[0:n]$, the whole array is sorted and the loop goal is achieved.
- **Loop Control Variable**: $i$ is the loop control variable acting as the partition boundary.
  - It points to the element we need to insert for the current iteration, and it's the start index of the unsorted subarray that we handle in i-th iteration, while the end index is always $n-1$.
  - So at the star of an iteration, the sorted subarray is $arr[:i]$, while the unsorted subarray is $arr[i:n]$.
  - Its initial value is 1, which corresponds to a sorted subarray of only $1$ element, and we increment it by $1$ for each iteration. 
  - The loop continues until $i$ reaches $n$, ensuring that the value of $i$ covers the range of from $1$ to $n-1$, both end inclusively, making sure each iteration we insert only one element from unsorted subarray to sorted subarray.

According to the above analysis, we have the framework of the outer loop:
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        # inner loop
        pass
```

#### Inner Loop Implementation

<img src="img/insertionsort_inner_loop.png" alt="insertionsort_inner_loop.png" style="width: 50%; height: 70%; display: block; margin-left: auto; margin-right: auto;">

**Implementation Design**
- The above image is a graphical visualization of what the inner loop does. After the mid-level design, the inner loop is conceptually clear, and we need to do two things at the same time:
    - Locating the correct insert position for the entry, which is an array index.
    - Shifting all larger elements one position to the right, making room for the insertion.
    - It's like the insert position is a gap between the original sorted subarray, and the entry fills that gap.
- Now we just need to define appropriate variables and translate the procedure into code.
    - Most of the loop elements are straightforward, so we only discuss a few important points.
    - The entry we need to insert is $arr[i]$, and the start and end indices of the sorted subarray is clear: $arr[0:i]$. 
    - It makes sense to use a loop control variable, say $j$ to scan the subarray, which means we use it to point to each element in the subarray to compare it with $arr[i]$.

- **Insert Position Cases**: according to our previous discussion of the insert position, since we partition the whole array as the sorted subarray followed by the unsorted subarray, so the sorted subarray could only grow to the left. We can analyze the possibilities of insert position:
    - **Best Case**: $arr[i]$ is already at the correct position, in which case $arr[i]$ >= max($arr[0:i]$), no data movement is required.
    - **Worst Case**: $arr[i]$ needs to be inserted at the start of the subarray, in which case $arr[i]$ < min($arr[0:i]$), maximum data movement as all elements in the subarrays needs to be shifted. Notice that if there are elements equal to $arr[i]$, it's impossible to be the worst case, because we can append $arr[i]$ to the end of the elements equal to it.
    - **Common Case**: $arr[i]$ needs to be inserted into somewhere middle of the subarray with some data movement, which means some of the sorted elements greater than $arr[i]$ will be shifted right, but not all of them.
- **Edge Cases**: Both best case and worst case are edge cases.
    - Because the insert positions are the two ends of the subarray, where things could easily go awry. 
    - As with before, it's a good practice to recognize the edge cases, but don't rush to write code for them, instead let's focus on the common cases first.
- Below is an image summarizing the three cases.

<img src="img/insertionsort_insert_position.png" alt="insertionsort_insert_position.png" style="width: 50%; height: 60%; display: block; margin-left: auto; margin-right: auto;">

- **Temporary Variable**: You should be able to notice that except in the best case, $arr[i]$ will always be overridden in the loop during shifting.
    - But we need to access the value of $arr[i]$ during the loop for comparison. In addition, after the loop terminates, we need to insert it into the correct position, so we can't afford to lose the data store at $arr[i]$. 
    - The standard solution is to use a temporary variable, say $entry$ to store $arr[i]$ during the loop initialization. Then we use $entry$ for all comparisons, and when we find the insertion spot, we write $entry$ into that position.
    - By doing this, we ensure the original value is not lost during shifting.
- **Iteration Direction**: Which direction should we scan the sorted subarray for the insertion point? In fact both iteration direction work. But right-to-left is preferred for our approach.
    - The left-to-right direction starting from the first element of the subarray is only suitable for the **Search then Shifting** approach. 
    - For example, in the common case of the above image, if we scan from left to right, both $arr[0]$ and $arr[1]$ involve in comparison but not shifting, so this kind of drifts from our choice of **Search while Shifting**. 
    - For **Search while Shifting**, we should scan from left to right, starting from the last element in the sorted subarray. In this case, every element that is larger than th entry will be compared and shifted.
    - There are some uncertainties about the insert position for a particular element, so we cannot say right-to-left is definitely better than left-to-right when we use **Search then Shifting**, at least in terms of the number of comparison.
- **Definition and Range of $j$**: it's evident that the good definition of $j$ is to the array index of elements to be compared.
    - Like in Bubble Sort, there are other choices, but you should be able to see why use $j$ directly point to the elements to be compared with the entry is the go-to choice. 
    - In addition, doing so makes $j$ directly related to the insert position we want to locate.
    - This means the range of $j$ is from $i-1$ down to $0$ (inclusive), or equivalently $range(i-1, -1, -1)$ in Python. Although we don't necessarily need compare all $arr[0:i]$ with $entry$: as soon as we find an element no greater than $entry$ the loop could stop, which suggests another part of the loop condition besides checking the range of $j$.
    - Let's take a look the above three cases:
        - For the **Best Case**, $j$ will end as $i-1$, and the insert position is $i=j+1$, so the entry stays in place.
        - For the **Worst Case**, since $arr[0]$ is the last element to be compared with $entry$, after that comparison $j$ will be $-1$. This is ok, because the insert position is $0$, which is also $0=j+1$.
        - For the **Common Case**, $j$ will points to an element no greater than $entry$, and in this case, the loop should terminate, and $entry$ should be inserted at $j+1$.
    - We can see in all three possible cases, after the loop terminates $j+1$ is the correct insert position. So using $j$ in this way is sound.
- **Loop Structure Choice**: Do we use `for` loop or `while` loop? 
    - Since `for` loop and `while` loop could be converted to each other, the choice of the loop structure is not so important. 
    - What is important is to notice that, we need to access the value of $j$ to insert $entry$ after the loop terminates, which means in Python, we need $j$ to be accessible after the loop, so we shouldn't overwrite it inside a $for$ loop that hides its value, which makes $while$ loop a more suitable choice.
    - In addition, we also need to initialize $entry$ outside of the loop and access it after the loop. These reasons suggests `while` loop will be more clear and easy to read.

**Loop Elements Analysis**. After the above analysis, we have figured out the loop elements of the inner loop.
- **Loop Goal**: insert the element $arr[i]$ into the sorted subarray $arr[0:i]$ so that after insertion, $arr[0:i+1]$ is sorted.
- **Loop Premise**: the $arr[0:i]$ is sorted.
    - This is guaranteed by the outer loop invariant. So we can use it as given knowledge throughout the analysis.
- **Loop Initialization**: initialize $j$ as $i-1$ and $entry$ as $arr[i]$ before the main loop.
- **Loop Condition**: the loop continues as long as there are more elements to compare, and we haven't found an element no greater than $entry$. 
    - Combined with loop termination and edge case analysis, we see that $j \ge 0$ `and` $arr[j]>entry$ is a proper expression for loop condition.
    - It checks two things: that we haven't run off the left end of the array ($j \ge 0$ ensures we still have elements to check on the left side) and that the entry $arr[j]$ is greater than the key (meaning the key should be inserted to the left of $arr[j]$).
    - As long as both conditions hold, the loop continues. This condition embodies the idea "while we haven't reached the beginning and $arr[j]$ is still greater than $entry$, keep shifting." 
- **Loop Termination**: we terminate the loop when there are no more elements to compare or we find an element no greater than $entry$.
    - For the first case, it is the worst case, and $j=-1$. $entry$ should be inserted right at $arr[0]$.
    - For the second case, we know that $arr[j] \le entry$. $entry$ should be inserted right at $arr[j+1]$.
    - Either case, after the loop terminates, all elements originally in $arr[j+1:i]$ before the shifting are larger than $entry$, and they have been already shifted to the correct position, which occupies $arr[j+2:i]$. 
    - And according to the above analysis, $j+1$ is the correct insert position. Thus inserting $entry$ to $arr[j+1]$ will make $arr[0:i+1]$ sorted. The inner loop’s job is done, and control returns to the outer loop to process the next element.
- **Iteration Goal**: shift $arr[j]$ one position to the right if it's greater than $entry$.
    - Inside the loop body, we already know that $arr[j]>entry$. Then we know that $j+1$, the temporary gap, is not the insert position for $entry$. 
    - So by moving a larger element $arr[j]$ out of the way to $arr[j+1]$ and decrementing $j$, we continue to consider the next position as a potential candidate for insertion.
- **Edge Cases**: both best case and worst case are edge cases, and we have analyzed them in the previous discussion.
    - Even in edge cases, $j+1$ is still the correct insert position and all involved elements will be correctly shifted, so no additional code is required to handle edge cases specifically.
- **Loop Variant**: the number of unexamined elements in the sorted subarray, which is $j+1$. 
    - We decrement $j$ using $j -= 1$ in each iteration, ensuring the loop progresses.
    - With each iteration, $j$ moves closer to the beginning of the array, toward $-1$, guaranteeing that the loop will eventually end. Even in the worst case, the loop finishes after $i$ comparisons/shifts.
- **Loop Invariant**: at the start of the `j`-th iteration, $entry$ < min($arr[j+2:i+1]$) — noting that this subarray now contains the shifted elements that originally came from $arr[j+1:i]$; both $arr[0:j+1]$ and $arr[j+2:i+1]$ are sorted, and their concatenation is the original sorted subarray $arr[0:i]$.
    - Since we are shifting the elements in the loop, we need to note that elements in $arr[j+2:i+1]$ during loop, are actually the elements originally in $arr[j+1:i]$ before the loop, but are shifted one position to the right.
    - Another way to understand this invariant is that except for the yet-to-be-placed $entry$ and the one-position gap we create by shifting, the order of the other elements is still sorted, exactly as before the loop runs.
    - **Initialization**: the invariant is initially true for the first iteration, which is when $j=i-1$ : $arr[j+2:i+1]$ is actually $arr[i+1:i+1]$, which is an empty subarray, and $arr[0:i]$ is sorted.
    - **Maintenance**: during the loop execution, we know that $arr[j]>entry$ according to the loop condition. The loop body will shift $arr[j]$ one position to the right, which is $arr[j+1]$, making $arr[j+1:i+1]$ sorted, which are the original element of $arr[j:i]$ in the same order before the loop. This preserves the order property for indices beyond $j$ because the element that was at $arr[j]$ was greater than $entry$ and also greater than all elements to its left. At the end of the iteration, $j -= 1$ executes and we maintain the invariant.
    - **Termination**: After the loop terminates, $j$ could be $-1$ (in the worst case), or $i-1$ (in the best case), or between them. No matter which case, inserting $entry$ to $arr[j+1]$ would make $arr[0:i+1]$ sorted. 
- **Loop Control Variable**: $j$ is the loop control variable. 
    - We use it to scan elements in the sorted subarray from right to left, and for the `j`-th iteration it points to the element $arr[j]$. 
    - Its initial value is $i-1$, which corresponds to the element immediately to the left of $arr[i]$ in the sorted subarray.
    - The loop moves $j$ leftwards decreasing it by $1$ for each iteration, as long as the element at $arr[j]$ is greater than $entry$.
    - The loop continues until $j$ reaches $-1$ or exit when $arr[j] \le entry$. This ensures that the value of $j$ covers the range from $0$ to $i-1$ if no such element is found, making sure we examine all elements and find the correct insert position.

Now we have the framework of the inner loop as well:

```python
def insertion_sort(a):
    for i in range(1, len(a)):
        j = i - 1  # initialization of j, the first element to the left of a[i]
        entry = a[i]  # initialization, saving a[i] to a temporary variable
        while j >= 0 and a[j] > entry:
            a[j + 1] = a[j]  # shift a[j] one position to the right
            j -= 1  # loop control variable update, moving to next element
        a[j + 1] = entry  # insert entry into the correct position pointed by j+1
```

#### Complete Implementation

Based on our design, below is the complete implementation of Insertion Sort.

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        j = i - 1
        entry = arr[i]
        while j >= 0 and arr[j] > entry:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = entry
```

- If there are many equal elements, the inner loop will still shift elements that are equal to the key? 
    - Actually, note the condition uses $arr[j] > entry$ (strictly greater than). That means if an element is equal to the key, the loop stops.
    - Insertion Sort as typically implemented above is **stable**, that is it preserves the order of equal elements, because it does not change the relative order of equal elements.
    - As an edge consideration, this ensures that the relative order of equal keys remains the same after sorting.
- Finally, it’s worth noting how the inner and outer loops interact logically:
    - The outer loop sets up the scenario by identifying which element to insert next and maintaining that all prior elements are sorted
    - The inner loop performs the insertion by rearranging the sorted portion to accommodate the entry. 
    - Each outer iteration uses the inner loop to ensure that the outer loop invariant continues to hold true for an expanded sorted section.
    - Each inner iteration assumes the previously sorted subarray guaranteed by the outer loop.
    - In combination, these loops systematically build up a sorted array, element by element.

Table for Loop Element Summary
| Element                 | Outer Loop (i)                   | Inner Loop (j)                                                            |
| ----------------------- | -------------------------------- | ------------------------------------------------------------------------- |
| **Goal**                | Sort the array                   | Insert $arr[i]$ into the sorted subarray                                  |
| **Invariant**           | $arr[0:i]$ is sorted             | $entry < \min(arr[j+2:i+1])$ ; $arr[0:j+1]$ and $arr[j+2:i+1]$ are sorted |
| **Variant?**            | $n - i$ (size of unsorted)       | $j+1$ (number of remaining elements)                                      |
| **Control Variable**    | $i$ (start of unsorted subarray) | $j$ (index of remaining element)                                          |

Below is the annotated implementation of Insertion Sort that includes documentation that describe the purpose and properties of each loop (loop goal, invariant, variant, etc.), aligning with the analysis above. The comments in the code help to bridge the gap between the algorithm’s formal reasoning and its implementation.

```python
def insertion_sort(arr):
    # Loop Goal: sort the entire array arr in non-decreasing order
    # Loop Control Variable i: index of the current element to insert into the sorted subarray arr[0:i]
    # Loop Range: i from 1 to len(arr) - 1; arr[0] is trivially sorted
    # Loop Invariant: arr[0:i] is sorted at the start of each iteration
    # Iteration Goal: insert arr[i] into arr[0:i], making arr[0:i+1] sorted
    # Edge Cases: if len(arr) == 0 or 1, the loop body will not execute
    for i in range(1, len(arr)):
        # Loop Goal: insert arr[i] into the correct position in arr[0:i]
        # Loop Control Variable j: current index being compared with entry
        # Loop Range: j starts from i - 1 and decreases toward 0
        # Loop Invariant: at the start of each j-th iteration,
        #   - entry < min(arr[j+2:i+1])
        #   - arr[0:j+1] and arr[j+2:i+1] (shifted elements) are sorted
        # Iteration Goal: shift arr[j] to the right if it's greater than entry
        # Edge Cases:
        #   - if entry < min(arr[0:i]), j becomes -1 (insert at position 0)
        #   - if entry >= max(arr[0:i]), j stays at i - 1 (insert at position i)
        j = i - 1
        entry = arr[i]
        while j >= 0 and arr[j] > entry:
            arr[j + 1] = arr[j]
            j -= 1
        # Loop Termination: all greater elements have been shifted;
        # j + 1 is the correct position to insert entry
        arr[j + 1] = entry
```

Brief complexity analysis:
- Insertion Sort is in-place, with constant extra space, so the space complexity is O(1).
- Due to the two layers of loop, the time complexity is $O(n^2)$. The following table summarizes the complexity of comparisons and shifts.
- **Best Case**: If the input array is already sorted, the outer loop will still run $n-1$ times, but the inner loop will do minimal work : each time it finds that the entry is larger than or equal to the last sorted element, so no shifts occur. 
- **Worst Case**: If the input array is reverse sorted, the outer loop still runs $n-1$ times, but each iteration will trigger the inner loop to run all the way to the beginning of the sorted portion, making the algorithm do the maximum number of shifts, which is $O(n)$.

| Metric      | Best Case    | Average Case | Worst Case |
| ----------- | ------------ | ------------ | ---------- |
| Comparisons | $O(n)$       | $O(n^2)$     | $O(n^2)$   |
| Shifts      | $O(1)$       | $O(n^2)$     | $O(n^2)$   |
| Overall     | $O(n)$       | $O(n^2)$     | $O(n^2)$   |

#### Alternative Implementation and Optimization

What we have discussed and implemented so far is the **Search while Shifting** approach of insertion. In this section we take a brief look at the other approaches, but we don't delve into the analysis details of them. 

##### Alternative Search while Shifting

In the original implementation, the core of the inner loop uses the following two lines:
```python
arr[j + 1] = arr[j]
j -= 1
```
In Java or C-like languages, some developers might try to compress this into one line using post-decrement operations (e.g., $arr[j + 1] = arr[j--])$, but Python does not support such syntax. Python deliberately avoids such side-effect expressions to improve code clarity.

##### Adjacent Swapping

```python
def insertion_sort_adjacent_swapping(arr):
    for i in range(1, len(arr)):
        for j in range(i, 0, -1):
            if arr[j] < arr[j - 1]:
                arr[j], arr[j - 1] = arr[j - 1], arr[j]  # Swap using tuple unpacking
            else:
                break
```

- Adjacent Swapping Insertion Sort looks similar to Bubble Sort.
- While easier to think about, swapping can result in three assignments per step (per swap) and tends to be slower in practice than the shifting approaches, which uses one assignment per step.
- The overall complexity are on the same level, but this approach is typically avoided in professional code.

##### Linear Search then Shifting

```python
def insertion_sort_search_then_shifting(a):
    for i in range(1, len(a)):
        j = i - 1
        entry = a[i]
        while j >= 0 and entry < a[j]:
            j -= 1
        for k in range(i - 1, j, -1):
            a[k + 1] = a[k]
        a[j + 1] = entry
```

- Search then Shifting is similar to Search while Shifting but decouples the search/comparison and shifting processes, so you could scan from left to right.
- Using two loops that scan the same range of values $[j+1:i]$ seems wasteful, but this can be optimized using slicing in Python, which is recommended in performance-sensitive scenarios.

```python
def insertion_sort_search_then_shifting_optimized(a):
    for i in range(1, len(a)):
        j = i - 1
        entry = a[i]
        while j >= 0 and entry < a[j]:
            j -= 1
        if j != i - 1:
            a[j + 2:i + 1] = a[j + 1:i]
            a[j + 1] = entry
```

- The shifting here can be implemented as bulk array copy operation, which is more efficient when the size of the elements shifted is large.
- You may feel the line using slicing is not very readable. The portion we need to shift is $a[j+1:i]$, and its length is $i - (j + 1)$, which is $i - j - 1$.

##### Binary Search then Shifting

If you are acute about searching for insert position in a sorted subarray or if you have done this particular leetcode problem: https://leetcode.com/problems/search-insert-position/description/ .

Then you might have notice that if we decouple searching and shifting, we can actually use binary search to find the insert position, which is $O(\log n)$, much better than the $O(n)$ average case or worst case.

```python
def insertion_sort_binary_search(a):
    for i in range(1, len(a)):
        entry = a[i]
        k = binary_search_insert_position(a, 0, i - 1, entry)
        if k != i:
            a[k + 1:i + 1] = a[k:i]
            a[k] = entry

def binary_search_insert_position(array, low, high, key):
    if key >= array[high]:
        return high + 1
    while low < high:
        mid = low + (high - low) // 2
        if key < array[mid]:
            high = mid
        else:
            low = mid + 1
    return low
```

- We will have a dedicated notebook for binary search, so we don't argue why it's correct here.
- This is a classic optimization of insertion sort for the search phase, reducing it from $O(n)$ to $O(\log n)$. 
- But the algorithm still has $O(n^2)$ overall time complexity due to shifting.
- Notice that this optimization doesn't always bring performance gain, because the original algorithm only requires $O(1)$ in the best case. 
- In fact this optimization is rarely worth it in practice unless:
    - comparisons are much more expensive than moves
    - the array is large and the data is not primitive (e.g., sorting large objects)

### Three-Way Partitioning

TODO

## Recommended Leetcode Problems

Now it's time to test your understanding and mastery of writing looping code. The following leetcode problems are the ones that involve relatively complicated loop statements. Try them and see if you can solve them in a methodical way.

Easy
- [26. Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
- [27. Remove Element](https://leetcode.com/problems/remove-element/)
- [67. Add Binary](https://leetcode.com/problems/add-binary/)
- [228. Summary Ranges](https://leetcode.com/problems/summary-ranges/)
- [925. Long Pressed Name](https://leetcode.com/problems/long-pressed-name/)
- [2200. Find All K-Distant Indices in an Array](https://leetcode.com/problems/find-all-k-distant-indices-in-an-array/)

Medium
- [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- [31. Next Permutation](https://leetcode.com/problems/next-permutation/)
- [38. Count and Say](https://leetcode.com/problems/count-and-say/)
- [43. Multiply Strings](https://leetcode.com/problems/multiply-strings/)
- [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
- [80. Remove Duplicates from Sorted Array II](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
- [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)
- [159. Longest Substring with At Most Two Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-two-distinct-characters/)
- [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum/)
- [340. Longest Substring with At Most K Distinct Characters](https://leetcode.com/problems/longest-substring-with-at-most-k-distinct-characters/)
- [395. Longest Substring with At Least K Repeating Characters](https://leetcode.com/problems/longest-substring-with-at-least-k-repeating-characters/)
- [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
- [438. Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/)
- [443. String Compression](https://leetcode.com/problems/string-compression/)
- [487. Max Consecutive Ones II](https://leetcode.com/problems/max-consecutive-ones-ii/)
- [567. Permutation in String](https://leetcode.com/problems/permutation-in-string/)
- [713. Subarray Product Less Than K](https://leetcode.com/problems/subarray-product-less-than-k/)
- [926. Flip String to Monotone Increasing](https://leetcode.com/problems/flip-string-to-monotone-increasing/)
- [1004. Max Consecutive Ones III](https://leetcode.com/problems/max-consecutive-ones-iii/)
- [1493. Longest Subarray of 1's After Deleting One Element](https://leetcode.com/problems/longest-subarray-of-1s-after-deleting-one-element/)
- [1838. Frequency of the Most Frequent Element](https://leetcode.com/problems/frequency-of-the-most-frequent-element/)
- [2348. Number of Zero-Filled Subarrays](https://leetcode.com/problems/number-of-zero-filled-subarrays/)

Hard
- [41. First Missing Positive](https://leetcode.com/problems/first-missing-positive/)
- [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)

# Recursion

Recursion is when a function calls itself to solve a smaller piece of a larger problem.

Recursion is another form of repetition, but in all of the previous notes, we never discuss when to use loop vs. recursion.

Almost every loop element has a recursive corresponding counter part. Since recursion itself is closely related to algorithms (in fact you rarely use recursion if you are not implementing an algorithm), it deserves its own treatment, a whole notebook dedicated to it.

# Documentation

## Block/Loop Documentation

Checklist for Block Documentation:

| **Element**                 | **Importance**                                             |
| --------------------------- | ---------------------------------------------------------- |
| **Purpose / Intent**        | What is this loop doing logically? (High-level goal)       |
| **Loop Invariant**          | What condition remains true before/after each iteration?   |
| **Key Variables & Roles**   | Clarify index meanings, accumulators, pointers             |
| **Exit Condition**          | When and why does this loop stop?                          |
| **Special Cases**           | Explain corner cases (e.g., early termination, edge cases) |
| **Time Complexity**         | What’s the cost of this loop?                              |
| **Side Effects (Optional)** | If this loop mutates external structures                   |


```python
# ------------------------------------------------------------------
# Loop Purpose:
#   - [What problem is this loop solving?]
#   - [What is the goal at a high level?]
#
# Loop Invariant:
#   - [What condition remains true before and after each iteration?]
#   - [How does this ensure correctness of the algorithm?]
#
# Loop Variant (Progress Measure):
#   - [What value decreases/increases to ensure termination?]
#   - [Why will the loop eventually stop?]
#
# Key Variables & Roles:
#   - [What do 'i', 'left', 'right', 'current', etc. represent?]
#   - [Clarify pointers, accumulators, state trackers, etc.]
#
# Exit Condition:
#   - [Under what condition does the loop terminate?]
#   - [Is there an early return?]
#
# Special Cases / Early Exit:
#   - [Any optimization or shortcuts inside the loop?]
#   - [Edge cases handled during iteration?]
#
# Side Effects:
#   - [Does this loop mutate any external structures?]
#   - [Does it affect global state, I/O, etc.?]
#
# Time Complexity:
#   - [What's the cost of this loop in Big O terms?]
# ------------------------------------------------------------------
```

## Function/Method Documentation

In Python, function documentation is typically written using triple-quoted strings (`"""..."""`) right after the function definition.  
Use the following checklist to write a clear and complete docstring:

Checklist for Method Documentation:
|**Element**        | **Importance**                     |
|------------------ | ---------------------------------- |
|**Description**    | Understand purpose                 |
|**Parameters**     | Know what to pass                  |
|**Return Value**   | Know what to expect back           |
|**Preconditions**  | Ensure correct usage               |
|**Postconditions** | Know what is guaranteed            |
|**Side Effects**   | Avoid unintended external changes  |
|**Exceptions**     | Handle errors properly             |
|**Complexity**     | Algorithm efficiency awareness     |

```python
def some_function(param1, param2):
    """
    [Brief one-line summary of what the method does.]

    Optional: Detailed description explaining the purpose of the method, how it works,
    and any important context about why it exists.

    Preconditions:
        - [List all assumptions or required states before calling this method.]
        - E.g., "The input array must be sorted."

    Postconditions:
        - [What guarantees are true after the method finishes.]
        - E.g., "Returns the index of target if found, array remains unchanged."

    Side Effects:
        - [Describe any external changes caused by the method.]
        - E.g., "Modifies the internal state of this object."

    Args:
        param1: [Description of param1, expected values, constraints.]
        param2: [Description of param2, expected values, constraints.]

    Returns:
        [Describe what is returned, meaning of special cases.]

    Raises:
        SomeException: [Under what condition is this thrown.]
        AnotherException: [Under what condition is this thrown.]

    Time Complexity:
        O(N log N) - [Describe in terms of input size.]

    Space Complexity:
        O(N) - [Optional but useful.]
    """
    pass
```

## Reference Materials

### Reference Materials

| **Resource** | **Link** |
|--------------|----------|
| PEP 257 – Docstring Conventions | https://peps.python.org/pep-0257/ |
| Python Official Docstrings Guide | https://docs.python.org/3/tutorial/controlflow.html#documentation-strings |
| Google Python Style Guide (Docstrings section) | https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings |
| NumPy/SciPy Docstring Standard | https://numpydoc.readthedocs.io/en/latest/format.html |
| Sphinx Documentation Style | https://www.sphinx-doc.org/en/master/usage/restructuredtext/domains.html#info-field-lists |

# Master Your Arsenal

## Built-In Library & Functions

Below is a list of built-in libraries and functions that are useful in competitive programming. Not all commonly used methods are listed—for example, methods like `append()` for lists or `add()` for sets are so ubiquitous that listing them here would seem redundant.

- Built-ins
    - a == b
      - equality comparison (equivalent to Objects.equals)
    - hash(obj)
      - returns hash code of an object (like Objects.hashCode)
    - hash((v1, v2, v3))               
      - combine multiple values into a tuple and hash it

- Arrays
    - sorted(a)
      - returns a new sorted list
    - sorted(a[from_index:to_index])
      - sort a slice and return new list
    - sorted(a)
      - works for any iterable (not just int[])
    - sorted(a[from_index:to_index])
      - works for object slices too
    - sorted(a, key=cmp_to_key(cmp))
      - sort with custom comparator
    - sorted(a[from_index:to_index], key=cmp_to_key(cmp))
      - partial sort with comparator
    - a == a2 
      - element-wise equality for lists
      - also works for lists of objects
    - all(cmp(x, y) == 0 for x, y in zip(a, a2)) 
      - equality with custom comparator
    - a[:] = [val] * len(a) 
      - fill entire list with value
    - a[from_index:to_index] = [val] * (to_index - from_index) 
      - fill slice
    - a[a], a[b] = a[b], a[a] 
      - swap two elements
    - a[:new_length] + [0] * (new_length - len(a)) 
      - pad or truncate list
    - a[:new_length] + [None] * (new_length - len(a)) 
      - for object arrays
    - a[from_index:to_index] 
      - copy a slice (inclusive-exclusive)
    - list(a) 
      - convert iterable to list (like asList)
    - hash(tuple(a)) 
      - hash code of list
      - also applies to list of objects
    - bisect.bisect_left(a, key, from_index, to_index)  
        - If `key` is not found, use `~idx` to get insertion point

- Collections
    - list.sort()
        - in-place sort of list
    - list.sort(key=cmp_to_key(cmp))      
        - in-place sort with custom comparator
    - list.reverse()                     
        - in-place reverse of list
    - list[:] = list[-distance:] + list[:-distance] 
        - rotate right if distance > 0
        - use list[distance:] + list[:distance] if distance < 0 for left rotation
    - set()                                
        - create empty set
    - list()                                 
        - create empty list
    - dict()                                   
        - create empty dictionary (map)
    - min(coll)                               
        - find min element in collection
    - max(coll)                               
        - find max element in collection

- List
    - list.sort(key=cmp_to_key(c))
        - in-place sort with custom comparator
    - list[fromIndex:toIndex]                   
        - sublist using slicing
    - list.index(o)                            
        - first index of o (raises ValueError if not found)
    - len(list) - 1 - list[::-1].index(o)          
        - last index of o (raises ValueError if not found)
    - list()                                        
        - equivalent to List.of() with 0 parameter
    - [e0], [e0, e1], ..., [e0, ..., e9]             
        - equivalent to List.of(...) with 1~10 elements

- set
    - set(iterable)
        - Create a set object from any iterable.
    - add(elem)
        - Add an element to the set.
    - remove(elem)
        - Remove a specific element from the set. Raises `KeyError` if not found.
    - discard(elem)
        - Remove element if present. No error if not.
    - pop()
        - Remove and return an arbitrary element from the set. Raises `KeyError` if empty.
    - clear()
        - Remove all elements from the set.
    - copy()
        - Return a shallow copy of the set.
    - union(*others)
        - Return new set with elements from all sets.
    - intersection(*others)
        - Return new set with only common elements.
    - difference(*others)
        - Return new set with elements in the set but not in others.
    - symmetric_difference(other)
        - Return new set with elements in either set but not both.
    - issubset(other)
        - Check if the set is a subset of another.
    - issuperset(other)
        - Check if the set is a superset of another.
    - isdisjoint(other)
        - Check if sets have no elements in common.
    - update(*others)
        - Add elements from all others (union in-place).
    - intersection_update(*others)
        - Keep only elements found in all others (intersection in-place).
    - difference_update(*others)
        - Remove all elements found in others (difference in-place).
    - symmetric_difference_update(other)
        - Keep only elements in either set but not both (XOR in-place).

- dict
    - keys()
        - Equivalent to Java's keySet()
    - items()
        - Equivalent to Java's entrySet()
    - get(key, default)
        - Equivalent to getOrDefault(Object key, V defaultValue)
    - setdefault(key, value)
        - Equivalent to putIfAbsent(K key, V value)
    - setdefault(key, mapping_function(key))
        - Equivalent to computeIfAbsent(K key, Function<? super K, ? extends V>)
    - list(my_dict.items()), list(my_dict.keys()), list(my_dict.values())
        - Equivalent to toArray(T[] a)
    - (k, v) tuple
        - Equivalent to entry(K k, V v)

- collections.deque
    - appendleft(e)
        - Equivalent to addFirst(E e), offerFirst(E e)
    - append(e)
        - Equivalent to addLast(E e), offerLast(E e)
    - popleft()
        - Equivalent to removeFirst()
        - throws IndexError when empty
    - pop()
        - Equivalent to removeLast()
        - throws IndexError when empty
    - (x.popleft() if len(x) > 0 else None)
        - Equivalent to pollFirst()
        - returns None when empty
    - (x.pop() if len(x) > 0 else None)
        - Equivalent to pollLast()
        - returns None when empty
    - x[0]
        - Equivalent to getFirst()
        - throws IndexError when empty
    - x[-1]
        - Equivalent to getLast()
        - throws IndexError when empty
    - (x[0] if len(x) > 0 else None)
        - Equivalent to peekFirst()
        - returns None when empty
    - (x[-1] if len(x) > 0 else None)
        - Equivalent to peekLast()
        - returns None when empty

- heapq (with list as min-heap)
    - heappush(heap, e)
        - Equivalent to add(E e)
    - heappop(heap) if heap else None
        - Equivalent to poll()
        - returns None when empty (manual check required)
    - heap[0] if heap else None
        - Equivalent to peek()
        - returns None when empty
    - heap.remove(o); heapq.heapify(heap)
        - Equivalent to remove(Object o)
        - Not recommended, O(n)
    - (o in heap)
        - Equivalent to contains(Object o)

- int, float, str (Python built-ins)
    - int(s)
        - Equivalent to valueOf(String s) or parseInt(String s)
        - Ignores leading 0s automatically
    - float(s)
        - Equivalent to Double.valueOf(String s)
    - bin(i)[2:]
        - Equivalent to toBinaryString(int i)
        - Removes the '0b' prefix

- int (Python built-in arbitrary-precision integer)
    - int(val)  
        - Equivalent to BigInteger(String val) or BigInteger(long val)
        - Python's `int` supports arbitrary precision by default
    - a + b  
        - Equivalent to add(BigInteger val)
    - a - b  
        - Equivalent to subtract(BigInteger val)
    - a * b  
        - Equivalent to multiply(BigInteger val)
    - a // b  
        - Equivalent to divide(BigInteger val)
    - abs(a)  
        - Equivalent to abs()
    - -a  
        - Equivalent to negate()
    - a % b  
        - Equivalent to mod(BigInteger m)
    - a & b  
        - Equivalent to and(BigInteger val)
    - a | b  
        - Equivalent to or(BigInteger val)
    - a ^ b  
        - Equivalent to xor(BigInteger val)
    - ~a  
        - Equivalent to not()
    - format(a, 'x') / format(a, 'b') / format(a, 'o')  
        - Equivalent to toString(radix)
        - 'x': hexadecimal, 'b': binary, 'o': octal

- math (Python built-in math module)
    - math.sqrt(a)  
        - Equivalent to Math.sqrt(double a)
    - math.ceil(a)  
        - Equivalent to Math.ceil(double a)
    - math.floor(a)  
        - Equivalent to Math.floor(double a)
    - math.pow(a, b)  
        - Equivalent to Math.pow(double a, double b)
        - Returns float
    - a ** b  
        - Also works like Math.pow(a, b)
        - Keeps int result if both a and b are int

- Iterator (Python: iterator object)
    - next() → next(it)
    - hasNext() → not applicable, use try-except StopIteration
    - remove() → not supported, modify collection manually

- str
    - str(char_list)
        - Equivalent to String(char value[])
    - ''.join(char_list[offset:offset+count])
        - Equivalent to String(char value[], int offset, int count)
    - bytes.decode()
        - Equivalent to String(byte[] bytes)
    - bytes[offset:offset+length].decode()
        - Equivalent to String(byte[] bytes, int offset, int length)
    - s[index]
        - Equivalent to charAt(int index)
    - str(i)
        - Equivalent to valueOf(int i)
    - s.encode()
        - Equivalent to getBytes()
    - list(s)
        - Equivalent to toCharArray()
    - s.startswith(prefix)
        - Equivalent to startsWith(String prefix)
    - s.endswith(suffix)
        - Equivalent to endsWith(String suffix)
    - s.find(ch)
        - Equivalent to indexOf(int ch) or indexOf(String str)
    - s.rfind(ch)
        - Equivalent to lastIndexOf(int ch) or lastIndexOf(String str)
    - s[begin:]
        - Equivalent to substring(int beginIndex)
    - s[begin:end]
        - Equivalent to substring(int beginIndex, int endIndex)
    - s.split(regex)
        - Equivalent to split(String regex)
    - s.split(regex, limit)
        - Equivalent to split(String regex, int limit)
    - s.lower()
        - Equivalent to toLowerCase()
    - s.upper()
        - Equivalent to toUpperCase()
    - s.strip()
        - Equivalent to trim()

- random
    - random()
        - Return a random float number between 0.0 to 1.0 (inclusive of 0.0, exclusive of 1.0)
    - randint(a, b)
        - Return a random integer N such that a <= N <= b
    - randrange(start, stop[, step])
        - Return a randomly selected element from range(start, stop, step)
    - uniform(a, b)
        - Return a random floating point number N such that a <= N <= b (if a < b) or b <= N <= a (if b < a)
    - choice(seq)
        - Return a random element from the non-empty sequence `seq`
    - choices(population, weights=None, *, k=1)
        - Return a k sized list of elements chosen from the population with replacement
    - sample(population, k)
        - Return a k length list of unique elements chosen from the population (without replacement)
    - shuffle(x)
        - Shuffle the sequence x in place
    - seed(a=None)
        - Initialize internal state of random number generator

## Classic Algorithms & Data Structures

Every Computer Science student is expected to be familiar with the following concepts of algorithms and data structures. Relevant leetcode problems are included for each topic.

- Elementary Data Structure
	- Stack
		- [225. Implement Stack using Queues](https://leetcode.com/problems/implement-stack-using-queues/)
		- [155. Min Stack](https://leetcode.com/problems/min-stack/)
		- [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
	- Queue
		- [232. Implement Queue using Stacks](https://leetcode.com/problems/implement-queue-using-stacks/)
		- [622. Design Circular Queue](https://leetcode.com/problems/design-circular-queue/)
		- [1670. Design Front Middle Back Queue](https://leetcode.com/problems/design-front-middle-back-queue/)
	- Deque
		- [641. Design Circular Deque](https://leetcode.com/problems/design-circular-deque/)
	- Linked List
		- Singly Linked List
			- [707. Design Linked List](https://leetcode.com/problems/design-linked-list/)
			- [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)
			- [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
		- Doubly Linked List
			- [146. LRU Cache](https://leetcode.com/problems/lru-cache/)
			- [460. LFU Cache](https://leetcode.com/problems/lfu-cache/)
- Searching and Selection
	- Sequential Search
		- [1773. Count Items Matching a Rule](https://leetcode.com/problems/count-items-matching-a-rule/)
	- Binary Search
		- [704. Binary Search](https://leetcode.com/problems/binary-search/)
		- [278. First Bad Version](https://leetcode.com/problems/first-bad-version/)
		- [33. Search in Rotated Sorted Array](https://leetcode.com/problems/search-in-rotated-sorted-array/)
	- Quick Selection
		- [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
		- [973. K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/)
- Sorting
	- Counting Sort
		- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
	- Radix Sort
		- [164. Maximum Gap](https://leetcode.com/problems/maximum-gap/)
	- Bucket Sort
		- [451. Sort Characters By Frequency](https://leetcode.com/problems/sort-characters-by-frequency/)
		- [1636. Sort Array by Increasing Frequency](https://leetcode.com/problems/sort-array-by-increasing-frequency/)
	- Selection Sort
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
	- Bubble Sort
		- No specific LeetCode problem focuses on bubble sort (rarely tested directly).
	- Insertion Sort
		- Sequential Search or Binary Search
		- [147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)
	- Shell Sort
		- No direct LeetCode problem for Shell sort (not commonly tested).
	- Merge Sort
		- Array and Linked List
		- Iterative and Recursive
		- Out-of-Place or In-Place
		- [148. Sort List](https://leetcode.com/problems/sort-list/)
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
		- [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)
	- Quick Sort
		- Lomuto Partition Scheme
		- Hoare Partition Scheme
		- Bentley-McIlroy 3-way Partitioning
		- [75. Sort Colors](https://leetcode.com/problems/sort-colors/)
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
	- Heap Sort
		- [215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
		- [912. Sort an Array](https://leetcode.com/problems/sort-an-array/)
- Tree
	- Binary Heap
		- [703. Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/)
		- [1046. Last Stone Weight](https://leetcode.com/problems/last-stone-weight/)
	- Binary Tree
		- Preorder Traversal
			- [144. Binary Tree Preorder Traversal](https://leetcode.com/problems/binary-tree-preorder-traversal/)
		- Inorder Traversal
			- [94. Binary Tree Inorder Traversal](https://leetcode.com/problems/binary-tree-inorder-traversal/)
		- Postorder Traversal
			- [145. Binary Tree Postorder Traversal](https://leetcode.com/problems/binary-tree-postorder-traversal/)
	- Binary Search Tree
		- Search
			- [700. Search in a Binary Search Tree](https://leetcode.com/problems/search-in-a-binary-search-tree/)
		- Insertion
			- [701. Insert into a Binary Search Tree](https://leetcode.com/problems/insert-into-a-binary-search-tree/)
		- Deletion
			- [450. Delete Node in a BST](https://leetcode.com/problems/delete-node-in-a-bst/)
		- Successor and Predecessor
			- [285. Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst/)
	- Self-Balanced Binary Search Tree(Optional)
- Hash Table
	- Separate Chaining
	- Linear Probing
	- [706. Design HashMap](https://leetcode.com/problems/design-hashmap/)
	- [705. Design HashSet](https://leetcode.com/problems/design-hashset/)
- Elementary Graph Algorithms
	- Depth-First Search
		- [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
		- [733. Flood Fill](https://leetcode.com/problems/flood-fill/)
	- Breadth-First Search
		- [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)
		- [127. Word Ladder](https://leetcode.com/problems/word-ladder/)
	- Cycle Detection
		- [207. Course Schedule](https://leetcode.com/problems/course-schedule/)
		- [261. Graph Valid Tree](https://leetcode.com/problems/graph-valid-tree/)
	- Topological Sort
		- [210. Course Schedule II](https://leetcode.com/problems/course-schedule-ii/)
		- [269. Alien Dictionary](https://leetcode.com/problems/alien-dictionary/)
- Minimum Spanning Tree Algorithms
	- Prim's Algorithm
		- [1584. Min Cost to Connect All Points](https://leetcode.com/problems/min-cost-to-connect-all-points/)
	- Kruskal's Algorithm
		- [1135. Connecting Cities With Minimum Cost](https://leetcode.com/problems/connecting-cities-with-minimum-cost/)
		- [1168. Optimize Water Distribution in a Village](https://leetcode.com/problems/optimize-water-distribution-in-a-village/)
- Shortest Path Algorithms
	- Dijkstra's Algorithm
		- [743. Network Delay Time](https://leetcode.com/problems/network-delay-time/)
	- Naive Bellman-Ford Algorithm
		- [787. Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)
	- Queue-based Bellman-Ford Algorithm(SPFA)
	- DAG Bellman-Ford Algorithm
	- Floyd-Warshall Algorithm
		- [1334. Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)
- Trie
	- R-way Trie
		- [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)
		- [211. Design Add and Search Words Data Structure](https://leetcode.com/problems/design-add-and-search-words-data-structure/)
	- Ternary Search Trie
- Substring Search
	- KMP Algorithm
		- [28. Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
	- Boyer-Moore Algorithm
- Algorithm Design Paradigm
	- Brute-Force
		- [1. Two Sum](https://leetcode.com/problems/two-sum/)
	- Backtracking
		- [46. Permutations](https://leetcode.com/problems/permutations/)
		- [39. Combination Sum](https://leetcode.com/problems/combination-sum/)
		- [51. N-Queens](https://leetcode.com/problems/n-queens/)
	- Divide and Conquer
		- [50. Pow(x, n)](https://leetcode.com/problems/powx-n/)
		- [241. Different Ways to Add Parentheses](https://leetcode.com/problems/different-ways-to-add-parentheses/)
	- Dynamic Programming
		- Top-Down Recursion + Memoization
			- [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)
		- Botton-Up Iteration + Cache
			- [198. House Robber](https://leetcode.com/problems/house-robber/)
	- Greedy Algorithm
		- [55. Jump Game](https://leetcode.com/problems/jump-game/)
		- [45. Jump Game II](https://leetcode.com/problems/jump-game-ii/)
		- [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

It's highly recommended that you understand and implement every one of the above algorithms or data structures. They incorporate the most classic algorithm design principles and are good programming practice. There are tons of material talking about these classic topic, [Introduction to Algorithms](https://www.amazon.com/Introduction-Algorithms-3rd-MIT-Press/dp/0262033844 "https://www.amazon.com/introduction-algorithms-3rd-mit-press/dp/0262033844") and [Algorithms 4ed](https://www.amazon.com/Algorithms-4th-Robert-Sedgewick/dp/032157351X "https://www.amazon.com/algorithms-4th-robert-sedgewick/dp/032157351x") are recommended. 

The following are some advanced topics that are less frequently seen in solving LeetCode problems, but some are still very useful, e.g. Binary Indexed Tree.

- Advanced Topics
	- 2-3 Tree
	- Red-Black Tree
	- AVL Tree
	- Connected Component
		- [323. Number of Connected Components in an Undirected Graph](https://leetcode.com/problems/number-of-connected-components-in-an-undirected-graph/)
		- [200. Number of Islands](https://leetcode.com/problems/number-of-islands/)
		- [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/)
	- B Tree
	- B+ Tree
	- Rolling Hash
		- [187. Repeated DNA Sequences](https://leetcode.com/problems/repeated-dna-sequences/)
		- [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)
		- [2156. Find Substring With Given Hash Value](https://leetcode.com/problems/find-substring-with-given-hash-value/)
	- Interval Tree
		- [715. Range Module](https://leetcode.com/problems/range-module/)
		- [2276. Count Integers in Intervals](https://leetcode.com/problems/count-integers-in-intervals/)
	- Segment Tree
		- [307. Range Sum Query - Mutable](https://leetcode.com/problems/range-sum-query-mutable/)
		- [308. Range Sum Query 2D - Mutable](https://leetcode.com/problems/range-sum-query-2d-mutable/)
		- [732. My Calendar III](https://leetcode.com/problems/my-calendar-iii/)
	- Suffix Tree
		- [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)
		- [1062. Longest Repeating Substring](https://leetcode.com/problems/longest-repeating-substring/)
	- Suffix Array
		- [1044. Longest Duplicate Substring](https://leetcode.com/problems/longest-duplicate-substring/)
		- [1062. Longest Repeating Substring](https://leetcode.com/problems/longest-repeating-substring/)
	- Range Minimum Query
	- Hierholzer's Algorithm
		- [332. Reconstruct Itinerary](https://leetcode.com/problems/reconstruct-itinerary/)
		- [753. Cracking the Safe](https://leetcode.com/problems/cracking-the-safe/)
	- Union Find/Disjoint Set
		- [684. Redundant Connection](https://leetcode.com/problems/redundant-connection/)
		- [721. Accounts Merge](https://leetcode.com/problems/accounts-merge/)
		- [990. Satisfiability of Equality Equations](https://leetcode.com/problems/satisfiability-of-equality-equations/)
	- Binary Indexed Tree/Fenwick Tree
		- [315. Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)
		- [493. Reverse Pairs](https://leetcode.com/problems/reverse-pairs/)
		- [1649. Create Sorted Array through Instructions](https://leetcode.com/problems/create-sorted-array-through-instructions/)
	- A* Algorithm
		- [773. Sliding Puzzle](https://leetcode.com/problems/sliding-puzzle/)
	- Skip List
		- [1206. Design Skiplist](https://leetcode.com/problems/design-skiplist/)
	- Treap
	- Spray Tree
	- LSD String Sort
	- MSD String Sort
	- Regular Expression
		- [10. Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)
		- [44. Wildcard Matching](https://leetcode.com/problems/wildcard-matching/)
	- Network Flow
		- [1349. Maximum Students Taking Exam](https://leetcode.com/problems/maximum-students-taking-exam/)

## Build Your Own Templates

TODO

