#450 Exam 1 Notes

## Lecture 1 - intro

A robot is a system that observes the state of the world and changes the state of the world.

Sense -> act -> plan -> sense -> ....

We will mostly work on planning.

## Lecture 2 - bug algorithms

$W=\mathbb{R}^2$or $\mathbb{R}^3$ depending on robot. Could be infinite or bounded.

Many algorithms assume global knowledge. Bug algorithms assume *only* local knowledge of the environment and a global goal.

Bug behaviors are simple:

- Follow a wall (L, R)
- Move in a straight line towards a goal

Bug1 & Bug2 assume esentially tactile sensing. Tangent bug deals with finite distance sensing.

Robot can:

-  perfect positioning: we know the position at all times

- known direction to goal: robot can measure distance $d(x,y)$ between pts $x$, $y$. 
- otherwise local sensing: walls/obstacles & encoders

### Bug 0 Algorithm

Notation: $q_{start}, q_{goal}$, $q^H_i$ $i$th hit point $q^L_i$ $i$th leave point.

A path is a sequence of hit/leave pairs bounded by $q_{start}$, $q_{goal}$.

```pseudocode
1. head towards goal
2. follow upstacles until you can head towards goal again
3. continue
```

### Bug 1 Algorithm - add some memory - (exhaustive algorithm)

```pseudocode
1. head towards goal
2. if an obstacle is encountered trace the entire obstacle and remember how close you get to the goal
3. return to the closest point by wall following and continue
```

Path length bounds: lower = $D$, upper = $D + 1.5\Sigma P_i$ , $D=$ distance from start to goal, $P_i=$ perimeter of the $i$th obstacle.

An algorithm is complete if, in finite time, it finds a path if such a path exists or terminates with failure if it does not.

Bug 1 is complete (proof by contradiction) .

### Bug 2 Algorithm - greedy

Call the line from $q_{start}$ to $q_{goal}$ the $m$-line. 

```pseudocode
1. head towards goal on m-line
2. if an obstacle is in the way, follow it until you hit the m-line again CLOSER TO THE GOAL
3. leave the obstacle and continue towards the goal
```

In many cases Bug2 will out perform Bug1 but Bug1 has more predictable performance overall.

Lower bound: $D$, Upper bound: $D + .5\Sigma n_i P_i$ $n_i=$ # of m-line intersections with obstacle $i$.

### Tangent Bug

Uses range sensor.

## Lecture 4

#### Homogenous Coordinates

$(x,y,w) \rightarrow (x / w, y / w) ,  $$w\neq0$

Thus, $c * (x,y,w) = (cx, cy, cw) \rightarrow (cx/cw, cy/cw)=(x/y,y/w)$

i.e. $c * (x,y,w) = (x, y, w)$

Note:

$(x,y,0)\rightarrow \infty$ in the direction of $(x,y)$ 

$(0,0,0)$ not allowed because gives $\frac{0}{0}$ 

At least one of x,y,w is always nonzero

Successive transformations can be easily computed in homogeneous
coordinates

#### Rigid Body Transformations

Rotation about the origin:

$R(\theta)=\begin{pmatrix}cos(\theta) & -sin(\theta) \\ sin(\theta) & cos(\theta)\end{pmatrix}$

Same transformation in homogeneous coordinates:

$\begin{pmatrix}R(\theta) & t \\ 0 & 1\end{pmatrix} \begin{pmatrix}v \\  1\end{pmatrix}$

Note: successive rotations+translations happen in revers of multiplication order. i.e. from right to left. rotate-2, translate-2, rotate-1, translate-1

Rotation about an arbitrary point $p$: 

$\begin{pmatrix}I & -p \\  0 & 1\end{pmatrix} \begin{pmatrix}R(\theta) & 0 \\ 0 & 1\end{pmatrix} \begin{pmatrix}I & p \\  0 & 1\end{pmatrix}$

#### Rotations in 3D

- x-yaw, y-pitch, z-roll

- Euler: any rotation can be represented by no more than three rotations about coordinate axes, - no two successive rotations are about the same axis

- not commutative 

general transformation in homogeneous coordinates (4x4 matrix):

$\begin{pmatrix}R(\alpha,\beta,\gamma) & t \\ 0 & 1\end{pmatrix}$

rotation about arbitrary axis k: Transform to the (x,y,z) coordinate axis, apply the
rotation, and then reverse the transformations.

$SO(n) = \{R\in\mathbb{R}^n \times \mathbb{R}^n : RR^t = I, det(R)=1\}$

## Lecture 5

problem: loss of DOF in 3D space

- Can occur when using rotation matrices for rotations

### Quaternions

- generalization of complex numbers in 4D space

**Definition**: $q=a+bi+cj+dk$ (sum of a scalar and a vector) 

**Conjugate**: $q^*=a-bi-cj-dk$ (sum of a scalar and a vector) 

**Multiplication of Basis Vectors:**

$i^2 = j^2 = k^2 = -1$

$ij = k, jk=i, ki=j$

$ji=-k, kj=-i,ik=-j$ 

**Multiplication of arbitrary vectors:**

