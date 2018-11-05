###Problem 1

1. No because the algorithm contains no way to report failure which a complete algorithm must do if there is no solution.

2. $(SE(3))^k$

3. The problem states that a mobile manipulator has 6 degrees of freedom and it is on a base. I am assuming this means that the manipulator itself has 6 degrees of freedom and that there are more degrees of freedom from the base.

   Thus the combined system has 6 degrees of freedom from the manipulator and 3 degrees of freedom from the mobile base (translation and rotation in 2D) for a total dimension of 9.

4. The free space is the set of free configurations. Thus the free space for this 20dof robot is the set of all configurations such that the robot in that configuration does not collide with any obstacle.

5. advantage: quaternions don't suffer from gimbal lock, concatinating rotations is faster and more stable. 

   disadvantage: quaternions are very hard to visualize or understand.

6. true the complexity is O(m + n)

7. O(mn)

8. Yes RRT does discrete collision checking when it needs to instead of computing the whole space. since it randomly samples the c-space it doesn't need to know about the entire c-space.

9. because the topology tells us about properties of the c-space. for example if your topology is R^2 vs SxS one is a torus so values will be circular and repeat wheras the other will not.

10. The c-space obstacles QA and QB depend on the robot. so even though A and B do not overlap in the workspace it is possible for QA and QB to overlap if the robot is big enough.

11. no 

12. This will give a path it a path is possible as time goes to infinity. It is not necessarily the shortest path because of the random choices that are inherent in sampling and smoothing.

13. There are many methods for many scenarios. one way to choose between them for a particular problem is to use lots of samplers and assign each one a weight. as a particular sampler performs well you increase its weight. you can then choose from the samplers based on the probability weight.

14. an advantage lazy PRM has over original PRM is that it delays computing collisions until it absolutely needs to. this helps to cut down on collision checking calculations which can often times be the brunt of the computational load.

    disadvantage: once the original PRM finishes its roadmap even though you have done more work calculating collisions, you now have all that information which can be reused. 

15. to get better resolution

16. Often times which variation of PRM is better depends on the situation (using gaussian distribution biased near obstacles might help with narrow passages). You could run both planners many times on many different situations in order to see which performs better on average or if you had a specific situation you could run each planner may times on that situation. you could also use both planners and assign weights as discussed in problem 13 and look at which planner ended up having a higher weight.

    ### Problem 2

    1. Topology = $S^1 \times S^1 \times S^1 \times S^1 \times S^1  \times S^1 $  Dimension = 6

    2. There is a typo on the test that says "move 1freely in the plane". I am taking this to mean that it can move freely in 2D (translation and rotation).

       The arrow on the base of the robot I am assuming is refering to the rotation of the mobile base and not some other join as it has no label.

       Topology = $\mathbb{R}^2 \times S^1 \times \mathbb{R}^1 \times S^1 \times S^1 \times S^1 \times S^1 \times S^1 \times S^1 \times S^1 \times S^1 \times S^1 \times \mathbb{R}^1$Dimension = 14.

    3. 

       ```pseudocode
       CollisionCheck(m1, m2) // checks if two meshes are in collision
       
       myCollisionChecker():
           for i in 1,...r // for each robot mesh
               for j in 1,...n // for each obstacle
                   if CollisionCheck(R_i, M_j)
                       return "COLLISION"
           return "NO COLLISION"
       ```

    4.  

       a. Since the converyor belt is moving I would want a local planner that doesn't remember obstacles between queries (i.e. not PRM, SRT). I would likely use and EST algorithm to continuially push the tree frontier searching for the moving weight.

       b. I would use PRM because we could build the road map once and reuse it for many queries.  

       c.Because the obstacles move around slightly you can't rely on a previously built roadmap to maintain an accurate representation i would use a local planner. Of the local planners I would choose Bi-tree with 2 RRTs from the start and end goals for improved computational efficiency.

    ### Problem 3

    drawn on written test
