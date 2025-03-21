tags:: data-structures, algorithms, bubble-sort, sorting-algorithms

- Bubble sort goes through each array and takes an element, compares it to the next and switches them if it is less then the next element
- https://excalidraw.com/#json=BNQ95D0qS-u3XOoTy4Zjy,o2rJESYbYjUay8Xc8S_4ZQ
- # Bubble Sort Diagram #card
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