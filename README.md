Download Link: https://assignmentchef.com/product/solved-cs2040s-problem-set-8-when-the-travelling-salesman-meets-the-mst
<br>
The travelling salesman problem (often abbreviated as TSP) is one of the most famous problems in combinatorial optimization. Like many good problems, it is a simple question that is easy to state. You are given a set of cities <em>v</em><sub>0</sub><em>,v</em><sub>1</sub><em>,…,v<sub>n</sub></em>. You know the distance<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a> between any pair of cities. What is the fastest route that visits every city exactly once, and then returns to where you started?

See Figure 1 for an example of the travelling salesman problem, along with three possible solutions. A valid tour visits every city exactly once, and ends back at the first node—forming a cycle<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>. The goal is to find the shortest valid tour.

The travelling salesman problem has a reputation for being very hard: it is well-known to be NP-hard, meaning that there is no polynomial time algorithm that can find an optimal tour <em>unless P = NP</em>. In reality, though, the travelling salesman problem is not that difficult: we can efficiently calculate solutions that are <em>pretty good</em>. That is, for a given set of cities, we can find a tour that is approximately as good as the best tour<a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>.

In this problem set, you will be developing a solution for the travelling salesman problem based on minimum spanning trees. The algorithm you develop will be a 2-approximation scheme, i.e., it will guarantee that the tour it discovers is at most twice as long as the optimal tour.

<strong>Figure 1: The top graph is an example of a TSP problem containing </strong>4 <strong>cities </strong>{<em>A,B,C,D</em>}<strong>. On the bottom, with dashed lines, are three sample tours. The first (</strong><em>A,D,B,C,A</em><strong>) has cost 32, the second (</strong><em>A,D,C,B,A</em><strong>) has cost 29, and the third (</strong><em>A,B,D,C,A</em><strong>) has cost 35. Notice that each tour visits all four cities exactly once.</strong>

<strong>TSP-MST Algorithm.  </strong>In describing the algorithm<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a>, we will think of the map as a graph <em>G </em>= (<em>V,E</em>) where <em>V </em>is a set of <em>n </em>cities and <em>E </em>is a set of <em>n</em>(<em>n </em>−1)<em>/</em>2 undirected edges connecting every pair of cities. The algorithm consists of three steps:

<ol>

 <li>Find a minimum spanning tree <em>T </em>of the graph <em>G</em>. (You can use any efficient algorithm for finding a minimum spanning tree.) Notice that the cost of the tree <em>T </em>is always less than the cost of the optimal tour (since if you remove any one edge from the optimal tour, it forms a spanning tree).</li>

 <li>Perform a depth-first-search walk of the tree <em>T</em>. When you perform the depth-first-search, remember every time you visit a node. Every node in the graph appears at least twice in the DFS walk. (Every edge in the tree <em>T </em>is crossed exactly twice in the DFS walk.) Let</li>

</ol>

<em>D </em>= <em>d</em><sub>0</sub><em>,d</em><sub>1</sub><em>,d</em><sub>2</sub><em>,d</em><sub>3</sub><em>,…,d</em><sub>2<em>n</em></sub>−1 be the 2<em>n </em>cities visited on the DFS treewalk. Notice that <em>D </em>has cost at most twice the optimal tour (since each edge is crossed twice), and visits every city at least once. It is not a valid tour, however, since cities are visited more than once.

<ol start="3">

 <li>Take short-cuts to avoid revisiting cities. For example, if you are in city <em>d<sub>i </sub></em>and have already visited city <em>d<sub>i</sub></em><sub>+1</sub>, then skip city <em>d<sub>i</sub></em><sub>+1 </sub>and go directly to <em>d<sub>i</sub></em><sub>+2</sub>. (If you have already visited <em>d<sub>i</sub></em><sub>+2</sub>, then skip it and go on to the next city.) Since the distances satisfy the triangle inequality, these short-cuts can only decrease the length of the tour. Now you have a valid tour that is at most twice as long as the optimal tour.</li>

</ol>

<strong>Figure 2: Here, we demonstrate how the algorithm works. In the figure, we have already calculated a minimum spanning tree, which consists of four edges: </strong>(<em>A,D</em>)<strong>, </strong>(<em>B,C</em>)<strong>, </strong>(<em>C,D</em>)<strong>, and </strong>(<em>C,E</em>)<strong>. (The dashed edges are not part of the spanning tree.) Next, starting at node </strong><em>A</em><strong>, we perform a depth-first-search treewalk, remembering every time we visit a node:</strong>

<strong>A</strong>→<strong>D</strong>→<strong>C</strong>→<strong>B</strong>→ <em>C </em>→<strong>E</strong>→ <em>C </em>→ <em>D </em>→ <em>A</em>

