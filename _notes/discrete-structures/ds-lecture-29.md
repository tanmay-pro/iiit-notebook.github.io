---
date: 2020-11-24
title: DS Lecture 29
author: Abhinav S Menon
code: ma5.101
number: 29
---

## Code Generation by Parity Checks
Let $H = [P|I_r]$ be a parity-check matrix of the code $N(H)$ in canonical form, *i.e.*,
<div>
$$
H = \begin{bmatrix}
h_{11} & h_{12} & h_{13} & \cdots & h_{1k} & 1 & 0 & \cdots & 0 \\
h_{21} & h_{22} & h_{23} & \cdots & h_{2k} & 0 & 1 & \cdots & 0 \\
h_{31} & h_{32} & h_{33} & \cdots & h_{3k} & 0 & 0 & \cdots & 0 \\
\vdots & \vdots & \vdots & \ddots & \vdots & \vdots & \vdots & \ddots & \vdots \\
h_{r1} & h_{r2} & h_{r3} & \cdots & h_{rk} & 0 & 0 & \cdots & 1
\end{bmatrix},
$$
</div>

where $k = n - r$.

### Encoding
Since the rows of $P$ have $(n-r)$ elements, the code $N(H)$ has $2^{(n-r)}$ codewords, each of length $(n-r)$, *i.e.*, of length $k$. We want to send a message $x = \langle x_1 x_2 ...  x_k \rangle$ of length $k$. We will supplement it by $r$ error detection bits, which form the vector $e = \langle e_1 e_2 ...  e_r \rangle$. We will concatenate $x$ and $e$, forming $y = \langle y_1 y_2 ... y_n \rangle$, which belongs to $N(H)$, and transmit $y$. 

Clearly, $y_i = x_i$, $i = 1, 2, ..., k$.

We will define $e_i$ as $x_1h_{i1} \oplus x_2h_{i2} \oplus \cdots \oplus x_kh_{ik}$, the dot product of $x$ and the $i$-th row of $P$. This means that $e = x \cdot P^{T}$.

To show that $y \in N(H)$, consider $y \cdot H^{T}$, as shown below:

<div>
$$
\begin{bmatrix} x_1 & x_2 & \cdots & x_k & e_1 & e_2 & \cdots & e_r \end{bmatrix}
\begin{bmatrix}
h_{11} & h_{21} & \cdots & h_{r1} \\
h_{12} & h_{22} & \cdots & h_{r2} \\
\vdots & \vdots & \ddots & \vdots \\
h_{1k} & h_{2k} & \cdots & h_{rk} \\
1 & 0 & \cdots & 0
\\ 0 & 1 & \cdots & 0 \\
\vdots & \vdots & \ddots & \vdots \\
0 & 0 & \cdots & 1
\end{bmatrix}
$$
</div>

The $i$-th element in the product is clearly $(x_1h_{i1} \oplus x_2h_{i2} \oplus \cdots \oplus x_kh_{ik} \oplus e_{i})$.

But the first $k$ terms are simply $e_i$, so this is nothing but $(e_i \oplus e_i)$, which is $\textbf{0}_r$. This holds for all $i = 1, 2,..., r$.

Hence $y \in N(H)$.

Note that each $e_i$ occurs exactly once the product, *i.e.* $e_i$ occurs only in the $i$-th element of the product. This is because the $e_i$ are the last $r$ elements of $y$, and the identity matrix forms the last $r$ rows of $H^{T}$ (the last $r$ columns of $H$), so the $e_i$ get multiplied by 1 in the $i$-th element of the product, and by 0 in every other element. Hence, we see that keeping the identity matrix at the end of parity-check matrix is directly related to keeping the parity-check bits at the end of the message.

If $H$ is not in canonical form, then the identity columns are interspersed throughout $H$. The $e_i$ must therefore be in the corresponding positions in $y$; *i.e.*, if the identity matrix corresponds to the columns $i_1, i_2, ... , i_r$ of $H$ (in that order), then $y_{i_1}, y_{i_2}, ... , y_{i_r}$ are $e_1, e_2, ... , e_r$ respectively. The remaining $y_i$ will have the values of $x_i$ in order. Now, for $y \cdot H^{T} = 0$, the $i$-th element of the product contains $e_i$, added to the product of the $x_i$ with the non-identity rows in $H^{T}$. If we let $e_i$ be equal to this product, we see that $y_i$ is $(e_i \oplus e_i) = 0$, so $y \in N(H)$.

Below is an example of a non-canonical parity-check matrix for a code where $n = 7, k = 4, r = 3$.
<div>
$$
H = \begin{bmatrix}
\textbf{0} & 1 & \textbf{1} & 1 & \textbf{0} & 0 & 1 \\
\textbf{1} & 1 & \textbf{0} & 1 & \textbf{0} & 1 & 0 \\
\textbf{0} & 0 & \textbf{0} & 1 & \textbf{1} & 1 & 1 \end{bmatrix}
$$
</div>

