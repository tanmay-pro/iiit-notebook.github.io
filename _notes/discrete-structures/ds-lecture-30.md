---
date: 2020-11-26
title: DS Lecture 30
author: Abhinav S Menon
code: ma5.101
number: 30
---

## Rings
### Definition
A ring $(R, \cdot, * )$ is a set $R$ equipped with two binary operations (say $\cdot$ and $*$) which satisfy the following properties:  
1. $R$ forms an abelian group under $\cdot$, *i.e.*,  
    (i) $R$ is closed under $\cdot$ $(\textbf{A}_1)$  
    (ii) $\cdot$ is associative in $R$ ($\textbf{A}_2$)  
    (iii) $\cdot$ has an identity in $R$ ($\textbf{A}_3$)  
    (iv) all $r \in R$ have inverses $r^{-1} \in R$ under $\cdot$ ($\textbf{A}_4$)  
    (v) $\cdot$ is commutative in $R$ ($\textbf{A}_5$)  

2. $R$ forms a semigroup under $*$, *i.e.*,  
    (i) $R$ is closed under $*$ ($\textbf{M}_1$)  
    (ii) $R$ is associative in $R$ ($\textbf{M}_2$)  

3. $* $ distributes over $\cdot$ in $R$ ($\textbf{M}_3$), *i.e.*,  
    (i) $a * (b \cdot c) = (a * b) \cdot (a * c)$,  
    (ii) $(a \cdot  b ) * c = (a * c) \cdot (b * c)$,  
    $\forall a,b,c \in R$.
    
    
$(R, \cdot, * )$ is called commutative if $\*$ is commutative in $R$. ($\textbf{M}_4$)  
$(R, \cdot, * )$ is said to be a ring *with identity* if $\*$ has an identity in $R$.

### Examples
The set of even integers under addition and multiplication, as well as the set of $n \times n$ matrices under matrix addition and multiplication, are examples of fields.

## Fields
### Definition
A field $(F, +, \times)$ is a set $F$ equipped with two binary operations $+$ and $\times$ which satisfy the following properties:  

1. $F$ is an integral domain, *i.e.*,  
    (i) $F$ forms a commutative ring under $+$ and $\times$, *i.e.*, ($\textbf{A}_1-\textbf{A}_4, \textbf{M}_1-\textbf{M}_4$)  
    (ii) $\times$ has an identity in $F$ (the *multiplicative identity*), *i.e.*, $\exists 1 \in F$ such that $a \times  1 = 1 \times a = a$, $\forall a \in F$. ($\textbf{M}_5$)  
    (iii) $F$ has no zero divisors, *i.e.*, $ab = 0 \Rightarrow (a = 0) \lor (b = 0)$, $\forall a,b \in F$. ($\textbf{M}_6$)  
    
2. all $f \in F$ (except 0) have multiplicative inverses $f^{-1} \in F$ such that $ff^{-1} = f^{-1}f = 1$. ($\textbf{M}_7$)

### Examples
The sets of real numbers, rational numbers and complex numbers all form fields under addition and multiplication.

$Z_n$ is a commutative ring with identity (under addition and multiplication mod $n$), but not a field unless $n$ is prime (if $n$ has non-trivial factors $a, b$, then $ab = 0$ but $a \neq 0$ and $b \neq 0$, so there are zero divisors).

For prime $p$, the field $(Z_p, +_p, \cdot_p)$ is called a *Galois field* and also denoted as $GF(p)$.
