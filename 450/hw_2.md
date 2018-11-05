# CS450 HW 2

### Peter Dulworth

1. **[20pts]**

$T = \{\text{all rigid body transformations in 2D in homogeneous coordinates}\}$

Notation:
$$
T = \{t_i \mid
t_i=
\begin{pmatrix}
R_i & p_i \\
0 & 1 \\
\end{pmatrix}
\}
$$
Denote the group of rotation matrices with matrix multiplication as $G(R, \cdot)$.

Denote the group of translation vectors with vector addition as $G(p, +)$.

Want: $G(T, \cdot)$

**Closure**

We want to prove that $T$ is closed under matrix multiplication. i.e. $\forall t_1, t_2 \in T \implies (t_1 \cdot t_2) \in T$.

Choose arbitrary $t_1, t_2 \in T$. 

Then,
$$
t_1 \cdot t_2 = 
\begin{pmatrix} 
R_1 & p_1 \\
0 & 1 \\
\end{pmatrix}
\cdot
\begin{pmatrix} 
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}
\\=
\begin{pmatrix} 
(R_1R_2 + p_1 * 0) & (R_1p_2 + p_1 * 1) \\
(0 * R_2 + 1 * 0) & (0 * p_1 + 1 * 1) \\
\end{pmatrix}
\\=
\begin{pmatrix} 
(R_1 R_2) & (R_1 p_2 + p_1) \\
0 & 1 \\
\end{pmatrix}
$$
It is given that rotation matrices with matrix multiplication form a group. Thus, rotation matrices are closed under matrix multiplication and $R_1 \cdot R_2$ can be rewritten as a single rotation matrix $R_3$. Similiarly, we know that translation vectors and vector addition form a group. Thus, translation vectors are closed under vector addition and $R_1 * p_2 + p_1$ can be rewritten as a single translation vector $p_3$. Thus,
$$
t_1 \cdot t_2 = 
\begin{pmatrix}
R_3 & p_3 \\
0 & 1 \\
\end{pmatrix}
\in T
$$
**Associativity**

Want: $\forall t_1, t_2, t_3 \in T \implies (t_1 \cdot t_2) \cdot t_3 = t_1 \cdot (t_2 \cdot t_3) $.

Choose arbitrary $t_1, t_2, t_3 \in T$.

Then,
$$
(t_1 \cdot t_2) \cdot t_3 = 
(\begin{pmatrix}
R_1 & p_1 \\
0 & 1 \\
\end{pmatrix} 
\cdot
\begin{pmatrix}
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}
)
\cdot
\begin{pmatrix}
R_3 & p_3 \\
0 & 1 \\
\end{pmatrix}
\\=
\begin{pmatrix} 
(R_1 R_2) & (R_1 p_2 + p_1) \\
0 & 1 \\
\end{pmatrix}
\cdot
\begin{pmatrix}
R_3 & p_3 \\
0 & 1 \\
\end{pmatrix}
\\=
\begin{pmatrix}
(R_1 R_2) R_3 & (R_1 R_2) p_3 + (R_1 p_2 + p_1) \\
0 & 1 \\
\end{pmatrix}
$$
Working from the other direction we get:
$$
t_1 \cdot (t_2 \cdot t_3) = 
\begin{pmatrix}
R_1 & p_1 \\
0 & 1 \\
\end{pmatrix} 
\cdot
(
\begin{pmatrix}
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}
\cdot
\begin{pmatrix}
R_3 & p_3 \\
0 & 1 \\
\end{pmatrix}
)

\\=
\begin{pmatrix}
R_1 & p_1 \\
0 & 1 \\
\end{pmatrix}
\cdot
\begin{pmatrix} 
(R_2 R_3) & (R_2 p_3 + p_2) \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix}
R_1 (R_2 R_3) & R_1  (R_2 p_3 + p_2) + p_1\\
0 & 1 \\
\end{pmatrix}
$$
With the given assumptions about rotation matrix groups and translation vector groups we can see that $(R_1 R_2) R_3 = R_1(R_2 R_3)$ and  $R_1  (R_2 p_3 + p_2) + p_1 = (R_1R_2)p_3 + (R_1p_2 + p1)$.  Thus,
$$
(t_1 \cdot t_2) \cdot t_3 = t_1 \cdot (t_2 \cdot t_3) 
$$
**Identity**

NOTE: I am using $t_2$ instead of $t_1$ because it contrasts better with the $I$ subscript.

Want: $\exists I \in T$ such that $I*t_2=t_2*I=t_2 \forall t_2 \in T$. 

