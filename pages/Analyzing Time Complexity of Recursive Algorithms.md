# Analyzing Time Complexity of Recursive Algorithms
	- Analyzing the time complexity of recursive algorithms is crucial for understanding their efficiency and scalability. Unlike iterative algorithms where we can often directly count the number of operations, recursive algorithms require a different approach due to their self-referential nature. This lesson will equip you with the tools and techniques to analyze the time complexity of recursive algorithms, preparing you for more advanced algorithm design paradigms like divide and conquer and dynamic programming.
	- ## Understanding Recursion and Time Complexity
		- Recursion is a powerful technique where a function calls itself to solve smaller subproblems of the same type. A recursive function typically has two parts:
			- **Base Case:** A condition that stops the recursion and provides a direct solution.
			- **Recursive Step:** The function calls itself with a modified input, moving closer to the base case.
		- The time complexity of a recursive algorithm depends on:
			- 1.  The amount of work done in each recursive call (excluding the recursive calls themselves).
			- 2.  The number of recursive calls made.
		- ### The Substitution Method
			- The substitution method involves the following steps:
				- 1.  **Write a Recurrence Relation:** Express the time complexity _T(n)_ of the recursive algorithm as a function of the input size _n_. This relation will typically involve _T(n/b)_, where _b_ is a factor by which the input size is reduced in each recursive call, and some function _f(n)_ representing the work done outside the recursive calls.
				- 2.  **Guess a Solution:** Based on the recurrence relation, make an educated guess about the form of the solution (e.g., _O(n)_, _O(n log n)_, _O(n2)_).
				- 3.  **Prove by Induction:** Use mathematical induction to prove that your guess is correct. This involves showing that the base case holds and that if the guess holds for smaller values of _n_, it also holds for _n_.
			- **Example: Factorial Function**
			- Consider the recursive factorial function:
			- ```
			  def factorial(n):
			    """
			    Calculates the factorial of a non-negative integer n recursively.
			    Args:
			      n: A non-negative integer.
			    Returns:
			      The factorial of n.
			    """
			    if n == 0:
			      return 1  # Base case
			    else:
			      return n * factorial(n-1)  # Recursive step
			  ```
				- 1.  **Recurrence Relation:**
					- _T(n) = T(n-1) + O(1)_ (One recursive call with input size _n-1_, plus constant time for multiplication and comparison)
					- _T(0) = O(1)_ (Base case takes constant time)
				- 2.  **Guess a Solution:** We guess that _T(n) = O(n)_.
				- 3.  **Prove by Induction:**
					- **Base Case:** For _n = 0_, _T(0) = O(1)_, which satisfies _O(n)_.
					- **Inductive Hypothesis:** Assume _T(k) <= c_k* for some constant _c_ and all _k < n_.
					- **Inductive Step:** We need to show that _T(n) <= c_n*.
					- _T(n) = T(n-1) + O(1)_ _T(n) <= c(n-1) + c'_ (where _c'_ is a constant representing the _O(1)_ work) _T(n) <= cn - c + c'_
					- If we choose _c_ large enough such that _-c + c' <= 0_, then _T(n) <= cn_. Therefore, _T(n) = O(n)_.
			- **Example: Fibonacci Sequence**
			- Consider the recursive Fibonacci sequence function:
			- ```
			  def fibonacci(n):
			    """
			    Calculates the nth Fibonacci number recursively.
			    Args:
			      n: A non-negative integer.
			    Returns:
			      The nth Fibonacci number.
			    """
			    if n <= 1:
			      return n  # Base case
			    else:
			      return fibonacci(n-1) + fibonacci(n-2)  # Recursive step
			  ```
				- 1.  **Recurrence Relation:**
					- _T(n) = T(n-1) + T(n-2) + O(1)_ (Two recursive calls with input sizes _n-1_ and _n-2_, plus constant time for addition and comparison)
					- _T(0) = O(1)_
					- _T(1) = O(1)_
				- 2.  **Guess a Solution:** This is trickier. Since we have two recursive calls, the time complexity will be exponential. We guess that _T(n) = O(2n)_.
				- 3.  **Prove by Induction:**
					- **Base Cases:** For _n = 0_ and _n = 1_, _T(0) = O(1)_ and _T(1) = O(1)_, which satisfy _O(2n)_.
					- **Inductive Hypothesis:** Assume _T(k) <= c_2k* for some constant _c_ and all _k < n_.
					- **Inductive Step:** We need to show that _T(n) <= c_2n*.
					- _T(n) = T(n-1) + T(n-2) + O(1)_ _T(n) <= c_2n-1 + c_2n-2 + c'_ (where _c'_ is a constant representing the _O(1)_ work) _T(n) <= c_2n/2 + c_2n/4 + c'_ _T(n) <= c_2n(3/4) + c'*
					- If we choose _c_ large enough such that _c'_ is dominated by _c_2n/4*, then _T(n) <= c_2n*. Therefore, _T(n) = O(2n)_.
		- ### The Recursion Tree Method
			- The recursion tree method provides a visual way to understand the time complexity of a recursive algorithm.
				- 1.  **Draw the Recursion Tree:** Represent each recursive call as a node in a tree. The root node represents the initial call, and the children of each node represent the recursive calls made by that node.
				- 2.  **Calculate the Work at Each Level:** Determine the amount of work done at each level of the tree (excluding the recursive calls themselves).
				- 3.  **Sum the Work Across All Levels:** Sum the work done at each level to obtain the total time complexity.
			- **Example: Merge Sort (Preview)**
			- While a full discussion of Merge Sort is in a later lesson, we can use it as an example to illustrate the recursion tree method. Merge Sort recursively divides an array into two halves until each subarray contains only one element. Then, it merges the subarrays in sorted order.
				- 1.  **Recursion Tree:** The recursion tree for Merge Sort is a binary tree where each node represents a call to the `mergeSort` function.
				- 2.  **Work at Each Level:** At each level, the `merge` function performs _O(n)_ work to merge the subarrays.
				- 3.  **Sum Across Levels:** The height of the tree is _O(log n)_, and each level performs _O(n)_ work. Therefore, the total time complexity is _O(n log n)_.
			- This example previews the divide and conquer paradigm, which will be covered in detail in the next lesson.
		- ### The Master Theorem
			- The Master Theorem provides a cookbook solution for recurrence relations of the form:
			- _T(n) = aT(n/b) + f(n)_
			- where:
				- _a_ >= 1 is the number of subproblems in the recursion.
				- _b_ > 1 is the factor by which the subproblem size is reduced in each recursive call.
				- _f(n)_ is the work done outside the recursive calls.
			- The Master Theorem has three cases:
				- 1.  **Case 1:** If _f(n) = O(nlogba - ε)_ for some constant _ε > 0_, then _T(n) = Θ(nlogba)_.
				- 2.  **Case 2:** If _f(n) = Θ(nlogba)_, then _T(n) = Θ(nlogba log n)_.
				- 3.  **Case 3:** If _f(n) = Ω(nlogba + ε)_ for some constant _ε > 0_, and if _a f(n/b) <= c f(n)_ for some constant _c < 1_ and all sufficiently large _n_, then _T(n) = Θ(f(n))_.
			- **Example: Applying the Master Theorem**
			- Consider the recurrence relation _T(n) = 2T(n/2) + n_.
			- Here, _a = 2_, _b = 2_, and _f(n) = n_.
			- _nlogba = nlog22 = n1 = n_.
			- Since _f(n) = Θ(nlogba)_, we are in Case 2.
			- Therefore, _T(n) = Θ(n log n)_.
			- **Example: Another Application**
			- Consider the recurrence relation _T(n) = 4T(n/2) + n_.
			- Here, _a = 4_, _b = 2_, and _f(n) = n_.
			- _nlogba = nlog24 = n2_.
			- Since _f(n) = O(n2 - ε)_ for _ε = 1_, we are in Case 1.
			- Therefore, _T(n) = Θ(n2)_.
	- ## Practice Activities
		- 1.  **Binary Search:** Analyze the time complexity of a recursive binary search algorithm using the substitution method and the Master Theorem. Write the recurrence relation, guess the solution, and prove it by induction.
		- 2.  **Tower of Hanoi:** Analyze the time complexity of the Tower of Hanoi puzzle using the recursion tree method. Draw the recursion tree and calculate the work at each level.
		- 3.  **Modified Fibonacci:** Consider a modified Fibonacci sequence where each number is the sum of the previous three numbers. Write a recursive function to calculate the nth number in this sequence and analyze its time complexity.
		- 4.  **Merge Sort Recurrence:** Verify the time complexity of Merge Sort, _T(n) = 2T(n/2) + O(n)_, using the substitution method.
	- ## Summary and Next Steps
		- This lesson covered techniques for analyzing the time complexity of recursive algorithms, including the substitution method, the recursion tree method, and the Master Theorem. Understanding these techniques is essential for designing efficient algorithms, especially when dealing with recursive solutions.
		- In the next lesson, we will explore the divide and conquer paradigm, which relies heavily on recursion. We will examine specific algorithms like Merge Sort and Quick Sort and apply the techniques learned in this lesson to analyze their time complexity.