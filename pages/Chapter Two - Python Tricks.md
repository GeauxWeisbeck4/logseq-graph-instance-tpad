id:: 6801bc4f-53de-4f32-bebf-993969de1992
tags:: python, no-starch-press, programming-books, programming

- title:: [[Python One Liners]]
- author:: Christian Mayer
- chapter:: 2
- # Chapter Two - Python Tricks
	- ## List Comprehension
		- Say you work in the human resources department of a large company and
		  need to find all staff members who earn at least $100,000 per year. Your
		  desired output is a list of tuples, each consisting of two values: the employee
		  name and the employee’s yearly salary. Here’s the code you develop:
			- ```python
			  employees = {'Alice' : 100000,
			  			 'Bob' : 99817,
			  			 'Carol' : 122908,
			  			 'Frank' : 88123,
			  			 'Eve' : 93121}
			  top_earners = []
			  for key, val in employees.items():
			  	if val >= 100000:
			  		top_earners.append((key,val))
			  print(top_earners)
			  # [('Alice', 100000), ('Carol', 12290
			  ```
	- While the code is correct, there’s an easier and much more concise—
	  and therefore more readable—way of accomplishing the same result. All
	  things being equal, the solution with fewer lines allows the reader to grasp
	  the meaning of code faster.
	- Python offers a powerful way of creating new lists: list comprehension.
	  The simple formula is as follows:
	- `[ expression + context ]`
	- The enclosing brackets indicate that the result is a new list. The context
	  defines which list elements to select. The expression defines how to modify
	  each list element before adding the result to the