Let $I=
\begin{pmatrix}
R_I & p_I \\
0 & 1 \\
\end{pmatrix}$ where $R_I$is the identity of $G(R, \cdot)$ and $p_I$ is the identity of $G(p,+)$ .

That is, $R_I=\begin{pmatrix}1&0\\0&1\\\end{pmatrix}$ and $p_I=\begin{pmatrix}0\\0\\\end{pmatrix}$. 

Then for some arbitrary $t_2 \in T$
$$
I * t_2 = 
\begin{pmatrix}
R_I & p_I \\
0 & 1 \\
\end{pmatrix}
\cdot
\begin{pmatrix}
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_I R_2 & R_I p_2 + p_I \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_2 & R_I p_2 + p_I \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}

\\=t_2
$$
and
$$
t_2 * I = 
\begin{pmatrix}
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}
\cdot
\begin{pmatrix}
R_I & p_I \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_2 R_I & R_2 p_I + p_2 \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_2 & p_2 \\
0 & 1 \\
\end{pmatrix}

\\=t_2
$$
Thus, $I*t_2=t_2*I=t_2 \forall t_2 \in T$.

**Inverse**

Want: $\forall t_1 \exists t_2 \in T$ such that $t_1 * t_2 = t_2 * t_1 = I$.

Let $t_1=\begin{pmatrix}R_1 & p_1\\ 0 & 1\\\end{pmatrix}$ be an arbitrary element of $T$.

Consider $t_2=\begin{pmatrix}R_2 & p_2\\ 0 & 1\\\end{pmatrix}$ where $R_2$ is the inverse of $R_1$ in $G(R, \cdot)$ and $p_2$ is the inverse of $p_1$ in $G(p, +)$. Then,
$$
t_1 \cdot t_2 = 
\begin{pmatrix} 
R_1 R_2 & R_1 p_2 + p_1 \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_I & p_I \\
0 & 1 \\
\end{pmatrix}

\\=
\begin{pmatrix} 
R_2 R_1 & R_2 p_1 + p_2 \\
0 & 1 \\
\end{pmatrix}

\\= t_2 \cdot t_1 
$$
Thus $t_1 * t_2 = t_2 * t_1 = I$.  $\square$

2. **[10 pts]** 

   (a) **[5 pts]** 

   Let $N=\begin{pmatrix}0\\0\\1\\  \end{pmatrix}$ and $\theta = \frac{\pi}{2}$.  

   Then, 

   $q_1=q(N, \frac{\theta}{2}) = cos(\frac{\pi}{4}) + \begin{pmatrix}0\\0\\sin(\frac{\pi}{4})\end{pmatrix}
   = \frac{1}{\sqrt{2}} + \begin{pmatrix}0\\0\\\frac{1}{\sqrt2}\end{pmatrix} = \frac{1}{\sqrt2}+\frac{1}{\sqrt{2}}k$.

   (b) **[5 pts]** 


$$
   q_1 q_2 = (\frac{1}{\sqrt2}+\frac{1}{\sqrt{2}}k)(i)
   =(\frac{1}{\sqrt2}i)+(\frac{1}{\sqrt2}j)
$$


3. **[15 pts]**

   | Manipulator   | Description             | Dimension | Topology                            |
   | ------------- | ----------------------- | --------- | ----------------------------------- |
   | Manipulator 1 | 2 prismatic joints      | 2         | $\mathbb{R}^1\times\mathbb{R}^1$    |
   | Manipulator 2 | 3 revolute joints       | 3         | $ T^3$= $S^1 \times S^1 \times S^1$ |
   | Manipulator 3 | 2 revolute, 1 prismatic | 3         | $S^1\times S^1 \times \mathbb{R}^1$ |

4. [**30 pts**]

   a. topology: $S^1\times S^1\times S^1$, dimension =3 

5. [**15 pts**]

   If $A\cap B \neq \empty$ then $QA$ and $QB$ always overlap. Assuming that $A$ and $B$ overlap in at least one point, it follows that $QA$ and  $QB$ overlap at atleast one point because $A$ is a subset of $QA$ and $B$ is a subset of $QB$.

   If $A\cap B = \empty$ then $QA$ and $QB$ may overlap. Consider the example of two rectangular obstacles and a circular robot. Assume the rectangular obstacles have a gap between them of 1.0. Assume the width of the circular robot is 2.0. Consider the case when the robot is in the center of the gap. The robot would be in collision with both $A$ and $B$ causing their configuration space obstacles to overlap. 

6. [**10 pts**]

   topology: $(\mathbb{R}^3 \times SO(3))^5$ 

   dimension: $5*6=30$