Here, the third, first are fifth rows are the identity columns in that order. Hence, we have $y_3 = e_1, y_1 = e_2, y_5 = e_3$. The remaining bits of $y$ are equal to $x$ in order, so $\langle y_2 y_4 y_6 y_7 \rangle = \langle x_1 x_2 x_3 x_4 \rangle$. For each element of the product
<div>
$$
y \cdot H^{T} = \begin{bmatrix} e_2 & x_1 & e_1 & x_2 & e_3 & x_3 & x_4 \end{bmatrix}
\begin{bmatrix}
\textbf{0} & \textbf{1} & \textbf{0} \\
1 & 1 & 0 \\
\textbf{1} & \textbf{0} & \textbf{0} \\
1 & 1 & 1 \\
\textbf{0} & \textbf{0} & \textbf{1} \\
0 & 1 & 1 \\
1 & 0 & 1 \end{bmatrix}
$$
</div>
to be zero, as described above, we let $e_i = x_1h_{i2} \oplus x_2h_{i4} \oplus x_3h_{i6} \oplus x_4h_{i7}$; then the $i$-th element of the product reduces to $(e_i \oplus e_i)$. 

### Decoding
Suppose we have received the $n$-tuple $y$.  We need to find the true codeword $x$, which is the word in $C$ which is the least distance from $y$.

Let us consider the set
<div>
$$
E = \{(y \oplus c) | c \in C\} = y \oplus C,
$$
</div>

which is a coset of the set of all binary codes of length $n$, with respect to its subgroup $C$.

Any element $\epsilon \in E$ is, therefore, equal to $y \oplus c$ for some $c \in C$. Hence, $w(\epsilon) = w(y \oplus c) = H(y, c)$, *i.e.*, the distance of $y$ from $c$.

We know that the original message is $c'$ such that the distance of $y$ from $c'$ is minimum, which means we need to find $c'$ such that $H(y,c') = \min_{c \in C} H(y, c)$, which is simply $\min_{\epsilon \in E} w(\epsilon)$.

Suppose $\epsilon_m \in E$ is the least-weight element of $E$ (called the *coset leader* of $E$), and suppose $c_m \in C$ is such that $\epsilon_m = y \oplus c_m$.

Since $w(\epsilon_m)$ is minimum among all $\epsilon \in E$, $w(y \oplus c_m)$ is minimum among all $c \in C$; this implies that $H(y,c_m)$ is minimum among all $c \in C$. This means that $c_m$ is the original message we are looking for.

Adding $y$ on both sides to $\epsilon_m = y \oplus c_m$, we see that $c_m = \epsilon_m \oplus y$. So now we only need to find a way to identify $\epsilon_m$ from $y$.

We know $E$ to be a left coset of this set w.r.t. $C$. But we know that $\textbf{0}_r \in C$, so $y \in E$, *i.e.*, $E$ is the coset containing $y$. Therefore, $\epsilon_m$ is simply the coset leader of the coset containing $y$.

In conclusion, to decode $y$, we find the coset leader $\epsilon_m$ of the coset $E$ containing $y$. Then, the decoded message can be found as $x = y \oplus \epsilon_m$. [Note that this means that $y = x \oplus \epsilon_m$, so $\epsilon_m$ can be thought of an "error vector" that was added to $y$ during transmission.]

### Syndrome Decoding
This method is based largely on the procedure outlined in the preceding section, but there are some new details.

The *syndrome* of any $n$-tuple $y$ (which may or may not be in $C$) is defined as $S_y = y \cdot H^{T}$. For $y \in C$, we see that $S_y = 0$.

Before understanding the idea, we will prove a useful lemma.

**Lemma.** Two $n$-tuples belong to the same coset iff they have the same syndrome.

**Proof.** This is a double implication; we will prove both directions.

(i) Two $n$-tuples belong to the same coset $\Rightarrow$ they have the same syndrome.

Suppose $a$ and $b$ both belong to a coset $y \oplus C$, *i.e.*, $a = y \oplus c_1$ and $b = y \oplus c_2$ for some $c_1, c_2 \in C$.

We know that $S_a = a \cdot H_{T} = (y \oplus c_1) \cdot H^{T} = (y \cdot H^{T}) \oplus (c_1 \cdot H^{T}) = y \cdot H^{T}$ (since $c_1 \in C$).

Similarly, $S_b = b \cdot H_{T} = (y \oplus c_2) \cdot H^{T} = (y \cdot H^{T}) \oplus (c_2 \cdot H^{T}) = y \cdot H^{T}$ (since $c_2 \in C$).

Therefore, $a$ and $b$ have the same syndrome.

(ii) Two $n$-tuples have the same syndrome $\Rightarrow$ they belong to the same coset.

