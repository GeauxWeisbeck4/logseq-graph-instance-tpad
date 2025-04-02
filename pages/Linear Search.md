tags:: data-structures, algorithms, dsa, searching-operations, efficiency, linear-search

- title:: [[Linear Search]]
- category:: [[Data Structures and Algorithms]]
- algorithm-type:: [[Searching Operations and Efficiency]]
- # Linear Search
	- A simple search method that goes through each item in a data set and stop when it locates element we are searching for.
	- Simple
	- Reliable
	- Slow and inefficient
- ## `linear_search.py`
	- ```python
	  def linear_search(arr, x):
	  	for i in range(len(arr)):
	      	if arr[i] == x:
	          	return i
	      return -1
	  ```
	-