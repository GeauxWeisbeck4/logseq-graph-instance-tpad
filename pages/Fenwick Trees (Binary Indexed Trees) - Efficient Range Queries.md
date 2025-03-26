# Fenwick Trees (Binary Indexed Trees): Efficient Range Queries
	- Fenwick Trees, also known as Binary Indexed Trees (BIT), provide an efficient way to calculate prefix sums and perform updates on an array. Unlike segment trees, Fenwick Trees generally require less space and are simpler to implement, making them a valuable tool for solving range query problems. This lesson will delve into the structure, implementation, and applications of Fenwick Trees, focusing on how they enable efficient range queries and updates.
	- ## Core Concepts of Fenwick Trees
		- A Fenwick Tree is a data structure that efficiently calculates prefix sums in an array. It allows for both point updates (modifying a single element) and prefix sum queries (calculating the sum of elements from the beginning of the array up to a given index) in logarithmic time.
		- ### Prefix Sums
			- The fundamental concept behind Fenwick Trees is the efficient computation of prefix sums. Given an array `arr`, the prefix sum at index `i` is the sum of all elements from `arr[0]` to `arr[i]`. A naive approach to calculating prefix sums would involve iterating through the array for each query, resulting in O(n) time complexity for each query. Fenwick Trees optimize this process.
			- _Example:_
			- Consider the array `arr = [2, 1, 1, 3, 2, 3, 4, 5, 6, 7, 8, 9]`. The prefix sum at index 5 would be `2 + 1 + 1 + 3 + 2 + 3 = 12`.
		- ### Binary Representation and Tree Structure
			- Fenwick Trees leverage the binary representation of indices to create a tree-like structure implicitly represented within an array. Each index in the Fenwick Tree stores the sum of a specific range of elements from the original array. The size of this range is determined by the lowest significant bit (LSB) of the index.
			- _Example:_
			- Let's consider the index 12 (binary representation: 1100). The lowest significant bit is 100 (4 in decimal). This means that the Fenwick Tree node at index 12 stores the sum of the elements from index 8 (12 - 4) to index 12 in the original array.
		- ### Calculating the Lowest Significant Bit (LSB)
			- The LSB of a number `x` can be calculated using the bitwise AND operation: `x & -x`. This operation isolates the rightmost set bit.
			- _Example:_
			- For `x = 12` (binary: 1100), `-x` in two's complement is (binary: ...10100). `x & -x` results in (binary: 0000...00100), which is 4.
		- ### Fenwick Tree Array
			- The Fenwick Tree is typically implemented using an array, often named `BIT` or `tree`. The size of this array is `n + 1`, where `n` is the size of the original array. The index 0 is usually unused.
			- _Example:_
			- For the array `arr = [2, 1, 1, 3, 2, 3, 4, 5, 6, 7, 8, 9]`, the Fenwick Tree array `BIT` would have a size of 13.
	- ## Operations on Fenwick Trees
		- ### Update Operation
			- The update operation involves modifying the value of an element in the original array and propagating this change to the relevant nodes in the Fenwick Tree. To update the Fenwick Tree, we start at the index corresponding to the element being modified and iteratively add the change to all parent nodes. The parent nodes are found by repeatedly adding the LSB to the current index.
			- _Algorithm:_
				- 1.  Let `index` be the index of the element to be updated in the original array (1-based index).
				- 2.  Let `value` be the amount by which the element is being changed (the difference between the new value and the old value).
				- 3.  While `index` is within the bounds of the Fenwick Tree array:
					- Add `value` to `BIT[index]`.
					- Update `index` by adding `index & -index` to it.
			- _Example:_
			- Suppose we want to update the value at index 2 in the original array `arr` from 1 to 5 (a change of +4).
				- 1.  Start at `index = 2`. `value = 4`.
				- 2.  `BIT[2] = BIT[2] + 4`.
				- 3.  `index = 2 + (2 & -2) = 2 + 2 = 4`.
				- 4.  `BIT[4] = BIT[4] + 4`.
				- 5.  `index = 4 + (4 & -4) = 4 + 4 = 8`.
				- 6.  `BIT[8] = BIT[8] + 4`.
				- 7.  `index = 8 + (8 & -8) = 8 + 8 = 16`. Since 16 is out of bounds (greater than the size of `BIT`), the update operation stops.
		- ### Query Operation
			- The query operation calculates the prefix sum up to a given index. To query the Fenwick Tree, we start at the given index and iteratively add the values of the relevant nodes. The relevant nodes are found by repeatedly subtracting the LSB from the current index.
			- _Algorithm:_
				- 1.  Let `index` be the index up to which the prefix sum is to be calculated (1-based index).
				- 2.  Initialize `sum = 0`.
				- 3.  While `index` is greater than 0:
					- Add `BIT[index]` to `sum`.
					- Update `index` by subtracting `index & -index` from it.
				- 4.  Return `sum`.
			- _Example:_
			- Suppose we want to calculate the prefix sum up to index 7 in the original array `arr`.
				- 1.  Start at `index = 7`. `sum = 0`.
				- 2.  `sum = sum + BIT[7]`.
				- 3.  `index = 7 - (7 & -7) = 7 - 1 = 6`.
				- 4.  `sum = sum + BIT[6]`.
				- 5.  `index = 6 - (6 & -6) = 6 - 2 = 4`.
				- 6.  `sum = sum + BIT[4]`.
				- 7.  `index = 4 - (4 & -4) = 4 - 4 = 0`. Since 0 is not greater than 0, the query operation stops.
				- 8.  Return `sum`.
	- ## Code Implementation (Python)
		- ```
		  class FenwickTree:
		      def __init__(self, n):
		          self.n = n
		          self.bit = [0] * (n + 1)  # 1-indexed array
		      def update(self, index, value):
		          # index in BIT is 1-based
		          index += 1
		          while index <= self.n:
		              self.bit[index] += value
		              index += index & -index
		      def query(self, index):
		          # index in BIT is 1-based
		          index += 1
		          sum_val = 0
		          while index > 0:
		              sum_val += self.bit[index]
		              index -= index & -index
		          return sum_val
		  # Example Usage:
		  arr = [2, 1, 1, 3, 2, 3, 4, 5, 6, 7, 8, 9]
		  n = len(arr)
		  ft = FenwickTree(n)
		  # Initialize Fenwick Tree with values from arr
		  for i in range(n):
		      ft.update(i, arr[i])
		  # Query prefix sum up to index 5
		  print(f"Prefix sum up to index 5: {ft.query(5)}")  # Output: 12
		  # Update value at index 2 from 1 to 5
		  ft.update(2, 5 - 1)
		  # Query prefix sum up to index 5 again
		  print(f"Prefix sum up to index 5 after update: {ft.query(5)}")  # Output: 16
		  ```
	- ## Exercises
		- 1.  **Range Sum Query:** Implement a function to calculate the sum of elements within a given range (left, right) using the Fenwick Tree. Remember that you can calculate the range sum by subtracting prefix sums.
		- 2.  **Array Initialization:** Modify the FenwickTree class to accept an initial array during construction and initialize the BIT array accordingly.
		- 3.  **Point Query:** Implement a function to query the value of a single element at a given index using the Fenwick Tree.
		- 4.  **Negative Values:** Adapt the Fenwick Tree implementation to handle arrays with negative values.
		- 5.  **Performance Comparison:** Compare the performance of calculating prefix sums and range sums using a Fenwick Tree versus a naive approach (iterating through the array). Use arrays of different sizes to observe the difference in execution time.
	- ## Summary and Next Steps
		- This lesson covered the core concepts of Fenwick Trees, including their structure, update operation, and query operation. You learned how to implement a Fenwick Tree in code and apply it to solve range query problems. You also worked through exercises to solidify your understanding.
		- Next, we will explore how to apply tree structures to the social media feed case study introduced in Module 1. This will involve using the data structures and algorithms learned in this module to optimize the performance and functionality of the feed.