Suppose $a$ and $b$ have the same syndrome, *i.e.*, $a \cdot H^{T} = b \cdot H^{T}$. Bringing them both to the same side and applying the distributive law, we get $(a \oplus b) \cdot H^{T} = 0$.

However, this means that $(a \oplus b)$ is a member of $C$. Let $c = a \oplus b \in C$.

This implies that $a = b \oplus c$, so $a \in b \oplus C$. However, we know that $a \in a \oplus C$, which means that the cosets $(a \oplus C)$ and $(b \oplus C)$ are identical; *i.e.*, $a$ and $b$ belong to the same coset.

From this lemma, we see that the syndrome is a characteristic of a coset. Hence, we construct a table for the code beforehand, mapping the syndrome of each coset with its leader. When we receive a message $y$, we can compute its syndrome $S_y = y \cdot H^{T}$. By referring to the table, we can find the leader of the coset containing $y$, and directly find the original message.


## Example
Let us now consider a particular example – a Hamming code where $n = 7$, $k = 4$ and $r = 3$, *i.e.*, messages of length 4, plus 3 parity-check bits, can be transmitted. We will use the following parity-check matrix:
<div>
$$
H = \begin{bmatrix}
0 & 0 & 0 & 1 & 1 & 1 & 1 \\
0 & 1 & 1 & 0 & 0 & 1 & 1 \\
1 & 0 & 1 & 0 & 1 & 0 & 1
\end{bmatrix}
$$
</div>

Note that the $i$-th column of this matrix is simply the binary representation of $i$.

However, $H$ is not in canonical form; we can convert it to canonical form by rearranging the columns. Thus we find $P$.
<div>
$$
H' = \begin{bmatrix}
0 & 1 & 1 & 1 & 1 & 0 & 0 \\
1 & 0 & 1 & 1 & 0 & 1 & 0 \\
1 & 1 & 0 & 1 & 0 & 0 & 1
\end{bmatrix}
$$
</div>

Since no column is all zeroes, no two columns are identical, and $c_1 \oplus c_2 \oplus c_3 = \textbf{0}_3$, we can conclude that $d = 3$; *i.e.*, 1 error can be corrected.

Consider the message $x = \langle 1 0 1 0 \rangle$. Three more parity bits have to be added; we calculate them as mentioned above, using the formula $x \cdot P^{T}$:
<div>
$$
\begin{bmatrix} 1 & 0 & 1 & 0 \end{bmatrix}
\begin{bmatrix}
0 & 1 & 1 \\
1 & 0 & 1 \\
1 & 1 & 0 \\
1 & 1 & 1 \end{bmatrix}
= \begin{bmatrix} 1 & 0 & 1 \end{bmatrix}
$$
</div>

Hence, the message to be transmitted is $y = \langle 1 0 1 0 1 0 1 \rangle$. We can verify that it does, in fact, belong to $N(H)$:
<div>
$$
y \cdot H^{T} = \begin{bmatrix} 1 & 0 & 1 & 0 & 1 & 0 & 1 \end{bmatrix}
\begin{bmatrix}
0 & 1 & 1 \\
1 & 0 & 1 \\
1 & 1 & 0 \\
1 & 1 & 1 \\
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \end{bmatrix}
= \begin{bmatrix} 0 & 0 & 0 \end{bmatrix}
$$
</div>


Now, suppose the 3rd bit of $y$ was flipped, yielding $y' = \langle 1 0 0 0 1 0 1 \rangle$, the received message.

The syndrome table for single-bit errors is as follows:

| Syndrome | Error |
| :---: | :---: |
| $\langle 0 1 1 \rangle$ | $\langle 1 0 0 0 0 0 0 \rangle$ |
| $\langle 1 0 1 \rangle$ | $\langle 0 1 0 0 0 0 0 \rangle$ |
| $\langle 1 1 0 \rangle$ | $\langle 0 0 1 0 0 0 0 \rangle$ |
| $\langle 1 1 1 \rangle$ | $\langle 0 0 0 1 0 0 0 \rangle$ |
| $\langle 1 0 0 \rangle$ | $\langle 0 0 0 0 1 0 0 \rangle$ |
| $\langle 0 1 0 \rangle$ | $\langle 0 0 0 0 0 1 0 \rangle$ |
| $\langle 0 0 1 \rangle$ | $\langle 0 0 0 0 0 0 1 \rangle$ |

Now, we will calculate the syndrome of $y'$:
<div>
$$
y' \cdot H^{T} = \begin{bmatrix} 1 & 0 & 0 & 0 & 1 & 0 & 1 \end{bmatrix}
\begin{bmatrix}
0 & 1 & 1 \\
1 & 0 & 1 \\
1 & 1 & 0 \\
1 & 1 & 1 \\
1 & 0 & 0 \\
0 & 1 & 0 \\
0 & 0 & 1 \end{bmatrix}
= \begin{bmatrix} 1 & 1 & 0 \end{bmatrix}
$$
</div>

