# Dynamic Programming: Principles and Applications
	- Dynamic programming is a powerful algorithmic technique used to solve optimization problems by breaking them down into smaller, overlapping subproblems, storing the solutions to these subproblems, and reusing them to find the overall solution. This approach avoids redundant computations, leading to significant efficiency gains, especially for problems with overlapping subproblems and optimal substructure. Understanding dynamic programming is crucial for any developer aiming to tackle complex computational challenges efficiently.
	- ## Core Principles of Dynamic Programming
		- Dynamic programming relies on two key properties for it to be applicable and effective: _overlapping subproblems_ and _optimal substructure_.
		- ### Overlapping Subproblems
			- A problem is said to have overlapping subproblems if it can be broken down into subproblems which are reused multiple times. In other words, the same subproblems are solved repeatedly. This is in contrast to divide and conquer algorithms like merge sort or quicksort, where subproblems are independent.
			- _Example:_ Consider calculating the nth Fibonacci number. The Fibonacci sequence is defined as F(n) = F(n-1) + F(n-2), with base cases F(0) = 0 and F(1) = 1. A naive recursive implementation would repeatedly calculate the same Fibonacci numbers, leading to exponential time complexity. Dynamic programming avoids this by storing the results of subproblems (Fibonacci numbers) and reusing them when needed.
			- _Non-Example:_ Merge sort does _not_ exhibit overlapping subproblems. When sorting two halves of an array, the subproblems (sorting each half) are independent and do not share any further subproblems.
		- ### Optimal Substructure
			- A problem exhibits optimal substructure if an optimal solution to the problem can be constructed from optimal solutions to its subproblems. This means that the optimal solution to the overall problem incorporates the optimal solutions to its constituent subproblems.
			- _Example:_ Consider the shortest path problem. If the shortest path from A to B goes through C, then the path from A to C must be the shortest path from A to C, and the path from C to B must be the shortest path from C to B. This is optimal substructure.
			- _Non-Example:_ Consider finding the longest path between two nodes in a graph. While you can break the problem into subproblems, finding the longest path from A to C and C to B does not guarantee that combining these paths will result in the longest path from A to B. The longest path problem, in general, does not exhibit optimal substructure (unless the graph is a directed acyclic graph).
		- ### Memoization (Top-Down) vs. Tabulation (Bottom-Up)
			- Dynamic programming problems can be solved using two main approaches: memoization (top-down) and tabulation (bottom-up).
			- #### Memoization (Top-Down)
				- Memoization involves solving the problem recursively, but storing the results of subproblems in a table (usually a hash map or an array) to avoid recomputation. When the function is called with the same input again, it simply retrieves the stored result.
				- _Example:_ Fibonacci sequence with memoization (Python):
				- ```
				  def fibonacci_memoization(n, memo={}):
				      if n in memo:
				          return memo[n]
				      if n <= 1:
				          return n
				      memo[n] = fibonacci_memoization(n-1, memo) + fibonacci_memoization(n-2, memo)
				      return memo[n]
				  # Example usage:
				  print(fibonacci_memoization(10)) # Output: 55
				  ```
				- In this example, the `memo` dictionary stores the calculated Fibonacci numbers. Before computing `fibonacci_memoization(n)`, the function checks if the result is already stored in `memo`. If it is, the stored value is returned directly. Otherwise, the value is computed, stored in `memo`, and then returned.
			- #### Tabulation (Bottom-Up)
				- Tabulation involves building a table of solutions to subproblems in a bottom-up fashion. Starting with the base cases, the table is filled in iteratively until the solution to the original problem is reached.
				- _Example:_ Fibonacci sequence with tabulation (Python):
				- ```
				  def fibonacci_tabulation(n):
				      if n <= 1:
				          return n
				      # Initialize a table to store Fibonacci numbers
				      fib_table = [0] * (n + 1)
				      # Base cases
				      fib_table[0] = 0
				      fib_table[1] = 1
				      # Fill the table iteratively
				      for i in range(2, n + 1):
				          fib_table[i] = fib_table[i-1] + fib_table[i-2]
				      return fib_table[n]
				  # Example usage:
				  print(fibonacci_tabulation(10)) # Output: 55
				  ```
				- In this example, `fib_table` is an array that stores the Fibonacci numbers from 0 to n. The base cases `fib_table[0]` and `fib_table[1]` are initialized, and then the table is filled iteratively using the Fibonacci recurrence relation.
			- #### Comparison
				- | Feature | Memoization (Top-Down) | Tabulation (Bottom-Up) |
				  | --- | --- | --- |
				  | Approach | Recursive | Iterative |
				  | Order | Solves subproblems as needed | Solves all subproblems in a specific order |
				  | Implementation | Easier to implement for some problems | Can be more efficient due to no recursion overhead |
				  | Space Complexity | Can use more space due to recursion stack | Can sometimes be optimized to use less space |
				- Choosing between memoization and tabulation depends on the specific problem and personal preference. Memoization can be more intuitive for some problems, while tabulation can be more efficient due to the absence of recursive function calls.
	- ## Dynamic Programming Applications
		- Dynamic programming is used to solve a wide range of optimization problems. Here are a few examples:
		- ### 1. Knapsack Problem
			- The knapsack problem is a classic optimization problem where you are given a set of items, each with a weight and a value, and a knapsack with a maximum weight capacity. The goal is to determine the subset of items to include in the knapsack that maximizes the total value without exceeding the weight capacity.
			- _Problem Statement:_ Given a set of items, each with a weight `w_i` and a value `v_i`, and a knapsack with a maximum weight capacity `W`, find the subset of items that maximizes the total value while keeping the total weight less than or equal to `W`.
			- _Dynamic Programming Approach:_
			- Let `dp[i][w]` be the maximum value that can be obtained using the first `i` items and a knapsack with a weight capacity of `w`. The recurrence relation is as follows:
				- If `w_i > w`: `dp[i][w] = dp[i-1][w]` (the current item cannot be included)
				- If `w_i <= w`: `dp[i][w] = max(dp[i-1][w], v_i + dp[i-1][w - w_i])` (either include the current item or exclude it)
			- _Example:_
			- Items:
			- | Item | Weight (w) | Value (v) |
			  | --- | --- | --- |
			  | 1 | 1 | 1 |
			  | 2 | 3 | 4 |
			  | 3 | 4 | 5 |
			  | 4 | 5 | 7 |
			- Knapsack capacity: W = 7
			- _Tabulation (Bottom-Up) Implementation (Python):_
			- ```
			  def knapsack(W, weights, values, n):
			      dp = [[0 for _ in range(W + 1)] for _ in range(n + 1)]
			      for i in range(n + 1):
			          for w in range(W + 1):
			              if i == 0 or w == 0:
			                  dp[i][w] = 0
			              elif weights[i-1] <= w:
			                  dp[i][w] = max(values[i-1] + dp[i-1][w-weights[i-1]], dp[i-1][w])
			              else:
			                  dp[i][w] = dp[i-1][w]
			      return dp[n][W]
			  # Example usage:
			  values = [1, 4, 5, 7]
			  weights = [1, 3, 4, 5]
			  W = 7
			  n = len(values)
			  print(knapsack(W, weights, values, n)) # Output: 9
			  ```
			- The `knapsack` function constructs a 2D table `dp` where `dp[i][w]` stores the maximum value achievable using the first `i` items with a maximum weight of `w`. The table is filled iteratively, considering whether to include each item or not based on its weight and value.
		- ### 2. Longest Common Subsequence (LCS)
			- The longest common subsequence (LCS) problem involves finding the longest subsequence common to two given sequences. A subsequence is a sequence that can be derived from another sequence by deleting some or no elements without changing the order of the remaining elements.
			- _Problem Statement:_ Given two sequences, `X` and `Y`, find the length of the longest subsequence common to both.
			- _Dynamic Programming Approach:_
			- Let `dp[i][j]` be the length of the LCS of the first `i` characters of `X` and the first `j` characters of `Y`. The recurrence relation is as follows:
				- If `X[i] == Y[j]`: `dp[i][j] = dp[i-1][j-1] + 1` (the last characters match, so include them in the LCS)
				- If `X[i] != Y[j]`: `dp[i][j] = max(dp[i-1][j], dp[i][j-1])` (the last characters do not match, so take the maximum LCS length from the previous subproblems)
			- _Example:_
			- X = "AGGTAB" Y = "GXTXAYB"
			- _Tabulation (Bottom-Up) Implementation (Python):_
			- ```
			  def longest_common_subsequence(X, Y):
			      m = len(X)
			      n = len(Y)
			      dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
			      for i in range(m + 1):
			          for j in range(n + 1):
			              if i == 0 or j == 0:
			                  dp[i][j] = 0
			              elif X[i-1] == Y[j-1]:
			                  dp[i][j] = dp[i-1][j-1] + 1
			              else:
			                  dp[i][j] = max(dp[i-1][j], dp[i][j-1])
			      return dp[m][n]
			  # Example usage:
			  X = "AGGTAB"
			  Y = "GXTXAYB"
			  print(longest_common_subsequence(X, Y)) # Output: 4
			  ```
			- The `longest_common_subsequence` function constructs a 2D table `dp` where `dp[i][j]` stores the length of the LCS of `X[0...i-1]` and `Y[0...j-1]`. The table is filled iteratively, considering whether the characters at the current positions match or not.
		- ### 3. Edit Distance
			- The edit distance problem involves finding the minimum number of operations (insertions, deletions, or substitutions) required to transform one string into another.
			- _Problem Statement:_ Given two strings, `str1` and `str2`, find the minimum number of edits (insertions, deletions, or substitutions) needed to convert `str1` into `str2`.
			- _Dynamic Programming Approach:_
			- Let `dp[i][j]` be the edit distance between the first `i` characters of `str1` and the first `j` characters of `str2`. The recurrence relation is as follows:
				- If `str1[i] == str2[j]`: `dp[i][j] = dp[i-1][j-1]` (no cost if the characters match)
				- If `str1[i] != str2[j]`: `dp[i][j] = min(dp[i-1][j] + 1, dp[i][j-1] + 1, dp[i-1][j-1] + 1)` (take the minimum cost of insertion, deletion, or substitution)
			- _Example:_
			- str1 = "sunday" str2 = "saturday"
			- _Tabulation (Bottom-Up) Implementation (Python):_
			- ```
			  def edit_distance(str1, str2):
			      m = len(str1)
			      n = len(str2)
			      dp = [[0 for _ in range(n + 1)] for _ in range(m + 1)]
			      for i in range(m + 1):
			          for j in range(n + 1):
			              if i == 0:
			                  dp[i][j] = j  # Min. operations = j (insert all characters of str2)
			              elif j == 0:
			                  dp[i][j] = i  # Min. operations = i (remove all characters of str1)
			              elif str1[i-1] == str2[j-1]:
			                  dp[i][j] = dp[i-1][j-1]
			              else:
			                  dp[i][j] = 1 + min(dp[i-1][j],      # Deletion
			                                     dp[i][j-1],      # Insertion
			                                     dp[i-1][j-1])    # Substitution
			      return dp[m][n]
			  # Example usage:
			  str1 = "sunday"
			  str2 = "saturday"
			  print(edit_distance(str1, str2)) # Output: 3
			  ```
			- The `edit_distance` function constructs a 2D table `dp` where `dp[i][j]` stores the edit distance between `str1[0...i-1]` and `str2[0...j-1]`. The table is filled iteratively, considering the costs of insertion, deletion, and substitution.
	- ## Exercises
		- 1.  **Rod Cutting:** Given a rod of length _n_ inches and an array of prices that contains prices of all pieces of size smaller than _n_. Determine the maximum obtainable value by cutting up the rod and selling the pieces. For example, if the length of the rod is 8 and the values of different pieces are given as follows, then the maximum obtainable value is 22 (by cutting in two pieces of lengths 2 and 6)
			- ```
			      length   | 1   2   3   4   5   6   7   8
			      --------------------------------------------
			      price    | 1   5   8   9  10  17  17  20
			    ```
		- 2.  **Matrix Chain Multiplication:** You are given a sequence of matrices. Find the most efficient way to multiply these matrices together. The problem is not actually to perform the multiplications, just to decide the order of multiplications.
		- 3.  **Coin Change:** Given a set of coin denominations and a target amount, find the minimum number of coins needed to make up that amount.
	- ## Summary and Next Steps
		- This lesson covered the fundamental principles of dynamic programming, including overlapping subproblems and optimal substructure. We explored the two main approaches to dynamic programming: memoization (top-down) and tabulation (bottom-up), and illustrated these concepts with examples like the Fibonacci sequence, Knapsack problem, Longest Common Subsequence, and Edit Distance.
		- Next, we will explore Greedy Algorithms, another important algorithm design paradigm. Understanding the differences between dynamic programming and greedy algorithms is crucial for choosing the right approach for a given problem. We will also continue to build upon the concepts learned in this module as we move forward in the course.