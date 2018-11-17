# 450 Exam 2 Notes

#### Content:

- [x] **10/2 — Lecture 13 — Analysis of Planners**
- [x] PRM: Ch. 7.4
- [x] Path planning in expansive cfg spaces ftp://cs.stanford.edu/cs/robotics/dyhsu/papers/icra97.pdf
- [x] **10/4, 10/11 — Lecture 14, 15 — Kinodynamic Planning**
- [x] LaValle's Book: 13.1.1 - 13.1.2 and 13.2 - 13.2.3
- [x] PRM: 12.5.6 and skim 12.2
- [x] A Fast Path Planner for a Car Like Indoor Mobile Robot https://www.aaai.org/Papers/AAAI/1991/AAAI91-103.pdf
- [x] Randomized Kinodynamic Planning https://www.clear.rice.edu/comp450/papers/lavalle_kuffner_ijrr_2001.pdf
- [x] **10/16 — Lecture 16 — PDST Planner** 
- [ ] Motion Planning in the Presence of Drift http://www.roboticsproceedings.org/rss01/p31.pdf
- [x] **10/18 — Lecture 17 — Potential Field, Cell Decomposition, and Roadmaps**
- [ ] PRM: Chapters 4.1-4.5, skim 4.6, 4.7, 6.1
- [x] Brush Fire Algorithm
- [x] **10/23 — Lecture 18 — Optimization**
- [x] Incremental Sampling-based Algorithms for Optimal Motion Planning [only read the main algorithm] https://arxiv.org/pdf/1005.0416.pdf
- [x] **10/30 — Lecture 19 — Task and Motion Planning**
- [x] Shakey https://www.youtube.com/watch?v=qXdn6ynwpiI
- [ ] LaValle Ch.2, Discrete Planning
- [ ] Artificial Intelligence: Ch.7, Ch.10, Ch11 Read only Hierarchical Planning
- [x] **11/1 — Lecture 20 — Task and Motion Planning II**
- [ ] Incremental Task and Motion Planning http://www.roboticsproceedings.org/rss12/p02.pdf
- [x] **11/6 — Lecture 21 — Planning Under Uncertainty**
- [ ] Stochastic Motion Roadmap: https://robotics.cs.unc.edu/publications/Alterovitz2007_RSS.pdf
- [ ] Artificial Intelligence: Ch. 17, Ch. 1- Ch. 2.2
- [x] **11/8 — Lecture 22 — Multi Robot Planning**
- [x] Deadlock free and collision free coordination of two robot manipulators: http://people.csail.mit.edu/tlp/publications/coordination-o-donnell.pdf
- [x] using a PRM planner to compare centralized and decoupled planning for a multi-robot systems: http://ai.stanford.edu/~latombe/papers/icra02-gil/final.pdf
- [x] **Review Project 3**
- [x] **Review Project 4**



## Analysis of Planners

PRM is probabalistically complete. i.e. the probability of failure goes to zero as the number of samples used to construct the roadmap goes to infinity.

Tradeoff: Sacrifice completeness for speed

Complete planner is exponential in the number of degrees of the configuration space.

PRM is good for multiple path planning queries. Particularly suitable for problems where multiple path-planning queries have to be answered in the same static environment.

If you have higher order ODE's you can collapse them down to first order BUT it comes at the cost of increasing the dimension.

**Visibility** all configurations in free space that can be seen by a free configuration $p$.

<u>$\epsilon$-goodness</u>: Every free configurations "sees" at least an $\epsilon$ fraction of the free space with $\epsilon \in [0,1]$.

<u>$\beta$-lookout of a subspace $S$</u>: Subset of points in $S$ that can see a $\beta$ fraction of $F  \backslash S$ with $\beta \in (0,1].$ 

e.g. the $0.4$-lookout of some subspace $S$ is the subset of $S$ that can see $0.4$ of everything outside of $S$.

<u>$(\epsilon, \alpha, \beta)$-expansive</u>: The free space $F$ is $(\epsilon, \alpha, \beta)$ expansive if 

- $F$ is $\epsilon$ good.
- For each subspace $S$ of $F$, its $\beta$ lookout is at least an $\alpha$ fraction of $S$.

Bigger $\epsilon, \alpha, \beta$ implies easier to construct a roadmap with good connectvity / coverage.

As $\epsilon, \alpha, \beta$ decrease, the number of milestones needs to be increased (to maintain good connectivity).

