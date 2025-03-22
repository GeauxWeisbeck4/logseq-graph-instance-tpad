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
- # Selection Sort Advantages #card
	- One of the main advantages of using the Selection Sort algorithm is its ability to effectively minimize the number of swaps required to sort the list. By performing only $n-1$ swaps, where $n$ represents the total number of elements in the list, the Selection Sort guarantees that the final result will be a fully sorted list. This efficient approach makes the Selection Sort algorithm particularly suitable for sorting small to medium-sized lists, especially in scenarios where minimizing the number of swaps is crucial.
	- In addition to its efficiency, the Selection Sort algorithm also offers simplicity and ease of implementation. Its clear and straightforward logic makes it accessible even to those who are new to sorting algorithms. Therefore, the Selection Sort algorithm is often preferred when dealing with smaller lists and when the focus is on reducing the number of swaps required to achieve a sorted outcome.
- # Selection Sort Performance
	- When it comes to the performance of Selection Sort, it is important to note that regardless of the input size, it always takes $O(n^2)$ time for both the average and worst-case scenarios. This happens because, for each element in the list, the algorithm searches for the minimum value among the remaining elements.This search process contributes to the overall time complexity of the algorithm. Therefore, even if the input is sorted or partially sorted, Selection Sort still has to compare each element with the rest of the list, leading to a quadratic time complexity.