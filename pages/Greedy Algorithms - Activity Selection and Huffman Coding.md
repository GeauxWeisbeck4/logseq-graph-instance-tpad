# Greedy Algorithms: Activity Selection and Huffman Coding
	- Greedy algorithms are a powerful paradigm for solving optimization problems. They make locally optimal choices at each step with the hope of finding a global optimum. While this approach doesn't always guarantee the best solution, it often provides a good approximation and is computationally efficient. This lesson will explore the greedy approach through two classic examples: the Activity Selection Problem and Huffman Coding. Understanding these examples will provide a solid foundation for recognizing and applying greedy algorithms in various problem-solving scenarios.
	- ## Greedy Algorithms: Core Principles
		- Greedy algorithms operate on the principle of making the "best" choice at each stage of the algorithm, without regard to future consequences. This "best" choice is determined by a specific criterion, which is crucial to the algorithm's success.
		- ### Key Characteristics
			- **Optimal Substructure:** A problem exhibits optimal substructure if an optimal solution to the problem contains within it optimal solutions to subproblems. Greedy algorithms rely on this property, assuming that by making the best local choice, we are moving towards the global optimum.
			- **Greedy Choice Property:** The greedy choice property states that a globally optimal solution can be arrived at by making a locally optimal (greedy) choice. In other words, the choice made by a greedy algorithm should lead to an optimal solution for the subproblem at hand.
			- **No backtracking:** Once a choice is made, it is final. The algorithm does not reconsider previous choices, even if they later prove to be suboptimal. This is what makes greedy algorithms efficient, but also limits their applicability.
		- ### When to Use Greedy Algorithms
			- Greedy algorithms are suitable when:
				- The problem exhibits optimal substructure and the greedy choice property.
				- A near-optimal solution is acceptable, and computational efficiency is a priority.
				- Other approaches, like dynamic programming, are too complex or computationally expensive.
			- However, it's crucial to remember that not all optimization problems can be solved optimally using a greedy approach. It's essential to prove that the greedy choice leads to a globally optimal solution before applying a greedy algorithm.
	- ## Activity Selection Problem
		- The Activity Selection Problem is a classic example of how a greedy algorithm can be used to solve an optimization problem. The problem involves selecting the maximum number of mutually compatible activities from a set of activities, given their start and finish times.
		- ### Problem Definition
			- Given a set _S_ of _n_ activities with start times _si_ and finish times _fi_, select a maximum-size subset of mutually compatible activities. Activities _i_ and _j_ are compatible if their intervals [_si_, _fi_) and [_sj_, _fj_) do not overlap.
		- ### Greedy Approach
			- The greedy approach to the Activity Selection Problem involves the following steps:
				- 1.  **Sort:** Sort the activities by their finish times in ascending order.
				- 2.  **Select:** Select the first activity in the sorted list.
				- 3.  **Iterate:** Iterate through the remaining activities, selecting an activity if its start time is greater than or equal to the finish time of the previously selected activity.
		- ### Example
			- Let's consider the following set of activities:
			- | Activity | Start Time | Finish Time |
			  | --- | --- | --- |
			  | A | 1 | 4 |
			  | B | 3 | 5 |
			  | C | 0 | 6 |
			  | D | 5 | 7 |
			  | E | 3 | 9 |
			  | F | 6 | 10 |
			  | G | 8 | 11 |
				- 1.  **Sort by Finish Time:**
					- | Activity | Start Time | Finish Time |
					    | --- | --- | --- |
					    | A | 1 | 4 |
					    | B | 3 | 5 |
					    | C | 0 | 6 |
					    | D | 5 | 7 |
					    | E | 3 | 9 |
					    | F | 6 | 10 |
					    | G | 8 | 11 |
				- 2.  **Select Activity A (finish time 4).**
				- 3.  **Iterate and Select:**
					- B (3, 5): Not compatible (3 < 4).
					- C (0, 6): Not compatible (0 < 4).
					- D (5, 7): Compatible (5 >= 4). Select Activity D.
					- E (3, 9): Not compatible (3 < 7).
					- F (6, 10): Not compatible (6 < 7).
					- G (8, 11): Compatible (8 >= 7). Select Activity G.
			- Therefore, the selected activities are A, D, and G. This is one of the optimal solutions, providing a maximum of 3 compatible activities.
		- ### Code Example (Python)
			- ```
			  def activity_selection(activities):
			      """
			      Selects a maximum set of compatible activities using a greedy approach.
			      Args:
			          activities: A list of tuples, where each tuple represents an activity
			                      and contains (start_time, finish_time, activity_name).
			      Returns:
			          A list of activity names representing the selected activities.
			      """
			      # Sort activities by finish time
			      activities.sort(key=lambda x: x[1])
			      selected_activities = []
			      last_finish_time = float('-inf')  # Initialize with negative infinity
			      for activity in activities:
			          start_time, finish_time, activity_name = activity
			          if start_time >= last_finish_time:
			              selected_activities.append(activity_name)
			              last_finish_time = finish_time
			      return selected_activities
			  # Example usage:
			  activities = [
			      (1, 4, 'A'),
			      (3, 5, 'B'),
			      (0, 6, 'C'),
			      (5, 7, 'D'),
			      (3, 9, 'E'),
			      (6, 10, 'F'),
			      (8, 11, 'G')
			  ]
			  selected = activity_selection(activities)
			  print("Selected activities:", selected)  # Output: ['A', 'D', 'G']
			  ```
		- ### Proof of Correctness
			- The greedy approach for the Activity Selection Problem is proven to be optimal. The proof relies on showing that the first activity selected (the one with the earliest finish time) is always part of an optimal solution. This can be proven using an exchange argument.
		- ### Exercise
			- 1.  Consider the following activities with their start and finish times:
				- | Activity | Start Time | Finish Time |
				    | --- | --- | --- |
				    | 1 | 1 | 2 |
				    | 2 | 3 | 4 |
				    | 3 | 0 | 6 |
				    | 4 | 5 | 7 |
				    | 5 | 8 | 9 |
				    | 6 | 5 | 9 |
				- Apply the greedy algorithm to find the maximum number of compatible activities. Show each step.
			- 2.  Modify the Python code to handle cases where activities have the same finish time. How would you prioritize them?
	- ## Huffman Coding
		- Huffman coding is a lossless data compression algorithm that uses a greedy approach to assign variable-length codes to input characters based on their frequencies. More frequent characters get shorter codes, and less frequent characters get longer codes, resulting in efficient compression.
		- ### Problem Definition
			- Given a set of characters and their frequencies, construct a prefix code (a code where no codeword is a prefix of any other codeword) that minimizes the average code length.
		- ### Greedy Approach
			- The Huffman coding algorithm uses a greedy approach to build an optimal prefix code:
				- 1.  **Build a Priority Queue:** Create a priority queue (min-heap) containing all the characters and their frequencies. The character with the lowest frequency has the highest priority.
				- 2.  **Merge Nodes:** While the priority queue contains more than one node:
					- Extract the two nodes with the lowest frequencies (highest priority).
					- Create a new internal node with these two nodes as children. The frequency of the new node is the sum of the frequencies of its children.
					- Insert the new node back into the priority queue.
				- 3.  **Root Node:** The remaining node in the priority queue is the root of the Huffman tree.
				- 4.  **Assign Codes:** Traverse the Huffman tree to assign codes to each character. Assign '0' to the left branch and '1' to the right branch. The code for a character is the sequence of bits encountered along the path from the root to the character.
		- ### Example
			- Let's consider the following characters and their frequencies:
			- | Character | Frequency |
			  | --- | --- |
			  | A | 5 |
			  | B | 9 |
			  | C | 12 |
			  | D | 13 |
			  | E | 16 |
			  | F | 45 |
				- 1.  **Build Priority Queue:** The priority queue initially contains the nodes A(5), B(9), C(12), D(13), E(16), and F(45).
				- 2.  **Merge Nodes:**
					- Extract A(5) and B(9). Create a new node AB(14) and insert it into the queue. The queue now contains AB(14), C(12), D(13), E(16), and F(45).
					- Extract C(12) and D(13). Create a new node CD(25) and insert it into the queue. The queue now contains AB(14), CD(25), E(16), and F(45).
					- Extract AB(14) and E(16). Create a new node ABE(30) and insert it into the queue. The queue now contains ABE(30), CD(25), and F(45).
					- Extract CD(25) and ABE(30). Create a new node ABECD(55) and insert it into the queue. The queue now contains ABECD(55) and F(45).
					- Extract F(45) and ABECD(55). Create a new node FABECD(100). The queue now contains only FABECD(100).
				- 3.  **Root Node:** FABECD(100) is the root of the Huffman tree.
				- 4.  **Assign Codes:**
					- F: 0
					- ABECD: 1
						- A: 1100
						- B: 1101
						- E: 10
						- C: 1110
						- D: 1111
			- Therefore, the Huffman codes are:
			- | Character | Code |
			  | --- | --- |
			  | A | 1100 |
			  | B | 1101 |
			  | C | 1110 |
			  | D | 1111 |
			  | E | 10 |
			  | F | 0 |
		- ### Code Example (Python)
			- ```
			  import heapq
			  class Node:
			      def __init__(self, char, freq):
			          self.char = char
			          self.freq = freq
			          self.left = None
			          self.right = None
			      # Define comparison based on frequency for the priority queue
			      def __lt__(self, other):
			          return self.freq < other.freq
			  def build_huffman_tree(frequencies):
			      """
			      Builds a Huffman tree from a dictionary of character frequencies.
			      Args:
			          frequencies: A dictionary where keys are characters and values are their frequencies.
			      Returns:
			          The root node of the Huffman tree.
			      """
			      # Create a priority queue of nodes
			      heap = [Node(char, freq) for char, freq in frequencies.items()]
			      heapq.heapify(heap)
			      # Merge nodes until only one node remains
			      while len(heap) > 1:
			          node1 = heapq.heappop(heap)
			          node2 = heapq.heappop(heap)
			          merged_node = Node(None, node1.freq + node2.freq)
			          merged_node.left = node1
			          merged_node.right = node2
			          heapq.heappush(heap, merged_node)
			      # The remaining node is the root of the Huffman tree
			      return heap[0]
			  def generate_huffman_codes(node, code="", huffman_codes={}):
			      """
			      Generates Huffman codes by traversing the Huffman tree.
			      Args:
			          node: The current node in the Huffman tree.
			          code: The current code being built.
			          huffman_codes: A dictionary to store the generated Huffman codes.
			      Returns:
			          A dictionary where keys are characters and values are their Huffman codes.
			      """
			      if node is None:
			          return
			      if node.char is not None:
			          huffman_codes[node.char] = code
			          return
			      generate_huffman_codes(node.left, code + "0", huffman_codes)
			      generate_huffman_codes(node.right, code + "1", huffman_codes)
			      return huffman_codes
			  # Example usage:
			  frequencies = {'A': 5, 'B': 9, 'C': 12, 'D': 13, 'E': 16, 'F': 45}
			  root = build_huffman_tree(frequencies)
			  huffman_codes = generate_huffman_codes(root)
			  print("Huffman Codes:", huffman_codes)
			  # Expected output (order may vary):
			  # {'F': '0', 'C': '100', 'D': '101', 'A': '1100', 'B': '1101', 'E': '111'}
			  ```
		- ### Proof of Correctness
			- Huffman's algorithm is proven to produce an optimal prefix code. The proof involves showing that merging the two least frequent characters at each step does not increase the average code length and that any other code can be transformed into the Huffman code without increasing the average code length.
		- ### Exercise
			- 1.  Given the following characters and their frequencies, construct the Huffman tree and determine the Huffman codes for each character:
				- | Character | Frequency |
				    | --- | --- |
				    | X | 2 |
				    | Y | 7 |
				    | Z | 24 |
				    | W | 32 |
			- 2.  Explain how Huffman coding can be used to compress text files. What are the limitations of Huffman coding?
	- ## Summary and Next Steps
		- This lesson explored the greedy algorithm design paradigm through two classic examples: the Activity Selection Problem and Huffman Coding. We examined the core principles of greedy algorithms, including optimal substructure and the greedy choice property. We also saw how to apply greedy algorithms to solve real-world problems and how to prove their correctness.
		- In the next lesson, we will introduce the Case Study, focusing on optimizing a social media feed algorithm. We will explore how the algorithm design paradigms learned in this module can be applied to solve complex, real-world problems. We will also discuss how to analyze the performance of different algorithms and choose the best one for a given task.