If $C$-space is expansive then a roadmap can be constructed efficiently with good connectivity and coverage. 

Limitation on implementation: 

- No theoretical guidance about stopping time.
- A planner stops when either a path is found or Max steps have been taken.

**New Algorithm For Single Queries**

This algorithm tries to sample only the portion of the cfg-space that is relevant to the current query, avoiding the cost of precomputing a roadmap for the entire configuration space. Thus, it is well-suited for problems where a single query is submitted for a given environment.



## Kinodynamic Planning

- Geometric constraints are generally not sufficient to adequately express robot motions.
- Constraints on velocity, forces, torques, accelerations are needed for better representations. 
- Underactuated systems have fewer controls than dof.

Parametric velocity constraints express velocities that are allowed and are of the form
$$
q\prime = f(q, u)
$$
where $f(q, u)$ is some function $f: Q \times U \rightarrow Q\prime$ that expresses a set of differential equations.

$u$ is an input control.

**Two Motion-planning methods for systems with differential constraints:**

1. **decoupled approach**: compute geometric solution ignoring differential constraints $\rightarrow$ transform into a trajectory that satisfies constraints

2. **sampling based**: take differential constraints into account while planning

#### A Simple Car

Car Configuration: $q = (x, y, \theta) \in \mathbb{R} \times S^1$.

Body Frame: origin is at the center of rear axel. x-axis points along main axis of the car.

Velocity: $s$

Steering angle: $\phi$

Express car motions as a set of differential equations:
$$
x\prime = f_1(x, y, \theta, s, \phi)\\
y\prime = f_2(x, y, \theta, s, \phi)\\
\theta\prime = f_3(x, y, \theta, s, \phi)\\
$$
In a small time interval $\Delta t$, the car must move approximately in the direction that the rear wheels are pointing.

In the limit, as $\Delta t \rightarrow 0$, then $\frac{dy}{dx}=tan\theta$, i.e. $-x\prime sin \theta + y\prime cos \theta = 0$.

Solution is of the form $x\prime = scos\theta$ and $y\prime = ssin\theta$

If the steering angle is fixed at $\phi$, the car travels in a circular motion, in which the radius of the circle is $\rho$. 

So, we control the car by setting the speed and the steering angle. i.e. $u = (u_s, u_{\phi})=(s, \phi)$

