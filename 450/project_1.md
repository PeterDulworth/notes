# C450 Project 1 Deliverables

1. **[5 pts]** Which method(s) did you use to install `OMPL` and `OMPL.app`?

   I installed `OMPL` and `OMPL.app` by downloading VirtualBox and the provided virtual machine with ubuntu, `OMPL`  and `OMPL.app` preinstalled.

2. **[30 pts]** Which demos did you run in Exercise 1? Describe the perceived difficulty of all the demos you ran. Did any demos seem easier or harder to solve than others?

   I ran `RigidBodyPlanning.cpp`, `SE3RigidBodyPlanning.cpp`, and `DynamicCarPlanning.cpp`.

   - `RigidBodyPlanning` seemed very easy to solve. When I ran it, it found a solution in `0.003951` seconds. The original solution had `188` states but in another `0.001995` seconds it was able to simplify that down to only `2` states. This makes sense as there were no obstacles. Of the demos that I ran, this seemed to be the easiest to solve.

   - `SE3RigidBodyPlanning` seemed moderately hard to solve. When I ran it, it found a solution in `1.542041` seconds. The original solution had `156` states but in another `.175536` seconds it was able to simplfy that down to only `26` states. It makes sense that this one was more difficult to solve than `RigidBodyPlanning` as there were obstacles. 

   - `DynamicCarPlanning` seemed difficult to  solve. When I ran it, it found an approximate solution in `40.01269` seconds. Of the demos that I ran, this seemed to be the hardest to solve.

3. **[20 pts]** Which configurations did you run in Exercise 2, the OMPL.app GUI? Describe the perceived difficulty of all the configurations you ran in Exercise 2. Did any configurations seem easier or harder to solve than others?

   I ran `Easy.cfg` and `Twistycool.cfg`.

   - `Easy.cfg` seemed easy to solve. The robot was placed directly in front of a window and its goal was to go through the window. `Easy.cfg` was definitely the easist to solve.
   - `Twistycool.cfg` seemed fairly difficult to solve. It was very similiar to `Easy.cfg` however the window that the robot had to go through was much smaller so it had to twist in order to fit. `Twistycool.cfg` was definitely the hardest to solve.

4. **[20 pts]** Did any planners perform better than others in Exercise 2? Elaborate.

   The two planners I used were `KPIECE1` and `PRM`. I tried both of them on the two configurations from question 3. 

   Both `KPIECE1` and `PRM` were able to solve the `Easy.cfg` configuration very quickly. I would say they performed about the same.

   In the `Twistycool.cfg` (when both planners had their default parameters) the `PRM` planner slightly out performed the `KPIECE1` planner. `PRM` averaged about `25` seconds – rarely taking more than `40` seconds. `KPIECE1` average about `40` seconds and would frequently take over `60` seconds to find a solution. However, after tweaking the parameters of both planners, the `KPIECE1` planner was able to outperform the `PRM` planner on the `Twistycool.cfg` configuration as I will explain below.

5. **[20 pts]** Did you encounter any cases where changing a planner's parameters improved performance on a particular configuration in Exercise 2? Explain.

   The only case in which I found that changing a planner's parameters improved its performance was `KPIECE1`'s range parameter on the `Twistycool.cfg` configuration. 

   By changing range to 100 `KPIECE1` was able to find a solution to `Twistycool.cfg` substantially faster. Before adjusting range `KPIECE1` averaged about `40` seconds to find a solution. After adjusting range, `KPIECE1` was on average able to find a solution in roughly `20` seconds.

6. **[5 pts]** Rate the difficulty of each exercise on a scale of 1-10 (1 being trivial, 10 being impossible). Give an estimate of how many hours you spent on each exercise, and detail what was the hardest part of the assignment.

   - Exercise 0:
     - Difficulty: `1`
     - Hours Spent: `0.5`

   - Exercise 1:
     - Difficulty: `4`
     - Hours Spent: `1.5`
   - Exercise 2:
     - Difficulty: `3`
     - Hours Spent: `2`

   The hardest part of the assignment was exercise `1`. While compiling and running the programs themselves was very straightforward, it took me a while to figure out what the output of the programs meant and overall just get used to the system.