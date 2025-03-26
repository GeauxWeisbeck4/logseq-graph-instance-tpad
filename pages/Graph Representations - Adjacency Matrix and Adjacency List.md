# Graph Representations: Adjacency Matrix and Adjacency List
	- Graphs are fundamental data structures used to model relationships between objects. Representing graphs effectively is crucial for designing efficient graph algorithms. The choice of representation can significantly impact the performance of algorithms that operate on the graph. This lesson explores two common graph representations: the adjacency matrix and the adjacency list, detailing their properties, trade-offs, and use cases. Understanding these representations is essential for choosing the right one for a given application and for implementing graph algorithms effectively.
	- ## Adjacency Matrix
		- An adjacency matrix is a 2D array (matrix) that represents the presence or absence of edges between vertices in a graph. For a graph with _n_ vertices, the adjacency matrix is an _n x n_ matrix, where the element at `matrix[i][j]` indicates whether there is an edge from vertex _i_ to vertex _j_.
		- ### Representation
			- In an _unweighted_ graph, the value of `matrix[i][j]` is typically a boolean value (true/false or 1/0). `matrix[i][j] = true` (or 1) indicates that there is an edge from vertex _i_ to vertex _j_, and `matrix[i][j] = false` (or 0) indicates that there is no edge.
			- In a _weighted_ graph, the value of `matrix[i][j]` represents the weight of the edge from vertex _i_ to vertex _j_. If there is no edge, the value is typically set to infinity (∞) or a special value (e.g., -1) to indicate the absence of an edge.
			- For _undirected_ graphs, the adjacency matrix is symmetric, meaning that `matrix[i][j] = matrix[j][i]` for all _i_ and _j_.
			- For _directed_ graphs, the adjacency matrix is not necessarily symmetric.
		- ### Example: Unweighted Undirected Graph
			- Consider an undirected graph with 4 vertices (A, B, C, D) and the following edges:
				- A - B
				- A - C
				- B - C
				- C - D
			- The adjacency matrix for this graph would be:
			- |  | A | B | C | D |
			  | --- | --- | --- | --- | --- |
			  | **A** | 0 | 1 | 1 | 0 |
			  | **B** | 1 | 0 | 1 | 0 |
			  | **C** | 1 | 1 | 0 | 1 |
			  | **D** | 0 | 0 | 1 | 0 |
		- ### Example: Weighted Directed Graph
			- Consider a directed graph with 4 vertices (A, B, C, D) and the following edges with their respective weights:
				- A -> B (weight: 2)
				- B -> C (weight: 3)
				- C -> A (weight: 1)
				- C -> D (weight: 4)
			- The adjacency matrix for this graph would be:
			- |  | A | B | C | D |
			  | --- | --- | --- | --- | --- |
			  | **A** | 0 | 2 | 0 | 0 |
			  | **B** | 0 | 0 | 3 | 0 |
			  | **C** | 1 | 0 | 0 | 4 |
			  | **D** | 0 | 0 | 0 | 0 |
			- Note: A value of 0 indicates no edge in this example. In practice, you might use infinity or -1.
		- ### Properties
			- **Space Complexity:** O(V2), where V is the number of vertices. This is because the matrix has V rows and V columns.
			- **Time Complexity:**
				- Checking if an edge exists between two vertices: O(1)
				- Iterating over all edges connected to a vertex: O(V)
				- Adding or removing an edge: O(1)
		- ### Advantages
			- Simple to implement.
			- Edge existence check is O(1).
			- Good for dense graphs (graphs with many edges).
		- ### Disadvantages
			- High space complexity, especially for sparse graphs (graphs with few edges).
			- Iterating over all edges connected to a vertex takes O(V) time, even if the vertex has only a few neighbors.
			- Adding or removing a vertex requires resizing the matrix, which can be expensive.
	- ## Adjacency List
		- An adjacency list is a collection of unordered lists used to represent a graph. Each list describes the set of neighbors of a vertex in the graph.
		- ### Representation
			- An adjacency list is typically implemented using an array, hash table, or linked list, where each element represents a vertex in the graph.
			- For each vertex, the corresponding element in the array/hash table/linked list stores a list of its adjacent vertices (neighbors).
			- In an _unweighted_ graph, the list contains the vertex IDs of the neighbors.
			- In a _weighted_ graph, the list contains pairs of (neighbor vertex ID, edge weight).
		- ### Example: Unweighted Undirected Graph
			- Consider the same undirected graph with 4 vertices (A, B, C, D) and the following edges:
				- A - B
				- A - C
				- B - C
				- C - D
			- The adjacency list for this graph would be:
				- A: [B, C]
				- B: [A, C]
				- C: [A, B, D]
				- D: [C]
		- ### Example: Weighted Directed Graph
			- Consider the same weighted directed graph with 4 vertices (A, B, C, D) and the following edges with their respective weights:
				- A -> B (weight: 2)
				- B -> C (weight: 3)
				- C -> A (weight: 1)
				- C -> D (weight: 4)
			- The adjacency list for this graph would be:
				- A: [(B, 2)]
				- B: [(C, 3)]
				- C: [(A, 1), (D, 4)]
				- D: [ ]
		- ### Properties
			- **Space Complexity:** O(V + E), where V is the number of vertices and E is the number of edges. This is because we store each vertex once and each edge once (or twice in an undirected graph).
			- **Time Complexity:**
				- Checking if an edge exists between two vertices: O(degree(V)), where degree(V) is the number of neighbors of vertex V. In the worst case, this can be O(V).
				- Iterating over all edges connected to a vertex: O(degree(V))
				- Adding or removing an edge: O(degree(V)) in the worst case (if you need to search for the edge in the list). Can be O(1) if you maintain a separate data structure (e.g., a hash set) for each adjacency list.
		- ### Advantages
			- Lower space complexity for sparse graphs compared to adjacency matrix.
			- Iterating over all edges connected to a vertex is efficient (O(degree(V))).
			- Adding or removing a vertex is generally easier than with an adjacency matrix.
		- ### Disadvantages
			- Edge existence check is slower than with an adjacency matrix (O(degree(V)) vs. O(1)).
			- Can be more complex to implement than an adjacency matrix.
	- ## Choosing the Right Representation
		- The choice between adjacency matrix and adjacency list depends on the specific application and the characteristics of the graph.
		- | Feature | Adjacency Matrix | Adjacency List |
		  | --- | --- | --- |
		  | Space Complexity | O(V2) | O(V + E) |
		  | Edge Existence | O(1) | O(degree(V)) |
		  | Edge Iteration | O(V) | O(degree(V)) |
		  | Density | Good for dense graphs | Good for sparse graphs |
		  | Implementation | Simple | More Complex |
			- **Dense Graphs:** If the graph is dense (i.e., it has many edges), the adjacency matrix is often a better choice because of its O(1) edge existence check.
			- **Sparse Graphs:** If the graph is sparse (i.e., it has few edges), the adjacency list is usually a better choice because of its lower space complexity.
			- **Memory Constraints:** If memory is a major constraint, the adjacency list is generally preferred, especially for large graphs.
			- **Algorithm Requirements:** Some algorithms may be more efficient with one representation than the other. For example, algorithms that frequently check for the existence of edges may benefit from the adjacency matrix.
	- ## Code Examples
		- Here are code examples in Python demonstrating the implementation of adjacency matrix and adjacency list.
		- ### Adjacency Matrix in Python
			- ```
			  class AdjacencyMatrix:
			      def __init__(self, num_vertices):
			          self.num_vertices = num_vertices
			          self.matrix = [[0] * num_vertices for _ in range(num_vertices)] # Initialize matrix with zeros
			      def add_edge(self, u, v, weight=1):
			          """Adds an edge between vertices u and v.  Weight defaults to 1 for unweighted graphs."""
			          self.matrix[u][v] = weight
			          self.matrix[v][u] = weight  # For undirected graphs
			      def remove_edge(self, u, v):
			          """Removes the edge between vertices u and v."""
			          self.matrix[u][v] = 0
			          self.matrix[v][u] = 0  # For undirected graphs
			      def has_edge(self, u, v):
			          """Checks if an edge exists between vertices u and v."""
			          return self.matrix[u][v] != 0
			      def get_neighbors(self, vertex):
			          """Returns a list of neighbors for a given vertex."""
			          neighbors = []
			          for i in range(self.num_vertices):
			              if self.matrix[vertex][i] != 0:
			                  neighbors.append(i)
			          return neighbors
			      def print_matrix(self):
			          """Prints the adjacency matrix."""
			          for row in self.matrix:
			              print(row)
			  # Example usage:
			  graph = AdjacencyMatrix(4)
			  graph.add_edge(0, 1)
			  graph.add_edge(0, 2)
			  graph.add_edge(1, 2)
			  graph.add_edge(2, 3)
			  graph.print_matrix()
			  # Expected output:
			  # [0, 1, 1, 0]
			  # [1, 0, 1, 0]
			  # [1, 1, 0, 1]
			  # [0, 0, 1, 0]
			  print(f"Has edge between 0 and 1: {graph.has_edge(0, 1)}") # True
			  print(f"Neighbors of vertex 0: {graph.get_neighbors(0)}") # [1, 2]
			  graph.remove_edge(0, 1)
			  print(f"Has edge between 0 and 1 after removal: {graph.has_edge(0, 1)}") # False
			  ```
		- ### Adjacency List in Python
			- ```
			  class AdjacencyList:
			      def __init__(self, num_vertices):
			          self.num_vertices = num_vertices
			          self.adj_list = [[] for _ in range(num_vertices)] # Initialize an empty list for each vertex
			      def add_edge(self, u, v, weight=1):
			          """Adds an edge between vertices u and v. Weight defaults to 1 for unweighted graphs."""
			          self.adj_list[u].append((v, weight))
			          self.adj_list[v].append((u, weight))  # For undirected graphs
			      def remove_edge(self, u, v):
			          """Removes the edge between vertices u and v."""
			          # Remove (v, weight) from u's adjacency list
			          self.adj_list[u] = [(neighbor, weight) for neighbor, weight in self.adj_list[u] if neighbor != v]
			          # Remove (u, weight) from v's adjacency list
			          self.adj_list[v] = [(neighbor, weight) for neighbor, weight in self.adj_list[v] if neighbor != u]
			      def has_edge(self, u, v):
			          """Checks if an edge exists between vertices u and v."""
			          for neighbor, _ in self.adj_list[u]:
			              if neighbor == v:
			                  return True
			          return False
			      def get_neighbors(self, vertex):
			          """Returns a list of neighbors for a given vertex."""
			          return [neighbor for neighbor, _ in self.adj_list[vertex]]
			      def print_list(self):
			          """Prints the adjacency list."""
			          for vertex in range(self.num_vertices):
			              print(f"Vertex {vertex}: {self.adj_list[vertex]}")
			  # Example usage:
			  graph = AdjacencyList(4)
			  graph.add_edge(0, 1)
			  graph.add_edge(0, 2)
			  graph.add_edge(1, 2)
			  graph.add_edge(2, 3)
			  graph.print_list()
			  # Expected output:
			  # Vertex 0: [(1, 1), (2, 1)]
			  # Vertex 1: [(0, 1), (2, 1)]
			  # Vertex 2: [(0, 1), (1, 1), (3, 1)]
			  # Vertex 3: [(2, 1)]
			  print(f"Has edge between 0 and 1: {graph.has_edge(0, 1)}") # True
			  print(f"Neighbors of vertex 0: {graph.get_neighbors(0)}") # [1, 2]
			  graph.remove_edge(0, 1)
			  print(f"Has edge between 0 and 1 after removal: {graph.has_edge(0, 1)}") # False
			  ```
	- ## Exercises
		- 1.  **Implement a weighted graph using both adjacency matrix and adjacency list representations.** Add methods to update the weight of an edge.
		- 2.  **Compare the memory usage of adjacency matrix and adjacency list for a graph with 1000 vertices and 10000 edges.** You can use Python's `sys.getsizeof()` to estimate memory usage.
		- 3.  **Implement a function that converts an adjacency matrix representation of a graph to an adjacency list representation, and vice versa.**
		- 4.  **Consider a social network where vertices represent users and edges represent friendships. How would you represent this graph using an adjacency list? What are the advantages and disadvantages of using an adjacency matrix in this scenario?**
		- 5.  **Modify the AdjacencyList class to use a hash set (e.g., `set` in Python) instead of a list to store the neighbors of each vertex. How does this affect the time complexity of the `has_edge` and `remove_edge` operations?**
	- ## Summary
		- This lesson covered two fundamental graph representations: the adjacency matrix and the adjacency list. We discussed their properties, advantages, and disadvantages, and provided code examples in Python. The choice of representation depends on the specific application and the characteristics of the graph, particularly its density. Understanding these trade-offs is crucial for designing efficient graph algorithms.
		- Next steps involve exploring fundamental graph traversal algorithms like Depth-First Search (DFS) and Breadth-First Search (BFS), which are covered in the next lesson. These algorithms rely on efficient graph representations to explore the connectivity and structure of graphs.