Equations of motion (L is distance from front to rear axels:
$$
x\prime = u_s cos \theta \\
y\prime = u_s sin \theta \\
\theta \prime = \frac{u_s}{L} tan u_{\phi}\\
$$
**Reeds-Shepp Car Constraints:**

$u_s \in \{-1, 0, 1\}$ i.e. reverse, park, forward

$u_{\phi}$ same as standard simple car

**Dubins Car**

$u_s \in \{0, 1\}$ i.e. park, forward 

$u_{\phi}$ same as standard simple car

#### Differential Drive

Input Controls: $u = (u_l, u_r)$ for angular velocity of left and right wheels respectively.

Equations of Motion: (r = radius, L = distance between wheels)
$$
x\prime = \frac{r}{2}(u_l + u_r) cos \theta \\ 
y\prime = \frac{r}{2}(u_l + u_r) sin \theta \\ 
\theta \prime = \frac{r}{L}(u_r - u_l)
$$
Alternatively:

$u_\omega = (u_l + u_r) / 2 $ and $u_\psi = (u_r - u_l) $ 
$$
x\prime = ru_\omega cos \theta \\ 
y\prime = ru_\omega sin \theta \\ 
\theta \prime = \frac{ru_\psi}{L}
$$

#### Two Models for a Car

**Kinematic (First Order) Model**

State $s=(x,y,\theta)\in\mathbb{R}^2\times S^1$. Position, Orientation.

Control $u=(u_s,u_\phi)\in \mathbb{R}^2.$ Translational velocity, steering angle.

Motion equations $s\prime = f(s, u)$ where
$$
s\prime = \begin{bmatrix}x\prime\\y\prime\\\theta\prime \end{bmatrix}
= \begin{bmatrix}u_s cos\theta \\ u_s sin\theta \\ \frac{u_s}{L} tan u_\phi \end{bmatrix}
$$
**Dynamics (Second Order) Model**

State $s=(x,y,\theta,s,\phi)\in \mathbb{R}^2 \times S^1 \times \mathbb{R} \times \mathbb{R}.$ Position, orientation, translational velocity, steering angle.

Control $u=(u_1, u_2)\in \mathbb{R}^2$. Translational acceleration, steering rotational velocity.

Motion equations $s\prime = f(s, u)$ where 
$$
s\prime = \begin{bmatrix}x\prime\\y\prime\\\theta\prime \\ s\prime \\ \phi\end{bmatrix}
= \begin{bmatrix}s cos\theta \\ s sin\theta \\ \frac{s}{L} tan \phi \\ u_1 \\ u_2 \\\end{bmatrix}
$$

#### Two Models for Differential Drive

**Kinematic (First Order) Model**

State $s = (x, y, \theta) \in SE2$.

Control $u = (u_l, u_r)\in \mathbb{R}^2$. Angular Velocities.

**Dynamics (Second Order) Model**

State $s = (x, y, \theta, s_l, s_r) \in SE2 \times \mathbb{R} \times \mathbb{R}$. Position, orientation, angular velocities.

Control $u = (u_1, u_2) \in \mathbb{R}^2.$ Angular accelerations.

#### Two Models for Unicycle

**Kinematic (First Order) Model**

State $s=(x,y,\theta)\in SE2$.

Control $u=(u_\sigma, u_\omega)$ where $u_\sigma$ is translational velocity and $u_\omega$ is rotational velocity.

**Dynamics (Second Order) Model**

State $s=(x,y,\theta, \sigma, \omega)$

Control $u = (u_1, u_2)$. Translational acceleration, rotational acceleration.

#### Motion Planning for Kinodynamic systems

How do we actually get the motions? Apply the input controls and integrate the equations of motion to get a state.

Two options:

- Roadmap based planners (PRM)
- Tree based planners (RRT)

#### Roadmap with Differential Constraints

need to find control function to get from one state to another. cannot always be solved analytically and numerical solutions increase computational cost. this is called the <u>two-point boundary value problem</u>. 

**NOTE**: PRM is very hard to use for dynamics. RRT is better. RRT you jsut have to go TOWARDS the goal. PRM you need to connect two points which is very hard.

#### Tree with Differential Constraints

**RRT** 

```
r = sample random state
n = find nearest state in tree to random state
generate trajectory from n to r
check if sub trajectory is valid given a step size
add edge from n to the end of the sub trajectory
```

why doesn't this create the same problem as PRM?

instead of computing a trajectory from "near" to "rand" compute a trajectory that starts at "near" and extends toward "rand".

**Two Approaches**

1. Extend trajectory according to random control
2. find the best out of many random controls 



## PDST Planner

In many control planning problems (as you have seen in Project 4), the distance metric may not be useful for guiding expansion. *RRT* heavily relies on the distance metric to guide expansion. *PDST* does not use a distance metric, and as such *PDST* can be more effective planning for robots with complicated dynamics.

*PDST* is a tree-based motion planner that attempts to detect the less explored area of the space through the use of a binary space partition of a projection of the state space. Exploration is biased towards large cells with few path segments. Unlike most tree-based planners which expand from a randomly select endpoint of a path segment, [PDST](http://ompl.kavrakilab.org/classompl_1_1control_1_1PDST.html)expands from a randomly selected point along a deterministically selected path segment. Because of this, it is recommended to increase the min. and max. control duration. If no projection is set, the planner will attempt to use the default projection associated to the state space.

**Real World Car**

A real world car is even more compled. Turning is controlled by steering. Car motion is determined by tire contact. Skidding, slip and other behavior.

**Simulated Car** 

3D rigid body dynamics. Car consists of body and four wheels. Wheels form friction contacts. Wheel torques are bounded.

**Physical System Model**

(state $q_t$, control $u_t$) $\rightarrow$ PHYSICS SIMULATOR $F(q_t, u_t)=q_{t+1}$ $\rightarrow$ state $q_{t+1}$

- time is taken to be discrete. the simulator is assumed to be discrete.

**Physical System Planning**

Given an initial state $q_0$ and a goal set $G \subset Q$ compute a sequence $u_0,...u_N$ such that $F(q_i, u_i)=q_{i+1}$ and $g_{N+1}\in G$.

**Why is this hard?**

- State space ie enourmous, continuous, and hard to discretize.
- solution paths are usually "deep"
- there are many solutions

### A new Planner PDST

> Path-Directed Subdivision Tree Exploration (PDST-EXPLORE)

- does not use connection primitives. does not use proximity queries. greedy coverage optimization. methodical expansion.

Select, propagate, subdivide, repeat.

Data: $S=\{\gamma_1,...,\gamma_n\}$ the sampled set of path segments. $C = \{C_1,...,C_k\}$ a partition of the state space into cells. propagate($\gamma$) = $\gamma\prime$. subdivide($C$) = $(C_L,C_R)$. $\mu(C)$ the volumne of cell $C$.

**<u>Every path segment is contained in exactly one cell.</u>**

**Priority and Score**

Every sample has an integer priority($\gamma$) and an integer score($\gamma$).

$score(\gamma)=\frac{\text{priotity}(\gamma)}{\mu(C)}$ where $C$ is the cell that contains $\gamma$. i.e. the higher the volume of the cell, the lower the score of the segment (minimal score is better).

**Select and Propagate**

1. select $\gamma_s$ such that score is minimal
2. $\gamma$ = propagate($\gamma_s$)
3. priority($\gamma_s$) = $2*$priority($\gamma_s$)+1
4. priority($\gamma$) = iteration
5. add $\gamma$ to S

**Subdivide**

1. Select $\gamma_s$ and propagate
2. let $C_s$ be the cell that contained $\gamma_s$
3. subdivide($C_s$) = $(C_L, C_R)$.
4. Remove $C_s$ from $C$.
5. Add $C_L$ and $C_R$ to $C$.

**PDST Is Probabilistically Complete**



## Potential Fields, Cell Decompositions and Roadmap Methods

Continuous representation $\rightarrow$ discretization $\rightarrow$ graph searching

1. **Potential Field**

   Define a function over the free space that has a global minimum at the goal configuration and use gradient descent.

2. **Cell Decomposition**

   Decompose the free space into simple cells and represent the connectivity of the free space by the adjacency graph of these cells.

3. **Roadmap**

   Represent the connectivity of the free space by a network of 1-D curves.

#### 1. Potential Field Methods

- initially proposed for real time collision avoidance.

Local-minimum issue

- alternate descents and random walks
- use local minimum free potential function

**Best-first search algorithm**

1. place regular grid $G$ over space
2. search $G$ using best-first search algorithm with potential as heuristic function

#### 2. Cell-Decomposition Methods

Two Classes of Methods

1. Exact cell decomposition

   The free space $F$ is represented by a collection of non-overlapping cells whose union is exactly $F$.

   e.g. trapezoidal decomposition.

2. Approximate cell decomposition

   $F$ is represented by a collection of non-overlapping cells whose union is contained in $F$. 

   e.g. quadtree, octree, $2^n$ - tree

**Trapezoidal Decomposition** - exact method

extend a line vertical from every vertex (in the outline of the graph and the vertics of the obstacles).

connect a line from the mid point of every vertical line to the midpoint of its neighboring vertical line.

planar sweep: O(nlogn) time. O(n) space

**Octree Decomposition** - approximate

3 types of cells: "empty, mixed, full"

1. compute cell decomposition to some resolution
2. identify start and goal cells
3. search for sequence of empty / mixed cells between start and goal
4. if no sequence -> exit with NO PATH
5. if sequence of empty cells -> exit with SOLUTION
6. if resolution threshold achieved -> exit with FAILURE
7. decompose further the mixed cells
8. return to 2

#### 3. Roadmap Methods

**Visibility Graph** - can produce shortest paths in 2-D configuration spaces

note this can work but you have to look at all pairs of vertices!!! yikes

reduced visability graph eliminates concave obstacle vertices 

generalized reduced visability graphs

**Veroni Diagram**

generates paths that maximizes clearance. $O(nlogn)$ time $O(n)$ space.

**Silhouette**

First complete general method that applies to spaces of any dimension and is singly exponential in # of dimensions.

**Completeness of Planner**

visability graph, voronoi diagram, exact cell decomposition, navigation function provide complete planners

weaker notions of completeness: 

- resolution completeness - discretizes the space and returns a path whenever one exists in thie representation (e.g. potential field with best-first search)
- probabilistic completeness (e.g. potential field with random walks)

**Preprocessing / Query Processing**

- Preprocessing: compute visability graph, voronoi diagram, cell decomposition, navigation function
- Query processing: connect start / goal configurations to visability graph, voronoi diagram, identify start / goal cell / graph search.



## Optimization

Find a feasible path that also minimizes the cost function over the path. Example cost functions: path length, work integral, clearance, end-effector movement, etc...

### Almost Surely Asymptotically Optimal Planners

> RRT*, RRT#, FMT*, Informed-RRT*, BIT*

- **Probabilistically Complete** $\rightarrow$ given "long enough", with probability 1, a feasible solution is found, if one exists.
- **Almost Surely Asymptotically Optimal** $\rightarrow$ given "long enough", with probability 1, an optimal solution is found, if one exists.



#### RRT*

RRT* builds a solution an iteratively improves on it as time goes on. RRT* is almost-surely-asymptotically-optimal. i.e. given enough time it will find an optimal solution if one exists.

```
while not timed out
	
	sample random node, x_rand
	find nearest node to x_rand in the tree, call it x_nearest
	propagate to some state x_new from x_nearest in the direction of x_rand
	
	if x_nearest -> x_new collision free
		
        for all nodes in T within some fixed radius of x_new
        	find minimum cost node using (cost + heuristic)
        	
        add (x_min, x_new) to T
        
        update all nodes in radius to have parent w/ minimum cost		
```

RRT commits to a solution, running it longer won't help. Running RRT* longer will improve the solution until the optimal solution is found.

<u>RRT* is a cool result. BUT it has AWFUL convergence.</u>

**Lifelong Planning A*** 

>  Designed for edge additions / deletions and changing weights.

Saves and reuses information from past searches.



#### RRT#

> RRT* with LPA* ensuring that the tree is always consistent

LPA* -> you only have to rewire a small fraction of all nodes in the graph (I'm not really sure how this is different than RRT* but it is).

<u>Much more efficience than RRT*.</u> 



#### FMT*

- Samples all points upfront. Uses fast marching to expand the frontier similar to Djikstra's.
- Without obstacles, result is identical to PRM*.
- With obstacles, it has a very small change of making suboptimal connections.



#### Informed RRT*

> Significantly improved run time / convergence over RRT!

- Run RRT* as normal until you find an initial path.
- Bound the problem with the $L^2$ norm, prune outside the bounds.
- Sample from the PHS defining that $L^2$ norm.
- Repeatedly bound the problem with the norm, prune and sample inside until convergence.



### Optimizing Planners

> Potential Fields, CHOMP, TrajOpt, GPMP

Optimizing planners are not complete because there is not guarantee that they will converge to the true global optimal solution. CHOMP is an exception as it starts every time from a new random solution so in some sense it will find the true optimal global solution if it restarts enough times. 

#### Potential Fields

- Th emanipulator moves in a field of forces. The position to be reached is an attractive pole for the end effector and obstacles are repusive surfaces for the manipulator parts.

**Constraints:**

- inequality constraints 
  - collisions
  - workspace restrictions
- equality constraints 
  - quasi-static balance forhumanoid walking
  - holding an object upright



#### CHOMP

> Covariant Hamiltonian Optimization for Motion Planning

- start with some initial trajectory (usually straight line)
- represent a trajectory as a series of way points
- define some cost function
- perform gradient descent on the cost function

Chomp represents obstacles with a heat map.



### Anytime Solution Optimization

**Shortcutting**

- find a shorter, collision-free segment between two waypoints along a path
- randomized, fast

**Path Hybridization**

- combine portions of multiple solution paths to yield a shorter, hybrid path



#### Summary

| Sampling Based Planners                 | Optimizing Planners                                          |
| --------------------------------------- | ------------------------------------------------------------ |
| Sample and connect states               | begin with any trajectory (feasible or infeasible) and perturb it until optimal |
| RRT\*, RRT#, FMT\*, BIT\*               | Potential Fields, CHOMP, TrajOpt, GPMP, STOMP                |
| Relatively slow approach to convergence | Fast (about 1 order of magnitude faster)                     |
| Probabilistically Complete              | No completeness guarantees (except for CHOMP)                |



## Planning Under Uncertainty

**Robot motion is inherently noisy.**

- 1. External uncertainty: environment

  - maps have limited accuracy and detail
  - objects may be moving; doors open / close

- 2. internal effects: motion / actuation

  - motor commands are not exact
  - systematic bias (drift); mechanical imperfections

- 3. perceptual limitations: sensing

  - configuration of robot is not known *exactly*
  - sensors have finite resolution
  - processing sensor data is computationally difficult

Environmental uncertainty and perception uncertainty are related - what you perceive directly feeds into your model of the environment the robot uses. Environmental uncertainty could be more broadly stated as model uncertainty, as in, you have some model of your world (a map of the environment, what obstacles can look like, what other agents in this space do) which is updated based upon sensor data, but you may not be able to model everything in the world accurately. Perception uncertainty is simpler - data received from sensors could be inaccurate, and you must cope with the fact that sensors are returning data that is not ground truth.

SLAM is concerned with many corners of uncertainty - in general, SLAM methods relocalize the robot to account for action uncertainty, uses probabilistic techniques to build a map (a model of the world) from unreliable sensor data. However, what kind of map is built and how it is updated can lead to environment uncertainty (e.g., moving obstacles may not be updated fast enough by the SLAM algorithm and the robot may collide with an obstacle).

#### Environment Uncertainty

The broadest category of uncertainty.

- mapping and localization (SLAM)
- moving or movable obstacles
- game theory (adversaries)

Difficult to model *a priori*

- information gathering
- valid motion plan may not exist

Methods are typically **reactive**

- behavior based protocols ("reflexes")
- vector field histogram (local mapping and navigation)
- dynamic window (local collision avoidance)

#### Action Uncertainty

Obstacles known in advance. Robot moves with uncertainty along path.

A fixed action sequence (path) will not work because robot will deviate from an open-loop plan

Solution: Compute a policy: specification of an action for every possible state.

**Markov Decision Process (MDP)**

assume that effects of future actions do not depend on past history and next state is a function of only current state and action.

Optimal Policy can be computed by solving an MDP.

MDP(S: set of states, A: set of actions, T: transition probability function T(s' | s, a), R: reward function): 

A policy is a mapping from S to A.

An optimal policy maximizes the total expected reward over all states.

<u>Continuous MDPs are difficult to solve exactly</u>. We can approximate the continuous MDP by combining PRM and a discrete MDP -> SMR.

Sample states and discover probabilistic transitions between states using Monte Carlo simulation. SMR gives you a weighted directed graph (weights are transition probabilities)

SMR uses discrete set of controls.

If we don't know what state we are in, how can we select the right action? Need to use POMDP

MDP polynomial time. POMDP - pspace complete POMDP takes FOREVER

dimension of belief spaces is number of robot states!!

sampling based POMDP solvers: Coreideaistocomputeoptimalpolicyover 

reachable and probable beliefs 

## PDDL

PDDL is a way to express a task planning problem. We want to find a sequence of actions from start to goal. We can use heuristic graph search algorithms over the state space. Find neighboring states by looking at all actions that can be applied from a given state (this means preconditions are met). 

Note: This search space is typically very large.

#### Task Planning Approaches

**A***

Maintain a queue of states to explore (initialize to initial state).

On each iteration:

- remove a state $S$ from the queue where $S$ minimizes costFromStartTo($S$) + costFromXToGoal($S$)
  - first part we know, second part is heuristic 

- add each unvisited neighboring state to the queue
- repeat until we find the goal

Note: the effectiveness of this approach really depends on the heuristic / problem

**SAT-Plan**

- reduce planning to a SAT problem instance (boolean logic)
- solve with off-the-shelf solver
- avoid relying on task planning heuristics

The formulas which specify which properties are not changed as a result of an action are called frame axioms. They are called frame axioms because they limit or frame the effects of actions.

**Task and Motion Planning Naive Solution**

- find a task plan (as per above)
- for each action in plan, solve the motion planning problem -> concatenate to solve TMP

Problems with naive solution:

- if motion plan is infeasible, the algorithm fails
- instead, we want to try a different task plan
- motion planning algorithms are usually probabilistically complete (we know when we succeed, but we don't know if the instance is infeasible)
- we want some sort of feetback between the task planning and the motion planning algorithms

**Refined TMP Solution**

- Find a task plan
- for each action, solve the motion planning problem
- if motion planning fails, try another task plan
- if we can find motion plans for all actions, concatenate them together and return this plan as the solution
- continue until we succeed or timeout

Problems with refined solution

- When do we stop motion planning?

**Probabilistically Complete Solution**

- find a task plan
- for each action in task plan, attempt to solve the motion planning problem
- if motion planning fails, try another task plan
- eventually revisit motion planning
- if we can find motion plans for all actions, concatenate them together and return this plan as the solution
- continue until we succeed or timeout

<u>if something changes then you must have done something to change it</u>



## Multi Robot Planning

A multi-robot system is really just more degrees of freedom.

For $n$ robots with $C$ spaces $C_1,...,C_n$:

$C_{group}=C_1 \times \cdot\cdot\cdot \times C_n$

$s_{group} = \{s_1,...,s_n\}$

**So what is the problem?**

Second order car $C$ space is 5 dimensional $(x, y, \theta, v, \psi)$

- Planning for a task with 10 cars is 50 dimensional...
- "Curse of dimensionality" applies for even single robots!
- Collision of the system is if any one robot is in collision
  - The valid portion of the state space becomes very small
- **<u>Computation time is exponential in the number of robots.</u>**

**Solution Strategies**

- Centralized control, centralized computation
  - exponential cost per robot, cannot scale
- Centralized control, distributed computation
  - still needs a path that considers the entire group
- Partially-centralized control, partially-distributed computation
  - form local robot groups that each compute centrally
- Decentralized control, distributed computation
  - each robot plans only for itself, needs communication for safety, <u>incomplete</u>

<u>In general there is a trade off between centralization and completeness.</u>

**1. Coupled, centralized approach** 

- Plan directly in the combined configuration space of the entire robot team
- requires computational time exponential in the dimension of the configuration space
- complete but only applicable for small problems

**2. Centralize control, distributed computation**

- Auction system
  - Each robot computes a global plan for the entire joint $C-space$
  - Each candidate plan is voted on
  - Winning plan is executed across entire robot system

Robots need to communicate to avoid getting stuck!

<u>Method 1: plan $\rightarrow $ coordinate</u>

1. Plan locally.
2. Broadcast plan to neighbors (scalable and effective!). 
3. Wait for acknowledgements. A robot will only execute the plan it broadcasts if ALL neighbors send acknowledgements that they have a safe plan to follow.

<u>Method 2: seperate path planning and velocity planning</u>

1. Plan paths for each robot independantly.
2. coordinate robot paths so that collisions among robots are avoided (velocity tuning).

**Advantages**

dimensionality of configuration space does not increase

**Disadvantage**

coordination not always possible $\rightarrow$ decoupled planning is incomplete

**Problems**

- Livelock: some agents always stuck executing the same actions over and over.
- wireless communication is bad.
- could need more time.

**Velocity Coordination**

2D grid with horizontal axis corresponding to time for Path1, vertical for path2. Cell (i,j) is marked "forbidden" iff the i-th segment of Path1 collides with the j-th segment of Path2. Coordination is achieved by selecting any non-decreasing curve that avoids the "forbidden" cells and connects the lower-left corner to the upper-right corner.

**Prioritized Multi-Robot Planning Approach**

> in-between centralized and decentralized planning

- Robots sequentially construct trajectories.
- As each robot constructs its trajectory, it will use previously constructed trajectories as obstacles to avoid.
- NOTE: The priority is of critical importance.
- Can also dynamically determine priorities by estimating each robot's degree of planning difficulty based on the amount of occupied space surrounding the robot.

#### Summary

Centralized multi-robot planning is easy to formulate as the cross product of all $C$ spaces.

- generally is complete
- very costly computationally

Distributed & decentralized multi-robot planning is more difficult to reason about.

- generally incomplete
- computation is spread across all robots
- usually has some form of communication
- possible to still make safety guarantees



## Project 3

Random Tree Planner expands by "pushing" while RRT expands by "pulling".

RRT selects a random configuration <u>from the $C$ space</u> and then finds the nearest neighbor in the tree.

RTP selects a random configuration <u>from the tree</u> and a random configuration from the $C$ space and tries to connect them. 

## Project 4

Torque Controlled Pendulum. Configuration space: $S^1 \times \mathbb{R}$ (angular position, angular velocity) control space is just torque ($\mathbb{R}$). Note that this is a case where the distance metric doesn't mean much.

Car: C = (x,y,theta,v), U = (angular velocity, forward acceleration).

RGRRT: sample $q_{rand}$ then find state that is closest to $q_{rand}$ and has a state in $R(q_{near})$ that is closer to $q_{rand}$ than $q_{near}$. 