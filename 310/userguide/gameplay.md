# GAMEPLAY - Final Project User Manual

**Authors:** Peter Dulworth (psd2), Sophia Jefferson (sgj1) 

**Group: **26

### Summary

- Nodes are black markers on the map (green when selected). 
- Edge weights are white markers on the map.
- The goal of the game is to collaborate with your teammates to select a sequence of nodes connecting and including `START` and `GOAL` such that the sum of the edge weights between them is minimized. i.e. the shortest path.
- The first team to find the solution wins!

### GamePlay

The goal of this game is to use (your best guess of) the $A^*$ algorithm to find an image of Scotty Scot Scot Rixner! There are $16$ nodes (numbered $1$-$14$, with a `START` and an `GOAL` node). **Nodes are indicated by black markers on the map.** There are $20$ edges connecting the nodes. **Edges weights are indicated by white markers on the map.** The goal of the game is to find the shortest path from `START` to `GOAL`. The first team to select the shortest path wins! However, you must coordinate as a team to find this path. This is enforced by the following rules.

**Collaboration Rules**

1. Every team member selects at least one node on the path.
2. No two members of the same team select the same node.
3. The union of all the team members selected nodes is the shortest path from `START` to `GOAL`.

To fulfill these requirements, make sure that your team has communicated in its team chatroom to first determine what the shortest path is and then to determine which segments each person would will submit, as part of the path.

**Who is on your team?**

Your team will be printed in bold on the top left of your screen. It will be either `Team 1` or `Team 2`. All the members of your team will be printed in the log at the bottom of the screen.

**Selecting Nodes**

To select a node simply click its label on the map. If it is selected it will turn from green.

**Submitting a Path to the Joint Team Path**

To let your teammates know what part of the total path you will are submitting, first select all the nodes that you want. Then press `Add Segment To Team Path`. You can redo this process as many times as you want. 

**Submitting a Team Solution**

Once you have coordinated with your team and every member has submitted their segment using the process described above, any member of the team can press the `Submit Team Path` button. If there are any errors with your path they will be printed to the console. If the path is incorrect it will be printed. If it is correct and you are the first team to submit the correct path you have won the game!

As the game progresses, the Game Server will notify your team of submissions along with the nodes in the submitted path in the team chatroom.