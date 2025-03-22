tags:: data-structures, algorithms, bubble-sort, sorting-algorithms

- category:: [[Data Structures and Algorithms]]
- title:: Bubble Sort
- algorithm-type:: #sorting-algorithm
- # Bubble Sort
	- Bubble sort goes through each array and takes an element, compares it to the next and switches them if it is less then the next element
- # Bubble Sort Diagram
	- {{renderer excalidraw, excalidraw-2025-03-21-08-20-46}}
- # Bubble Sort Code #card
  collapsed:: true
	- ```python
	  def bubble_sort(arr):
	      n = len(arr)
	      for i in range(n):
	          swapped = False
	          for j in range(0, n-i-1):
	              if arr[j] > arr[j+1]:
	                  arr[j], arr[j+1] = arr[j+1], arr[j]
	                  swapped = True
	          if not swapped:
	              break
	      return arr
	  
	  print(bubble_sort([2, 4, 7, 9, 1, 3, 5]))
	  ```
	-
- ## What type of use case is bubble sort useful for #card
  collapsed:: true
	- Bubble Sort's unique iterative process and the concept of "bubbling up" make it a reliable and efficient algorithm for sorting elements in ascending order.
- ## Bubble Sort Time Complexity #card
  collapsed:: true
	- Bubble Sort is a sorting algorithm with a worst-case and average-case time complexity of $O(n^2)$, where $(n)$ represents the number of items being sorted. Although this time complexity might seem inefficient, Bubble Sort has an advantage in that its best-case time complexity (when the list is already sorted) is $O(n)$.
	- ## Bubble Sort Advantages and Efficiency #card
	- This improved version of Bubble Sort checks if any swaps occurred, resulting in a more optimized performance. Additionally, Bubble Sort is a simple and easy-to-understand algorithm, making it a suitable choice for small or nearly sorted lists. It is also a stable sorting algorithm, meaning that the relative order of equal elements is preserved during the sorting process.
	- Overall, while Bubble Sort may not be the most efficient algorithm for large datasets, its simplicity and optimized best-case time complexity make it a viable option for certain scenarios.