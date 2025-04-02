tags:: data-structures, algorithms, dsa, searching-operations, efficiency, linear-search

- title:: [[Linear Search]]
- category:: [[Data Structures and Algorithms]]
- algorithm-type:: [[Searching Operations and Efficiency]]
- # Linear Search #card
	- A simple search method that goes through each item in a data set and stop when it locates element we are searching for.
	- Simple
	- Reliable
	- Slow and inefficient
- ## `linear_search.py` #card
	- ```python
	  def linear_search(arr, x):
	  	for i in range(len(arr)):
	      	if arr[i] == x:
	          	return i
	      return -1
	  ```
- ## Linear Search Example #card
	- ```python
	  arr = [2, 4, 7, 9, 11, 15]
	  x = 7
	  result = linear_search(arr, x)
	  if result != -1:
	    print(f"Element {x} is present at index {result}")
	  else:
	    print(f"Element {x} is not present in array.")
	  
	  # Returns:
	  # Element 7 is present at index 2
	  ```
	-
- # Time Complexity
	- ## Worst Case O(n)
		-