$(a + v)(c + w)=(ac-vw)+(cv+aw+v\times w)$

**Properties of quaternion multiplication:**

- associative 
- not commutative
- distributes through addition

**Unit Quaternion:**

$q(N, \theta)=cos(\theta) + sin(\theta)N$

N = unit vector

**Rotation of Vectors from Quaternion Multiplication**

$S_{q(N,\theta/2)}(v)=q(N,\theta/2) \cdot v \cdot q^*(N,\theta/2)$

where $N$ = axis of rotation, $\theta$  = angle of rotation.

Note: $q^*(N,\theta/2)=cos(\theta/2)-sin(\theta/2)N$

**Advantages of Quaternions**

- Quaternion approach does not suffer from gimbal lock problem 
- Concatenating rotations is computationally faster and numerically more stable 
- Extracting the angle and axis of rotation is simpler 
- Interpolation is more straightforward 

### Forward Kinematics

Position of the end effector in terms of joint
angles.

###Inverse Kinematics

Joint angles in terms of the position of the
end effector

![Screen Shot 2018-10-03 at 8.14.14 PM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-03 at 8.14.14 PM.png)

### Configuration Space

The configuration of a robot is a complete specification of the position of every
point of that system. 

The configuration space, or **$C$-space,** of the robot system is the space of all possible configurations of the system. 

Defrees of freedom (DOF): Dim($C$-Space). Minimum number of parameters needed to represent a configuration.

The free space $F$ is the set of free configurations.

The Minkowski sum of two convex polygons $P$ and $Q$ of $m$ and $n$ vertices respectively is a convex polygon $P \oplus Q$ of $m + n$ vertices. 

**Example** A rigid body in 3D with rotations + translation: 6 degrees of freedom. x,y,z,roll,pitch,yaw.

**Example** A rigid body in 2D with rotations + translation: 3 degrees of freedom. x,y,roll.

**Example** A rigid body in 4D with rotations + translation: 10 degrees of freedom. 4 linear, 6 angles.

General
$$
dof = \sum(\text{freedoms of points}) - \#\text{independent constraints}
$$
Rigid bodies:
$$
dof = \sum(\text{freedoms of bodies}) - \#\text{independent constraints}
$$

### Topology

"shape of space": e.g. sphere vs. plane

fundamental property that is not affected by choice of coordinates

two spaces are topologically quivlanet if one can be smoothly deformed to the other without cutting or gluing.

Topologically distinct 1-dim spaces:

circle, line, closed interval of line

Topologically distinct 2-dim spaces:

plane, surface of sphere, surphace of torus, surphas of cylinder 

![Screen Shot 2018-10-01 at 8.35.12 PM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-01 at 8.35.12 PM.png)





### Important Topological Spaces

| Group                                                        | Name                                       | Dim  | Matrix Representation                                        | Robot |
| ------------------------------------------------------------ | ------------------------------------------ | ---- | ------------------------------------------------------------ | ----- |
| $\mathbb{R}$                                                 | Real number line                           | 1    |                                                              |       |
| $\mathbb{R}^n$                                               | n-dimensional cartesian space              | n    |                                                              |       |
| $S^1$                                                        | boundary of circle in 2D                   | 1    |                                                              |       |
| $S^2$                                                        | surface of sphere in 3D                    | 2    |                                                              |       |
| $SO(2)$                                                      | set of 2D orientations                     | 1    | 2D rotation matrix                                           |       |
| $SO(3)$                                                      | set of 3D orientations                     | 3    | 3D rotation matrix                                           |       |
| $SE(2)$                                                      | set of rigid 2D translations and rotations | 3    | Linear transformation on homogeneous 3-vectors. $\mathbb{R}^2 \times S^1$ |       |
| $SE(3)$                                                      | set of rigid 3D translations and rotations | 6    | linear transformation on homogeneous 4-vectors $\mathbb{R}^3 \times S^1 \times S^1 \times S^1$ |       |
| $S^1 \times \cdot\cdot\cdot\times S^1 (\text{n times}) = T^n$ | n-dimensional torus                        | n    |                                                              |       |
| $S^1 \times \cdot\cdot\cdot\times S^1 (\text{n times}) \neq S^n$ | n-dimensional sphere in $\mathbb{R}^{n+1}$ | n    |                                                              |       |
| $S^1 \times S^1 \times S^1 \neq SO(3)$                       |                                            |      |                                                              |       |
| $SE(2) \neq \mathbb(R)^3$                                    |                                            |      |                                                              |       |
| $SE(3)\neq\mathbb{R}^6$                                      |                                            |      |                                                              |       |
|                                                              |                                            |      |                                                              |       |

| Type of Robot                                      | Representation of $C$                                        |
| -------------------------------------------------- | ------------------------------------------------------------ |
| Mobile robot translating in the plane              | $\mathbb{R}^2$                                               |
| Mobile robot translating and rotating in the plane | $\mathbb{R}^2 \times S^1$ or $SE(2)$                         |
| Rigid Body translating in 3 space                  | $\mathbb{R}^3$                                               |
| A spacecraft                                       | $\mathbb{R}^3 \times S^1 \times S^1 \times S^1$ or $SE(3)$ or $\mathbb{R}^3 \times SO(3)$ |
| n-joint revolute arm                               | $T^n = S^1 \times \cdot\cdot\cdot \times S^1$                |
| A planar mobile robot with an attached n-joint arm | $\mathbb{R}^2 \times S^1 \times T^n$                         |