Referring to the syndrome table, we see that the corresponding error is $\langle 0 0 1 0 0 0 0 \rangle$. Hence, the original message $y$ was $\langle 1 0 0 0 1 0 1 \rangle \oplus \langle 0 0 1 0 0 0 0 \rangle = \langle 1 0 1 0 1 0 1 \rangle$, as required. Knowing that the last three bits are parity-check bits, we discard them to get the original message, $\langle 1 0 1 0 \rangle$.


**Note.** It is possible to construct $H$ so that the syndrome of a certain error's coset indicates the position of the error directly, *e.g.*, a syndrome of $\langle 1 1 0 \rangle$ indicates an error in the sixth bit. However , for this, $H$ cannot be in canonical form, which means the parity-checking bits cannot be gathered together at the end of the message; they must be interspersed within the message (as mentioned in the beginning). This method is carried out below on the same code.

First, we must construct $H$ so that the syndrome of any error's coset corresponds the position of the error. For this, we note that $w(\epsilon_i) = 1$ (single-error correction). If the $k$-th element of $\epsilon_i$ is 1 and the rest are 0, then $\epsilon_i \cdot H^{T}$ will simply produce the $k$-th column of $H$. This is the syndrome of the error coset containing (led by) $\epsilon_i$, so it should be the binary representation of $k$. Hence, the $k$-th column of $H$ should be the binary representation of $k$; this is the $H$ we considered first in the above example.
<div>
$$
H = \begin{bmatrix}
\textbf{0} & \textbf{0} & 0 & \textbf{1} & 1 & 1 & 1 \\
\textbf{0} & \textbf{1} & 1 & \textbf{0} & 0 & 1 & 1 \\
\textbf{1} & \textbf{0} & 1 & \textbf{0} & 1 & 0 & 1
\end{bmatrix}
$$
</div>

Now, we must consider which positions in $y$ are occupied by the parity bits, and which are occupied by the message bits. Further, we must see how to calculate the parity bits.

We note that the first, second, and fourth columns constitute the identity in reverse order, so $y_4, y_2, y_1$ are the parity-check bits $e_1, e_2, e_3$ respectively. Therefore, $\langle y_3 y_5 y_6 y_7 \rangle = \langle x_1 x_2 x_3 x_4 \rangle$. So, for $y$ to be in $N(H)$,
<div>
$$
y \cdot H^{T} = \begin{bmatrix} e_3 & e_2 & x_1 & e_1 & x_2 & x_4 & x_4 \end{bmatrix}
\begin{bmatrix}
0 & 0 & 1 \\
0 & 1 & 0 \\
0 & 1 & 1 \\
1 & 0 & 0 \\
1 & 0 & 1 \\
1 & 1 & 0 \\
1 & 1 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 \end{bmatrix}.
$$
</div>

Now, we can say that $e_i = x_1h_{i3} \oplus x_2h_{i5} \oplus x_3h_{i6} \oplus x_4h_{i7}$. This gives $\langle e_1 e_2 e_3 \rangle = \langle 1 0 1\rangle$. Putting these values, we get $y = \langle\textbf{10}1\textbf{1}010 \rangle$ (the parity bits are in bold). We can verify that this belongs to $N(H)$:
<div>
$$
y \cdot H^{T} = \begin{bmatrix} 1 & 0 & 1 & 1 & 0 & 1 & 0 \end{bmatrix}
\begin{bmatrix}
0 & 0 & 1 \\
0 & 1 & 0 \\
0 & 1 & 1 \\
1 & 0 & 0 \\
1 & 0 & 1 \\
1 & 1 & 0 \\
1 & 1 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 0 & 0 \end{bmatrix}.
$$
</div>

Again, suppose the third bit of $y$ was flipped, and we receive $y' = \langle 1001010 \rangle$. We calculate the syndrome of $y'$ as
<div>
$$
y' \cdot H^{T} = \begin{bmatrix} 1 & 0 & 0 & 1 & 0 & 1 & 0 \end{bmatrix}
\begin{bmatrix}
0 & 0 & 1 \\
0 & 1 & 0 \\
0 & 1 & 1 \\
1 & 0 & 0 \\
1 & 0 & 1 \\
1 & 1 & 0 \\
1 & 1 & 1 \end{bmatrix} = \begin{bmatrix} 0 & 1 & 1 \end{bmatrix},
$$
</div>

which is the binary representation of 3, as expected. Hence we flip the third bit to get $y = \langle 1 0 1 1 0 1 0 \rangle$ back from $y'$. Knowing that the first, second and fourth bits of $y$ are parity-check bits, we discard them to get the original message, $\langle 1 0 1 0 \rangle$.
