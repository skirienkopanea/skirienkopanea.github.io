---
layout: post
title:  "Linear Algebra Cheatsheet"
date:   2021-02-16 12:00:00 +0100
categories: "linear algebra"
tags: math
---
{% include math.html %}
<!--more-->
# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Systems of Linear Equations](#systems-of-linear-equations)
    - [Linear equation](#linear-equation)
    - [Linear system](#linear-system)
    - [Matrix Notation](#matrix-notation)
    - [(Reduced) Echelon Matrix](#reduced-echelon-matrix)
      - [Echelon Matrix](#echelon-matrix)
      - [Reduced Echelon Matrix](#reduced-echelon-matrix-1)
    - [Solving a system](#solving-a-system)
    - [Row operations:](#row-operations)
    - [Row Reduction Algorithm](#row-reduction-algorithm)
      - [Forward phase](#forward-phase)
      - [Back-ward phase](#back-ward-phase)
    - [Variables](#variables)
      - [Basic variables](#basic-variables)
      - [Free variables](#free-variables)
    - [Parametric description of solution sets](#parametric-description-of-solution-sets)
  - [Spans and vector equations](#spans-and-vector-equations)
    - [Vector Equations](#vector-equations)

## Systems of Linear Equations
### Linear equation

$$
a_1 x_1 + a_2 x_2 + \cdots + a_n x_n = b
$$

* a's are the **coefficients** also called **weights**
* b is the **constant**

### Linear system

$$
a_{11} x_{1} + a_{12} x_{2} + \cdots + a_{1n} x_{n} = b \\
a_{21} x_{1} + a_{22} x_{2} + \cdots + a_{2n} x_{n} = c
$$

* a **solution** is a list ($s_1,s_2,\cdots,s_n$), also called **s**, where $s$ is usually represented as a vector, since all the x's can also be represented as a vector **x**.

$$ x =
\begin{pmatrix}
    x_1 \\
    x_2  \\
    \cdots  \\
    x_n 
\end{pmatrix}
\,\
s =
\begin{pmatrix}
    s_{1} \\
    s_{2}  \\
    \cdots  \\
    s_{n} 
\end{pmatrix}
$$

* Substituing s for x makes the equation true.
* Two linear systems are **equivalent** if they have the same **solution set**.
* The **solution set** is the set that contains all valid solution vectors.
* The solution set of a system of linear equation has:
  - no solution
  - exactly one solution
  - infinitely many solutions.
* A system is **consistent** if it has 1 or $\infty$ solutions.
* A system is **inconsistent** if has no solution.

### Matrix Notation
We can express the system below as a matrix

$$
a_{11} x_{1} + a_{12} x_{2} + \cdots + a_{1n} x_{n} = b \\
a_{21} x_{1} + a_{22} x_{2} + \cdots + a_{2n} x_{n} = c\\
\cdots\\
a_{m1} x_{1} + a_{m2} x_{2} + \cdots + a_{mn} x_{n} = k\\

=\\
\begin{bmatrix}
    a_{11} & a_{12} & \dots  & a_{1n} & | & b \\
    a_{21} & a_{22} & \dots  & a_{2n} & | & c \\
    \vdots & \vdots & \ddots & \vdots & | & \vdots \\
    a_{m1} & a_{m2} & \dots  & a_{mn} & | & k
\end{bmatrix}
$$

* **coefficient matrix**: Matrix representation that expresses **only** the **coefficients**
* **augmented matrix**: Matrix representation that includes the **constants**, sometimes separated by a vertical line
* The size of a matrix is expressed as **rows x columns** (m x n).

### (Reduced) Echelon Matrix
#### Echelon Matrix

$$
\begin{bmatrix}
    \blacksquare & * & \dots  & * & | & * \\
    0 & \blacksquare & \dots  & * & | & * \\
    \vdots & \vdots & \ddots & \vdots & | & \vdots \\
    0 & 0 & \dots  & \blacksquare & | & * \\
    0 & 0 & 0  & 0 & | & 0
\end{bmatrix}
$$

#### Reduced Echelon Matrix

$$
\begin{bmatrix}
    1 & 0 & \dots  & 0 & | &  * \\
    0 & 1 & \dots  & 0 & | &  * \\
    \vdots & \vdots & \ddots & \vdots & | & \vdots \\
    0 & 0 & \dots  & 1 & | &  * \\
    0 & 0 & 0  & 0 & | & 0
\end{bmatrix}
$$

* $\blacksquare$ represents any non-zero number and **\*** represents any number.
* $\blacksquare$ identify the **pivot** positions, also called **leading entries**.

Echelon form requirements:
* All-zero rows must be placed at the bottom
* Leading entries must be displayed as "downstairs"
  * It is fine if there's a "gap"/"missing leading entry"
  * In such occasion we find a non-pivot column
* $\blacksquare$ only has zeros below

Reduced echelon form requirements:
* The **leading entries** are 1's
* The 1's only have zero's below **and** above.
* While there are infinity ways to express an echelon matrix (with different scale), there's only 1 reduced echelon form.
* We call U the reduced echelon form of matrix A

### Solving a system
* Write the augmented matrix of the system.
* Bring matrix to the reduced echelon form with row operations.
* Provide the parametric description of the solution set, declare that the solution set is empty or give the vector notation of the unique solution.

### Row operations:
1. **Replacement**: Add a scaled row to another row
2. **Interchange**: Swap rows
3. **Scaling**: Scale row by a non-zero constant

### Row Reduction Algorithm
#### Forward phase
1. Start at the first non-zero column
2. Swap rows if needed to keep the pivot entry at the top.
   1. Choose a row whose leading entry is already a 1 or scale it into a 1 leading entry.
3. Work your way down with row operations so that there are only 0s under the pivot entry.
4. Move onto the next column and repeat the same process.

#### Back-ward phase
1. Start at the bottom right entry
2. Work your way up with row operations and try to remove all zeros above.

### Variables
The row reduction algorithm will not always produce a "clean" reduced echelon form where each row has a pivot. 
#### Basic variables
* Variables that correspond to pivots $\blacksquare$

#### Free variables
* Variables that correspond to non-pivot coeffcients $*$

Example: 

$$
\begin{bmatrix}
    1 & 0 & -5 & | & 1 \\
    0 & 1 & 1 & | & 4 \\
    0 & 0 & 0 & | & 0
\end{bmatrix}
$$

Corresponds to:

$x_1 \qquad -5x_3 = 1$

$\qquad x_2 +x_3 \ \ = 4$

$\qquad \qquad \quad \ 0 = 0$

Which equals the parametric description (of the solution set):

$$
\begin{cases}
               x_1 = 1 + 5x_3\\
               x_2 = 4 - x_3\\
               x_3\ is\ free
            \end{cases}
$$

$x_3$ is free means that you are free to choose any value for $x_3$ (also called **paramater**) and each different choice of $x_3$ determines a (different) solution of the system.

### Parametric description of solution sets
Solving a system amounts to finding a parametric description of the solution set or determining that the solution set is empty:
* If a system contains an equation of the form 0 = b, then it is inconsistent, else, consistent.
* If the system is consistent and has free variables, then it has $\infty$ solutions.
* If the system is consistent and has no free variables, then it has 1 solution.
* If a system is inconsistent, the solution set is empty, even if it has free variables.

## Spans and vector equations
### Vector Equations
* A matrix with only 1 column is a **column vector** or a **vector**
  * A **vector** stands for an "ordered list of numbers".
* The set of all vectors with 2 **entries** ("vector rows"/numbers)