<strong>We have put in bold the first time the DFS traversal visits a node, and we have used red to indicate later (unnecessary) visits. Finally, to devise the final tour, we skip the cities we have already visited, yielding the following tour:</strong>

<strong>A</strong>→<strong>D</strong>→<strong>C</strong>→<strong>B</strong>→<strong>E</strong>→ <em>A</em>

<strong>Notice that the first city is visited again at the end of the tour, completing the cycle.</strong>

<strong>TSPMap class. </strong>You are provided, as part of this problem, with the java class TSPMap. This class encapsulates the basic functionality for storing the locations of a set of cities, calculating the distance between the cities, and drawing a map of the cities. Note that you should not be modifying this class in your solution. The constructor TSPMap(String fileName) takes a map filename as an input and reads in all the points in the file. You can then access these points with the following methods:

<ul>

 <li>int getCount(): returns the number of points.</li>

 <li>Point getPoint(int i): returns point <em>i</em>.</li>

 <li>double pointDistance(int i, int j): calculates the distance between points <em>i </em>and <em>j</em>.</li>

</ul>

Each point can also store a single <em>link </em>to some other point. This link may be the parent edge in a spanning tree, or it might be the next point to visit on an MST tour. You can access the link using the following methods:

<ul>

 <li>void setLink(int i, int j): sets a link from <em>i </em>to <em>j</em>, and redraws the screen immediately.</li>

 <li>void setLink(int i, int j, boolean redraw): sets a link from <em>i </em>to <em>j </em>and redraws only if redraw==true.</li>

 <li>void eraseLink(int i): erases the outgoing link from <em>i </em>and redraws the screen immediately.</li>

 <li>void eraseLink(int i, boolean redraw): erases the outgoing link from <em>i </em>and redraws only if redraw==true.</li>

 <li>int getLink(int i): returns the current outgoing link from <em>i</em>.</li>

</ul>

Whenever you modify the map, you may want to call the redraw() method to redraw the map.

The map class contains a static nested class Point which contains the information for a point. Notably, this class stores the <em>x </em>and <em>y </em>coordinates for a point, as well as the link. It includes functionality to calculate the distance between two points.

The main method has an example of how to use the map class. It reads in a file hundredpoints.txt, and then sets up the links: each point <em>i </em>is linked to point <em>i</em>+1, and the last point is linked back to point 0—creating a valid (if suboptimal) tour. Finally, it calls redraw to draw the tour.

<strong>Assignment. </strong>Your task is to implement a class TSPGraph that implements the IApproximateTSP interface. This interface supports four methods:

<ul>

 <li>MST(TSPMap map): Calculate a minimum spanning tree for the points on the map. When the method returns, each point in the map should have its link set to its parent in the spanning tree.</li>

 <li>TSP(TSPMap map): Calculate a good TSP tour using the algorithm described above: perform a depth-first search on a minimum spanning tree, using short-cuts to ensure that each point is visited only once. When the method returns, each point in the map should have its link set to the next point on the tour. Note that TSP is a standalone method, MST should be called again within it.</li>

 <li>isValidTour(TSPMap map): Returns true if the links on the map form a valid tour. (Note: a tour can be valid even if it is far from optimal.) This may be useful for debugging.</li>

 <li>tourDistance(TSPMap map): If the links on the map form a valid tour, then this method returns the total length of that tour (i.e., the sum of all the edges on the tour). If the links do not form a valid tour, return -1. Note that the length of the tour includes the cost of returning back to the start when the tour is finished.</li>

</ul>

Note that a TSP implementation using a different algorithm than the one provided will only receive partial credit, even if it is a working solution.

You are also free to use the provided implementation of a priority queue TreeMapPriorityQueue as part of your solution. As its name suggests, this class implements the priority queue ADT with a TreeMap and includes the method decreasePriority, which Java’s PriorityQueue does not provide.

<strong>Figure 3: This figure contains an example using the file “fiftypoints.txt”. The first image contains the fifty points. The second image depicts a minimum spanning tree. The third image depicts a valid tour for the travelling salesman problem.</strong>

<a href="#_ftnref1" name="_ftn1">[1]</a> For today, we are focusing on the <em>Euclidean </em>travelling salesman problem where the distance between two cities is the Euclidean distance in the 2d-plane.

<a href="#_ftnref2" name="_ftn2">[2]</a> Technically, a valid tour is known as a Hamiltonian Cycle.

<a href="#_ftnref3" name="_ftn3">[3]</a> In fact, where the distances are Euclidean, you can get an approximation that is as close as you like! This is known as a PTAS: polynomial time approximation scheme.

<a href="#_ftnref4" name="_ftn4">[4]</a> Note that this 2-approximation algorithm only works if the problem instance satisfies the traingle inequality. In the context of this question, it means that the cost of going from city A to city C is less than the cost of going from city A to city B, then city B to city C.