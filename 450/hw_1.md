# C450 HW 1

1. **[20 pts]** Describe the trade-offs between the Bug 1 and Bug 2 algorithms. Limit your response to no more than half a page.

   Bug1 is an exhaustive algorithm, i.e. "always looks at all choices before committing". Bug2 is a greedy algorithm, i.e. it "takes the first thing that looks better". Bug2 seems as if it would be a much better algorithm than Bug1 because it doesn't waste so much motion by mapping out an entire obstacle everytime it encounters one, however there are many cases where Bug1 outperforms Bug2. 

   While Bug1's exhaustive technique is slow, it is also predictable. Bug2 on the otherhand needs more "luck". Because it is greedy and takes the "first thing that looks better" the algorithm performs very well as long as the thing that it thinks "looks better" is actually better. However, in cases where this is not true, it can end up taking much longer than Bug1 as we saw in the following example from class:

   <img src="/Users/Peter/Desktop/Screen Shot 2018-09-06 at 12.25.54 PM.png" height="300px">

2. **[30 pts]** Suppose you are planning for a point robot in a 2D workspace with polygonal obstacles. The start and goal locations of the robot are given. The visibility graph is defined as follows:

   - The start, goal, and all vertices of the polygonal obstacles compose the vertices of the graph.

   - An edge exists between two vertices of the graph if the straight line segment connecting the vertices does not intersect any obstacle. The boundaries of the obstacles count as edges.

     <u>Answer the following questions:</u>

     (a) **[15 pts]** Provide an upper-bound of the time it takes to construct the visibility graph in big-O notation. Give your answer in terms of *n*, the total number of vertices of the obstacles. Provide a short algorithm in pseudocode to explain your answer. Assume that computing the intersection of two line segments can be done in constant time.

     ```pseudocode
     for v_1 in V: // loop over all the vertices
     	for v_2 in V: // loop over all the vertices
     		for each edge of an obstacle: 
     			// edge.p1 and edge.p2 are the endpoints of the edge
     			if !does_intersect((v_1, v_2), (edge.p1, edge.v2)): 
     				add an edge from v_1 to v_2 to visibility graph
     				
     // note: i'm assuming that we have a method "does_intersect((a1, b1), (a2, b2))" that takes two tuples (a1, b1) and (a2, b2) such that a1 and b1 represent the endpoints of a line segment and a2 and b2 represent the endpoints of a line segment. It then computes in constant time the intersection of the two line segments and returns true if the intersection is found and false otherwise.
     ```

     This algorithm is $O(n^2*|E|)$. Where $|E|$ is the number of obstacle edges. If we assume that $|E|$ is proportional to $n$ then this algorithm is $O(n^3)$.

     (b) **[15 pts]** Can you use the visibility graph to plan a path from the start to the goal? If so, explain how and provide or name an algorithm that could be used. Provide an upper-bound of the run-time of this algorithm in big-O notation in terms of *n* (the number of vertices in the visibility graph) and *m* (the number of edges in the visibility graph). If not, explain why.

     Yes you can use the visibility graph to plan a path from start to goal. Run **BFS** starting at the start vertex however keep a mapping from nodes to parents. Once **BFS** is complete, you can retrace your steps from the goal node back to the start node using the nodes to parents mapping. If **BFS** never finds the goal node, there is no path. The runtime of this algorithm is just the runtime of **BFS** which is $O(n + m)$.

3. **[20 pts]** Let $\mathcal{A}$ be a unit disc centered at the origin in a workspace $\mathcal{W}=\mathbb{R}^2$. Assume that $\mathcal{A}$ is represented by a single algebraic primitive $H=\{(x,y) | x^2+y^2\leq1\}$. Show that if this primitive is rotated about the origin that the transformed primitive is unchanged. This can be shown by showing that any point within the transformed primitive $H'$  must be within $H$, and vice versa.

   ![IMG_3656](/Users/Peter/Downloads/IMG_3656.jpg)

4. **[30 pts]** You are given the endpoints to two line segments, $A_1B_1$ and $A_2B_2$, in a 2D workspace. The line segments include their endpoints. Provide an algorithm in pseudocode to compute the intersection points of these two line segments, if one exists. Be careful and consider all corner cases. You algorithm must provide the correct output with every input. Hint: There are many ways to represent all line segments. Choosing wisely will allow for a shorter and more efficient implementation.

   ```pseudocode
   // Note: (x1, y1) and (x2, y2) are the endpoints of the first line.
   // (x3, y3) and (x4, y4) are the endpoints of the second line.
   def does_intersect(x1, y1, x2, y2, x3, y3, x4, y4):
           m1 = (y2-y1)/(x2-x1) // calculate slope of first line
           m2 = (y4-y3)/(x4-x3) // calculate slope of second line
           b1 = y1 - (m1 * x1) // calculate y-int of first line
           b2 = y3 - (m2 * x3) // calculate y-int of second line
           
           // if the slopes are the same then the lines are either colinear
           // or do not intersect
           if (m1 == m2):
           	// if they are colinear
           	if ((x2 > min(x3, x4) and x2 < max(x3, x4)
           		and y2 > min(y3, y4) and y2 < max(y3, y4)) 
           		or 
           		(x1 > min(x3, x4) and x1 < max(x3, x4)
           		and y1 > min(y3, y4) and y1 < max(y3, y4))):
           		return "Colinear"
           	// if they don't intersect
           	else:
           		return False
           
           // calculate intersection point of two lines (not line segments)
           xInt = (b2-b1)/(m1-m2) 
           yInt = m1 * xInt + b1
   
           // check if the intersection is on line A
           if (xInt > max(x1, x2) or xInt < min(x1, x2) or yInt < min(y1, y2) or yInt > max(y1, y2)):
                   return False
   
   		// check if the intersection is on line B
           if (xInt > max(x3, x4) or xInt < min(x3, x4) or yInt < min(y3, y4) or yInt > max(y3, y4)):
                   return False
   
           return (xInt, yInt)
   ```


