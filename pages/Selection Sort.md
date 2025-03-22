tags:: algorithms, data-structures, sorting-algorithms, selection-sort

- category:: [[Data Structures and Algorithms]]
- title:: [[Selection Sort]]
- algorithm-type:: #sorting-algorithm
- # Selection Sort #card
	- Selection Sort, a straightforward and efficient sorting method, operates by consistently choosing the smallest (or largest for descending order) item from the unsorted portion of a list and swapping it with the first unsorted element. This method is repeated until the entire list is in order. The algorithm works by methodically comparing and swapping elements, ensuring that each item, whether smallest or largest, is placed in its proper position. Its simplicity and ease of implementation make it an excellent option for those just starting with sorting algorithms.
	-
- # Selection Sort Code #card
  collapsed:: true
	- ```python
	  def selection_sort(arr):
	      n = len(arr)
	      for i in range(n):
	          min_idx = i
	          for j in range(i+1, n):
	              if arr[j] < arr[min_idx]:
	                  min_idx = j
	          arr[i], arr[min_idx] = arr[min_idx], arr[i]
	      return arr
	  ```
- # Selection Sort Use Cases #card
  collapsed:: true
	- The Selection Sort is a well-known sorting algorithm that is used to arrange a list of elements in either ascending or descending order. It achieves this by repetitively selecting the smallest (or largest, depending on the desired sorting order) element from the unsorted portion of the list and swapping it with the element in its correct position. This process continues until the entire list is sorted.
-