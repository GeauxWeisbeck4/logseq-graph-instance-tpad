tags:: data-structures, algorithms, searching-operations, efficiency, binary-search

- title:: [[Binary Search]]
- category:: [[Data Structures and Algorithms]]
- algorithm-type:: [[Searching Operations and Efficiency]]
- # Binary Search #card
	- Uses divide and conquer to divide a sorted list or array in two and narrow down the space where the element we are looking for is located.
	- Complex
	- Efficient
- # `binary_search.py` #card
	- ```python
	  def binary_search(arr, x):
	      l,r=0, len(arr)-1
	      while l <= r:
	          mid = (l+r) // 2
	          if arr[mid] == x:
	              return mid
	          elif arr[mid] < x:
	              l = mid+1
	          else:
	              r = mid-1
	      return -1
	  
	  arr = [2,4,7,9,11,15]
	  x = 7
	  result = binary_search(arr, x)
	  if result != -1:
	      print(f"Element {x} is present at index {result}")
	  else:
	      print(f"Element {x} is not present in array")
	  ```