### Dimension of common manifolds

| Dimension | Manifold                         |
| --------- | -------------------------------- |
| 1         | $\mathbb{R}^1$, $SO(2)$          |
| 2         | $\mathbb{R}^2$, $S^2$and $T^2$   |
| 3         | $\mathbb{R}^3$, $SE(2)$, $SO(3)$ |
| 6         | $\mathbb{R}^6$, $SE(3)$, $T^6$   |

## PRM

1. sample random states (keep free states)
2. compute edges by local planner
3. connect S and G to roadmap, perform graph search

```pseudocode
1. add q_init and q_goal to roadmap vertex set V // initializaion
2. repeat several times							// sampling
	if isCollisionFree(q = sample()) = true
		add q to V
3. for each pair of neighboring samples in VxV	//connect samples
		generate a local path between them and if it is collision free add to roadmap edge set E
4. Graph search (V, E)
```

**Advantages of PRM**

- computationally efficient
- solves high dimensional problems (hundreds of dofs)
- easy to implement
- many applications
- good when trying to solve multiple queries 

**Disadvantages**

- not complete, maybe too much for a single query 

However, it does offer *probabilistic completeness*. When a solution exists, a probabilistically complete planner finds a solution with probability as time goes to infinity. 

i.e. gets more and more likely to find solution as time increases.

### Path Smoothing

```
for several times do
	select i and j uniformly at random from 1,2,...,n
	attempt to directly connect qi to qj
	if successful, remove the in-between nodes, i.e., qi +1 , . . . , qj
OR
for several times do
	select i and j uniformly at random from 1,2,...,n
	generate collision free sample q
	attempt to connect qi to qj through q
	if successful, replace the in-between nodes qi +1 , . . . , qj by q
```

Edges between neighboring nodes are more likely to be collision
free than edges between far away nodes. HOWEVER efficiency deteriorates rapidly not much better than brute force. Instead compute APPROXIMATE NNs.

### Lazy PRM

Collision checking is expensive. generate samples and roadmap without collision checking. then build paths and check the nodes in the paths.

### Narrow Passages

Approaches:

1. Sample from a Gaussian distribution biased near the obstacles (qa, r, qb) only keep if 1 of qa, qb is outside obstacle and other is inside. small std dev -> Require a lot of samples to generate sufficient number of surviving samples. large std dev opposite
2. Move samples in collision outside obstacle boundary
3. bridge (sample 2 obstacles, create path, take free point half way between)
4. importance sampling: Identify roadmap nodes that lie in regions that are hard to connect. sample more in these regions.

## Random Tree Planners

1. grow random tree from start
2. occasionally attempt to connect tree with goal
3. repeat until goal is connected to tree

**RRT** - (rapidly-exploring random tree) Pull the tree toward random samples in the configuration space

randomly sample the c-space, build path from closest point in tree (could have collisions).  take the first "step size" of the path and add to tree.

**EST** - (expansive space tree) Push the tree frontier in the free configuration space

**Bi-Trees** - Grow two trees, rooted at qinit and qgoal, towards each other

- Bi-directional trees improve computational efficiency compared to a single tree
- Growth slows down significantly later than when using a single tree
- Different tree planners can be used to grow each of the trees

PRM provides global sampling of c-space. tree planners provide fast local.

**SRT** - sampling based roadmap of trees. combines advantages of local, global

```pseudocode
CreateTreesInRoadmap // grow trees rooted at collision free configs (global)
SelectWhichTreesToConnect // k-nearest + random
ConnectTreesInRoadmap // bidirectional tree planner to connect
SolveQuery // tree planners from start and goal to connect to roadmap
```

**Separating Axis Theorem**

```pseudocode
For each face of each polytope:
	Project polytopes onto a line orthogonal to the face
	If projections do not overlap, then there exists a separating axis (and 		polytopes do not intersect)
```

Cost of collision checking a BVH = number of BV overlap checks * cost of each check + number of exact intersection checks * cost of each

```pseudocode
<- faster intersection checks <-
spheres , AABBs, OBBs, Convex Hulls
 -> fewer intersection checks ->
```

![Screen Shot 2018-10-04 at 10.50.57 AM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-04 at 10.50.57 AM.png)

![Screen Shot 2018-10-04 at 11.15.21 AM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-04 at 11.15.21 AM.png)

![Screen Shot 2018-10-04 at 11.15.25 AM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-04 at 11.15.25 AM.png)

![Screen Shot 2018-10-04 at 11.15.34 AM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-04 at 11.15.34 AM.png)

![Screen Shot 2018-10-04 at 11.16.36 AM](/Users/Peter/Desktop/comp450/Screen Shot 2018-10-04 at 11.16.36 AM.png)

