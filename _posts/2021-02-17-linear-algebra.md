---
layout: post
title:  "Linear Algebra Cheatsheet"
date:   2021-02-16 12:00:00 +0100
categories: "linear algebra"
tags: math
---
{% include math.html %}
<!--more-->

Disclaimer: sometimes I use n x m, sometimes m x n. Therefore do not assume the meaning of n nor m to always be either the number of entries or the number columns. Always pay attention to how the matrix is defined. The convention is rows x columns.

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
    - [Linear Combinations](#linear-combinations)
    - [A**x** = **b**](#ax--b)
      - [A**x**](#ax)
      - [All true XOR All false](#all-true-xor-all-false)
  - [Solution sets and linear independence](#solution-sets-and-linear-independence)
    - [Linear dependence](#linear-dependence)
    - [Homogeneous Linear Systems](#homogeneous-linear-systems)
      - [Solution sets and dependence](#solution-sets-and-dependence)
    - [Parametric Vector Equation](#parametric-vector-equation)
    - [Nonhomogeneous Linear Systems](#nonhomogeneous-linear-systems)
  - [Linear transformations](#linear-transformations)
    - [Matrix transformations](#matrix-transformations)
    - [The Matrix of a Linear transformation](#the-matrix-of-a-linear-transformation)
      - [Identity matrix](#identity-matrix)
  - [2D Transformations](#2d-transformations)
    - [90° rotation (counter clockwise)](#90-rotation-counter-clockwise)
    - [α rotation (in rad)](#α-rotation-in-rad)
    - [Reflection through the x-axis](#reflection-through-the-x-axis)
    - [Reflection through the y-axis](#reflection-through-the-y-axis)
    - [Reflection through the line y=x](#reflection-through-the-line-yx)
    - [Reflection through the origin](#reflection-through-the-origin)
    - [Reflection through the line y = -x](#reflection-through-the-line-y---x)
    - [Horizontal contraction and expansion](#horizontal-contraction-and-expansion)
    - [Vertical contraction and expansion](#vertical-contraction-and-expansion)
    - [Horizontal shear](#horizontal-shear)
    - [Vertical shear](#vertical-shear)
    - [i and j are dependent](#i-and-j-are-dependent)
    - [Projection onto the x-axis](#projection-onto-the-x-axis)
    - [Projection onto the y-axis](#projection-onto-the-y-axis)
    - [Surjective (onto) and injective (one-to-one) mappings](#surjective-onto-and-injective-one-to-one-mappings)
    - [Composition of 2 transformations](#composition-of-2-transformations)
  - [Matrix operations](#matrix-operations)
    - [Matrix rules](#matrix-rules)
    - [Matrix multiplication](#matrix-multiplication)
      - [Zero matrix](#zero-matrix)
      - [Square matrix](#square-matrix)
      - [Identity matrix](#identity-matrix-1)
      - [Lower/upper triangular matrix](#lowerupper-triangular-matrix)
    - [Matrix sum](#matrix-sum)
    - [Scalar multiplication](#scalar-multiplication)
    - [Transposing](#transposing)

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

Which equals the parametric description (of the solution set) below...

### Parametric description of solution sets

...example follow up:

$$
\begin{cases}
               x_1 = 1 + 5x_3\\
               x_2 = 4 - x_3\\
               x_3\ is\ free
            \end{cases}
$$

$x_3$ is free means that you are free to choose any value for $x_3$ (also called **paramater**) and each different choice of $x_3$ determines a (different) solution of the system.


Solving a system amounts to finding a parametric description of the solution set (such as the system of equations above or such as the [Parametric Vector Equation](#parametric-vector-equation)) or determining that the solution set is empty:
* If a system contains an equation of the form 0 = b, then it is inconsistent, else, consistent.
* If the system is consistent and has free variables, then it has $\infty$ solutions.
* If the system is consistent and has no free variables, then it has 1 solution.
* If a system is inconsistent, the solution set is empty, even if it has free variables.

## Spans and vector equations
### Vector Equations
* A matrix with only 1 column is a **column vector** or a **vector**
  * A **vector** stands for an "ordered list of numbers".
  * The vector whose entries are all zero is called the **zero vector**, denoted by **0**
* Vector sum:

$$\begin{bmatrix}   u_1 \\    u_2 \end{bmatrix} + \begin{bmatrix}   v_1 \\    v_2 \end{bmatrix} = \begin{bmatrix}   u_1 + v_1 \\    u_2 + v_2\end{bmatrix}$$

* Vector scaling:

$$a\cdot\begin{bmatrix}   u_1 \\    u_2 \end{bmatrix} = \begin{bmatrix}   a\cdot u_1 \\    a\cdot u_2 \end{bmatrix}$$

* The set of all vectors with 2 **entries** ("vector rows"/numbers) is expressed as $\Bbb R^2$
  * This represents all the vectors within a 2D/plane space
  * Two vectors in $\Bbb R^2$ are equal iff all entries are equal and in the same order.
  * The first entry is assigned to the x-coordinate and the second entry to the y-coordinate.
* $\Bbb R^3$ represents the set of all vectors in a 3D space
  * 1D and 2D vectors are not implicitly allowed as the vectors in $\Bbb R^3$ must have 3 coordinates (x,y,z)
  * However 1Ds may appear as (x,0,0) and 2Ds as (x,y,0) (but with the vertical notation)

### Linear Combinations
Let the vector **b** be defined by

$$ b = x_1\begin{bmatrix}   a_{11} \\ a_{21} \\   \vdots \\ a_{m1} \end{bmatrix} + x_2\begin{bmatrix}   a_{21} \\ a_{22} \\   \vdots \\ a_{m2} \end{bmatrix} + \cdots $$

* **$b$** is a linear combination of **$a_1$**, **$a_2$**... with **weights** (scalars) $x_1$, $x_2$, ...
  - observe that while the columns are **$a_1$**, **$a_2$**..., when we combine rows with columns, the row index is placed before the column index.
* The vector equation above has the same solution set as the linear system whose augmented matrix is:

$$
\begin{bmatrix}
    a_{11} & a_{12} & \cdots & a_{1n} & | & b_1 \\
    a_{21} & a_{22} & \cdots & a_{2n} & | & b_2 \\
    a_{31} & a_{32} & \cdots & a_{3n} & | & b_3 \\
    \vdots & \vdots & \cdots & \vdots & | & \vdots \\
    a_{m1} & a_{m2} & \cdots & a_{mn} & | & b_{mn} \\
\end{bmatrix}
$$

* **b** can only be generated by a linear combination of **a** iff the augmented matrix has a solution
* The set of all possible linear combinations of a list of vectors $a_1$, $a_2$, ... $a_n$ is denoted as Span{$a_1$, $a_2$, ... $a_n$}
  * That is, all the b's such that
  
$$x_1\begin{bmatrix}   a_{11} \\ a_{21} \\   \vdots \\ a_{m1} \end{bmatrix} + x_2\begin{bmatrix}   a_{21} \\ a_{22} \\   \vdots \\ a_{m2} \end{bmatrix} + \cdots = b$$
  
  *  yields an augmented matrix with solution.

### A**x** = **b**

#### A**x**

* This represents the multiplication of a matrix A by an ordered list of numbers in x.
  * This is defined iff the number of entries in x is the same as the number of columns in A.

$$
Ax = 
\begin{bmatrix}
    a_{11} & a_{12} & \cdots & a_{1n}  \\
    a_{21} & a_{22} & \cdots & a_{2n}  \\
    a_{31} & a_{32} & \cdots & a_{3n}  \\
    \vdots & \vdots & \cdots & \vdots  \\
    a_{m1} & a_{m2} & \cdots & a_{mn}  \\
\end{bmatrix} = x_1\begin{bmatrix}   a_{11} \\ a_{21} \\   \vdots \\ a_{m1} \end{bmatrix} + x_2\begin{bmatrix}   a_{12} \\ a_{22} \\   \vdots \\ a_{m2} \end{bmatrix} + \cdots
$$

* Ax = b has the same solution set as the vector equation:

$$x_1\begin{bmatrix}   a_{11} \\ a_{21} \\   \vdots \\ a_{m1} \end{bmatrix} + x_2\begin{bmatrix}   a_{12} \\ a_{22} \\   \vdots \\ a_{m2} \end{bmatrix} + \cdots = b$$

* which has the same solution set as the augmented matrix

$$
\begin{bmatrix}
    a_{11} & a_{12} & \cdots & a_{1n} & | & b_1 \\
    a_{21} & a_{22} & \cdots & a_{2n} & | & b_2 \\
    a_{31} & a_{32} & \cdots & a_{3n} & | & b_3 \\
    \vdots & \vdots & \cdots & \vdots & | & \vdots \\
    a_{m1} & a_{m2} & \cdots & a_{mn} & | & b_{m} \\
\end{bmatrix}
$$

* Iff A**x** = **b** has a solution, then we can say that **b** is a linear combination of the columns of A (with x as the column weights).
  * This make sense since A**x** = **b** is just the augmented matrix of a linear system where replacing the **x** coefficients for the solution **s** give you the linear combination on the left side of the equation such as it matches the right side vector (**b**) making the equation true.

#### All true XOR All false
Let A be an m x n matrix, these statements are either all true or all false.
* A**x** = **b** has a solution for all **b**'s in $\Bbb R^m$
* All **b**'s in $\Bbb R^m$ are a linear combination of A
* Span{$a_1,a_2,\cdots,a_n$} = $\Bbb R^m$
* A has a pivot position in every row

## Solution sets and linear independence
### Linear dependence

* A set of two vectors {$a_1$, $a_2$} is linearly dependent **iff** at least one of the vectors is a multiple of the other.
* A set of 2 or more vectors is linearly dependent **iff** at least one of the vectors is within the span of another vector(s) other than itself. That is, if a vector can be expressed as a linear combination of other vectors in the set.
* A set of 1 vector is always independent except for the **0** vector.
  * This is because $x_10=0$ has $\infty$ nontrivial solutions.
* If a set of vectors has more vectors than vector entries, then there will be at least a vector within the span of another vector(s), making the set linear dependent.
  * If a matrix has more columns than rows, then there will be free variables, therefore making the matrix dependent.
* If a set contains the zero vector, the set is linearly dependent
  * If a matrix has an all-zeros column, the matrix columns are linearly dependent.

### Homogeneous Linear Systems
* Systems that have the form A**x** = **0**
* Always have at least x = **0** solution (aka **trivial solution**)
* **Nontrivial solution** is a **non-zero vector x** that satisfies A**x** = **0**
* **Ax = 0 has a nontrivial solution iff the system has at least one free variable**
  * You can then use there an arbitrary non-zero number in the solution which makes $x \neq 0$
  * Each of these infinityely many nontrivial solutions of **Ax** = **0** are called **linear dependence relation** among $a_1$, $a_2$, ..., $a_n$.
  
#### Solution sets and dependence
* The solution set of an homogenous system (A**x**=**0**) can always be expressed as:
  * If it has only trivial, the solution set equals Span{**0**}
    * Which is the (0,...,0) coordinate 
    * Which also means that the column vectors of A are **[linear independent](#linear-dependence)**
  * If it has nontrivial, the solution set equals Span{$v_1 \cdots v_n$} (which is not necessarily $\Bbb R^n$, think of $v_1$[1,0,0], $v_2$[0,1,0], $v_3$[1,1,0] -assume vertical notation-, it spans a plane in $\Bbb R^3$, not the whole $\Bbb R^3$)
    * Only 1 free variable Span{$v_1$} means the solution is a line through the origin. (i.e. the x-axis)
    * 2 free variables Span{$v_1,v_2$} can yield a plane (or a line if $v_1$ and $v_2$ are **[linear dependent](#linear-dependence)**) through the origin. (i.e. the x-y plane)
    * It also means that there is a [parametric Vector Equation](#parametric-vector-equation) where there exists weights other than 0 for x that make A**x**=**0** true
* Therefore, when they ask you, is {$a_1,a_2,\cdots,a_n$} a dependent or an independent set of vectors, they're actually asking...
  * **Dependent: Does Ax=0 have at least a nontrivial solution?**
    - Check if it has at least 1 **free variable.**
  * **Independent:  Does Ax=0 have only the trivial x=0 solution?**
    - Check if there are **pivots in every row of the echelon reduced matrix.**

### Parametric Vector Equation
The parametric description of a (homogenous) solution set can be expressed in a single equation:

$$x = x_1v_1 + x_2v_2 + x_3v_3 \cdots$$

in CSE1205 $x = sv_1 + tv_2 + uv_3 $

### Nonhomogeneous Linear Systems
When these systems have $Ax \neq 0$ the system is translated by a vector constant **p** from the origin:

$$x = p + x_1v_1 + x_2v_2 + x_3v_3 \cdots$$

* The vector **$p$** itself is just one **particular** solution of A**x** = **b** corresponding to $x_1 \cdots x_n = 0$
  * $xv$ is the **general** solution of A**x**=**0**
* When there is only 1 parameter vector (v) this equation is also called "**the equation of the line through p parallel to v**"
* The solution set of A**x** = **b** is:
  - empty if it has no solution
  - a translate of the (parametric) solution for A**x**=**0**
* The parametric system of equations of the previous example: 

$$
\begin{cases}
               x_1 = 1 + 5x_3\\
               x_2 = 4 - x_3\\
               x_3\ is\ free\ (x_3 = 0 + x_3)
            \end{cases}
$$

Is represented as the parametric vector equation:

$$x = \begin{bmatrix}   1 \\ 4 \\ 0 \end{bmatrix} + x_3\begin{bmatrix} 5 \\ -1 \\ 1 \end{bmatrix}$$

## Linear transformations
All matrix transformations are **linear** functions, but not al linear functions are matrix transformations (i.e f(x)=3x+5).
A **matrix** linear transformation T must have the same properties as a Matrix-Vector Product A**x**:

$$(A(\mathbf{u} + \mathbf{v}) = A\mathbf{u} + A\mathbf{v}) \leftrightarrow (T(\mathbf{u} + \mathbf{v}) = T\mathbf{u} + T\mathbf{v})$$

$$(A(c\mathbf{u}) = c(A\mathbf{u})) \leftrightarrow (T(c\mathbf{u}) = c(T\mathbf{u}))$$


For an n x m matrix, the columns are vectors in $R^n\ {or}\ R^{rows}$. A**x**=**b** would only be defined when the entries in x are the same as the number of columns (m), therefore x is in $R^m$, but the output b has the same number of entries as A has rows, therefore in $R^n$.

The correspondence from **x** to **Ax** is a **function** or a **transformation** or a **mapping** from one set of vectors in $R^m$ to another in $R^n$. Formally denoted as $f: R^m \rightarrow R^n$, where '$f$' just happens to be the arbitrary name that we gave
to the function (or transformation, mapping, or even relation, see CSE1300).

The "dimension" of the **inputs** is called the **domain** of '$f$', in this case $R^m$, and the "dimension" of the **output** (formally kown as "image" or vagely written as *f(x)*) is called the **co-domain**, in this case $R^n$. However, this does not necessarily mean that all "dimensions" within the co-domain are "used", as a possible transformation to a co-domain in $R^3$ could deliberatly have z = 0 for all outputs. The **range** of $f$ (the function/transformation/mapping), is the **set of all outputs**, 
which happens to be the Span of each of the vector columns of A.

 In the example used where z is always 0, we would have a plane as a **range**, but still inside the 3D dimension (it would still have 3 coordinates, even if the 3rd is the same for all outputs). Since we know by the definition of this particular function that the co-domain is in the 3D dimension, we can already deduce that in the case of a
 $f(x)= A\mathbf{x}$ function, the column vectors of A have to be in the same $R^n$ as the co-domain, $R^3$ in this case.

### Matrix transformations

In this context, $T(x)$ is reserved for $T(x) = A\mathbf{x}$. This matrix transformation is also denoted as
$x \mapsto A\mathbf{x}$.

When the transformation maps an input to an output in the same dimension, it is called a **shear transformation**:


$$Let\ u = \begin{bmatrix} 0\\ 2 \end{bmatrix} and \ A = \begin{bmatrix} 1 & 3\\ 0 & 1 \end{bmatrix}$$

$$Then\ T(u) = 
\begin{bmatrix}
1 & 3\\
0 & 1 \end{bmatrix}
\begin{bmatrix}
0\\
2 \end{bmatrix} =
\begin{bmatrix}
6\\
2 \end{bmatrix}$$

When a transformation goes from a higher dimension to a lower one we can say that $x \mapsto A\mathbf{x}$ **projects** points in
$R^k$ onto $R^{k-1}$.

$$\begin{bmatrix}
x_1\\
x_2\\
x_3
\end{bmatrix}
\mapsto
\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 0 \end{bmatrix}
\begin{bmatrix}
x_1\\
x_2\\
x_3 \end{bmatrix} = \begin{bmatrix}
x_1\\
x_2\\
0 \end{bmatrix}$$

### The Matrix of a Linear transformation
#### Identity matrix
The identity matrix with size n x n, also denoted as $I_n$, is the matrix of a T(x) transformation such as T(x) = x.
The identity matrix returns the input in the same form since each of it's columns (n) contains only a 1 in the nth entry:

$$I_2 = \begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}$$

$$Let\ T(x) = I_2\mathbf{x}$$

$$T(
\begin{bmatrix}
5\\
3
\end{bmatrix}) = \begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}\begin{bmatrix}
5\\
3
\end{bmatrix}$$


$$=5\begin{bmatrix}
1\\
0
\end{bmatrix}
3
\begin{bmatrix}
0\\
1
\end{bmatrix}=
\begin{bmatrix}
5\\
0
\end{bmatrix}
+
\begin{bmatrix}
0\\
3
\end{bmatrix}=
\begin{bmatrix}
5\\
3
\end{bmatrix}=\mathbf{x}$$

The columns of $I_n$ are formally referred to as $\mathbf{e_n}$. For a 2x2 A matrix we may express the first and second column vectors as $\vec{i}$ and $\vec{j}$.

In the same way that in a 'f(x) = ax + b' linear function we can draw the line from just 2 points, Since T(x) is also a linear function, that means that knowing the output of 2 inputs is enough information to deduce the matrix used for the Matrix-vector product. The matrix used for the Matrix-vector product is called the **standard matrix for the linear transformation**.

## 2D Transformations
To easily see what the 2x2 A matrix does to a vector, analyze the difference between the default values for $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$, vs their new values defined by A. The default values are:

$$
\color{red}{\vec{j} = \begin{bmatrix}0\\1\end{bmatrix}} and\ \color{green}{\vec{i} = \begin{bmatrix}1\\0\end{bmatrix}} = \color{red}{\uparrow}\color{green}{\rightarrow}
$$

<iframe src="https://www.geogebra.org/classic/hxwycrkp?embed" width="800" height="600" allowfullscreen style="border: 1px solid #e4e4e4;border-radius: 4px;" frameborder="0"></iframe>

### 90° rotation (counter clockwise)

To do so the green arrow would have to point to (0,1) and the red arrow would have to point to (-1,0). The new values for $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ would be :

$$\vec{i}=\begin{bmatrix}0\\1\end{bmatrix} and\ \vec{j}=\begin{bmatrix}-1\\0\end{bmatrix}$$

Resulting in:

$$T(x)=\begin{bmatrix}0 & -1\\1 & 0\end{bmatrix}\begin{bmatrix}x_1\\x_2\end{bmatrix}$$

### α rotation (in rad)

When prompted to calculate the standard matrix for a rotation by α rad, $\color{green}{\vec{i}}$ previous coordinates (1,0) will change in the same fashion as the unit circle:

$$\color{green}{\vec{i}} = \begin{bmatrix}cos(\alpha)\\sin(\alpha)\end{bmatrix}$$

The changes for $\color{red}{\vec{j}}$ will be the same, but with a $\frac{1}{2}\pi$ offset, as $\color{red}{\vec{j}}$ was already 90° counterclockwise from $\color{green}{\vec{i}}$:

$$\color{red}{\vec{i}} = \begin{bmatrix}cos(\alpha + \frac{1}{2}\pi)\\sin(\alpha + \frac{1}{2}\pi)\end{bmatrix} = 
\begin{bmatrix}sin(\frac{1}{2}\pi - \alpha - \frac{1}{2}\pi)\\cos(\frac{1}{2}\pi - \alpha - \frac{1}{2}\pi)\end{bmatrix} =
\begin{bmatrix}sin(- \alpha)\\cos(- \alpha)\end{bmatrix} = \begin{bmatrix}-sin(\alpha)\\cos(\alpha)\end{bmatrix}
$$




### Reflection through the x-axis

Here $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ would have to go from $\color{red}{\uparrow}\color{green}{\rightarrow}$ to $\color{red}{\downarrow}\color{green}{\rightarrow}$. That means that only $\color{red}{\vec{j}}$ is changed:

$$\vec{j_{old}}=\begin{bmatrix}0\\1\end{bmatrix} \longrightarrow \vec{j_{new}}=\begin{bmatrix}0\\-1\end{bmatrix} \longrightarrow A=\begin{bmatrix}1 & 0\\0 & -1\end{bmatrix}$$ 

### Reflection through the y-axis

Here $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ would have to go from $\color{red}{\uparrow}\color{green}{\rightarrow}$ to $\color{green}{\leftarrow}\color{red}{\uparrow}$. That means that only $\color{green}{\vec{i}}$ is changed:

$$\vec{i_{old}}=\begin{bmatrix}1\\0\end{bmatrix} \longrightarrow \vec{i_{new}}=\begin{bmatrix}-1\\0\end{bmatrix} \longrightarrow A=\begin{bmatrix}-1 & 0\\0 & 1\end{bmatrix}$$ 

### Reflection through the line y=x

This is also known as the inverse of a function. We simply have to swipe the values between $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$, resulting in:

$$A=\begin{bmatrix}0 & 1\\1 & 0\end{bmatrix}$$ 

This could also be achieved with:
* rotation of 90° counter clockwise followed by a reflection in the y-axis
* reflextion in the x-axis followed by rotation of  90° counter clockwise

### Reflection through the origin

This is simply a 180° rotation, where we scale both $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ by -1, resulting in:

$$A=\begin{bmatrix}-1 & 0\\0 & -1\end{bmatrix}$$ 

You should have noticed that a reflection scales vectors by -1, whereas rotation swaps 0 for 1s. The reflection through the line (the inverse) combines both.

### Reflection through the line y = -x

This combines a reflection through the origin with an inverse.

$$Inverse = \begin{bmatrix} 0 & 1\\ 1 & 0 \end{bmatrix} \longrightarrow Reflection_{origin} = \begin{bmatrix} 0 & -1\\ -1 & 0 \end{bmatrix}
$$

### Horizontal contraction and expansion

$$A = \begin{bmatrix} k & 0 \\ 0 & 1\end{bmatrix}$$

If k > 1, then the image is stretched out. If 0 < k < 1, then the image is squeezed. If k < 0 the image is mirrored on the y-axis.

### Vertical contraction and expansion

$$A = \begin{bmatrix} 1 & 0 \\ 0 & k\end{bmatrix}$$

If k > 1, then the image is stretched out. If 0 < k < 1, then the image is squeezed. If k < 0 the image is mirrored on the x-axis.

### Horizontal shear

$$A = \begin{bmatrix} 1 & k \\ 0 & 1\end{bmatrix}$$

Here $\color{red}{\vec{j}}$ is tranformed from $\color{red}{\uparrow}$ to $\color{red}{\nearrow}$ with k slope.

Which means that as we move up, the image is stretched horizontally by k units.

### Vertical shear

$$A = \begin{bmatrix} 1 & 0 \\ k & 1\end{bmatrix}$$

Here $\color{green}{\vec{i}}$ is tranformed from $\color{green}{\rightarrow}$ to $\color{green}{\nearrow}$ with k slope.

Which means that as we move right, the image is stretched vertically by k units.

### i and j are dependent

If both $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ are dependent (they can be expressed as a linear combination of the other, in this case as a scalar of the other), such as in the matrix with all 1's, we would endup with $\color{red}{\nearrow}\color{green}{\nearrow}$. It means that both $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ "collapse" into each other making the image of the transformation get completly squeezed in the line that goes through both $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$.


### Projection onto the x-axis

The transformed image only has x-values (y remains constant 0).

$$A=\begin{bmatrix} 1 & 0 \\ 0 & 0\end{bmatrix}$$

### Projection onto the y-axis

The transformed image only has y-values (x remains constant 0).

$$A=\begin{bmatrix} 0 & 0 \\ 0 & 1\end{bmatrix}$$

### Surjective (onto) and injective (one-to-one) mappings

* A mapping $T:A\rightarrow B$ is **surjective** (A is **onto** B) iff Range = Codomain.
  * T(x) is surjective (onto) **iff** the **columns of A span $R^{rows}$**
    - A**x** = **b** has a solution for all **b**'s in $\Bbb R^m$
    - All **b**'s in $\Bbb R^m$ are a linear combination of A
    - A has a pivot position in every row
* A mapping $T:A\rightarrow B$ is **injective** (A is **one-to-one** with B) iff every input gets a unique output.
  * **T(x) is injective (onte-to-one) iff T(X) = 0 only has the trivial solution** (not being injective doesn't have to make you surjective and viceversa)
    - A has a pivot position in every row (no free variable)
    - The **columns of A are linearly independent**
* A mapping is bijective if it is both, surjective and injective.

### Composition of 2 transformations

Where $A_1$ and $A_2$ are the standard matrixes for the the 1st and the 2nd transformations respectively:

$$T(\mathbf{x})=
A_2\Big( A_1\begin{bmatrix}x \\ y\end{bmatrix}\Big)
$$

Which results in a matrix multiplication, explained in the chapter below.

## Matrix operations

### Matrix rules
* **ORDER MATTERS:** $A_2\cdot A_1\neq A_1\cdot A_2$
* cancellation laws do not apply in matrix algebra
* **associative law**: $A(BC) = (AB)C$, becuase the order is the same! (always right to left).
* **distributive law**:
  - A(B+C) = AB + AC (keep A on the side where it was)
  - (B+C)A = BA + CA
* r(AB) = (rA)B = A(rB) for any scalar r
* $I_nA=AI_n=A$
* **A TRANSLATION IS NOT A LINEAR TRANSFORMATION**. In a transformation, the origin is always in the same spot. In a translation it is not.
* $A^k = A\cdot A \dots A$ for k times.
* $A^0 = I_n$

### Matrix multiplication

From the example above where the composition of 2 transformation was

$$A_2\Big( A_1\begin{bmatrix}x \\ y\end{bmatrix}\Big)$$

in which we can clearly see that first we compute the things inside the brackets, and then the things outside, matrix multiplications come from this concept, and **matrix products are therefore read from right to left**.

Following up the example of 2 transformations. The matrix of the first transformation $A_1$ contains 2 columns, the updated versions of $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$:

$$A_1 = \begin{bmatrix} \color{green}{\vec{i_{A_1}}} & \color{red}{\vec{j_{A_1}}}
\end{bmatrix}$$

Then, to identify the new $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ for $A_2$, we'd have to apply a vector multiplication of $\color{green}{\vec{i_{A_1}}}$ and $\color{red}{\vec{j_{A_1}}}$ with $A_2$ respectively. It's easier to think about the transformation by focusing on just one vector at a time, so we apply 2 sequent transformations for $\color{green}{\vec{i}}$ and $\color{red}{\vec{j}}$ respectively, and once we're done, we join $\color{green}{\vec{i_{A_2}}}$ and $\color{red}{\vec{j_{A_2}}}$ into a single matrix $A_2$:

$$\color{green}{\vec{i_{A_2\cdot A_1}}} = A_2\cdot\color{green}{\vec{i_{A_1}}}$$

$$\color{red}{\vec{j_{A_2\cdot A_1}}} = A_2\cdot\color{red}{\vec{i_{j_1}}}$$

$$A_2\cdot A_1 = \begin{bmatrix} \color{green}{\vec{i_{A_2\cdot A_1}}} & \color{red}{\vec{j_{A_2\cdot A_1}}}\end{bmatrix}$$

The properties discussed here can be applied to larger matrixes.

#### Zero matrix

Expressed as 0, it's the equivalent of a m x n (size assumed by context), with all zeros.

#### Square matrix

m = n

#### Identity matrix

Expressed as $I_n$ It is a n x n **diagonal matrix** (all of the diagonal matrix are n x n), and as any diagonal matrix, it is a zero matrix besides for the **(main) diaogonal entries**, these happen to be all 1's and the property that it has is $A\cdot I_n = A$

#### Lower/upper triangular matrix

All entries above/below the main diagional are zeros.

### Matrix sum

Sum the columns, only defined when both matrixes have the exact same dimensions.

### Scalar multiplication

Scale every column. The scalar is only defined when it comes to the left of the matrix. The right side of the matrix should be reserved fo matrixes or vectors (which are just 1 column matrixes).

### Transposing

You sawp the columns for the rows (just like in excel). There are some properties:

* $(A^T)^T=A$
* $(A+B)^T=A^T+B^T$
* For any scalar $r$: $(rA)^T = rA^T$
* $(AB)^T = B^TA^T$





