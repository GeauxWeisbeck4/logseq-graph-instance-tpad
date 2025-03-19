tags:: math, programming, no-starch-press, books

- title:: Math For Programming
- publisher:: No Starch Press
- author:: Ronald T. Kneusel
- chapter:: Chapter One
- chapter-name:: Computers and Numbers
- # Table of Contents
	- ## Numbers and Number Bases
		- As humans, most things are in base 10
		- ### *Binary, Octal, and Hexadecimal Numbers*
			- **Binary** or Base-2
				- We have [0, 1] for the number of digits we tally for reading the base
				- 1011(base2) = $2^3+2^1+2^0$
				- = 8 + 2 + 1
				- = 11
			- How Python works with Binarys
				- ```
				  >>> 0b10100101
				  
				  165
				  
				  >>> bin(11)
				  
				  '0b1011'
				  
				  >>> "{0:b}".format(8675309)
				  
				  '100001000101111111101101'
				  ```
-