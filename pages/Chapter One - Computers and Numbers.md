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
			- Octal or Base-8
				- Base-8 numbers are _octal_ numbers. Their use has diminished in recent decades. We saw an example earlier with the number 1234.5678. In Python, use the 0o prefix, the oct function, or the %o format specifier:
					- ```
					  >>> 0o1234
					  
					  668
					  
					  >>> oct(77)
					  
					  '0o115'
					  
					  >>> "%o" % 77
					  
					  '115'
					  ```
			- Hexadecimal or Base-16
				- Finally, base-16 numbers are _hexadecimal_ numbers, or simply _hex_. Hex is far more common than octal. Both Python and C understand hex numbers via the 0x prefix and %x format specifier:
					- ```
					  >>> 0xFDED
					  
					  65005
					  
					  >>> 0xdeadbeef
					  
					  3735928559
					  
					  >>> "%x" % 8675309
					  
					  '845fed'
					  
					  >>> hex(8675309)
					  
					  '0x845fed'
					  ```
				- Python’s int function takes a second argument, a base in [2, 36] for symbols 0 through 9 and _A_ through _Z_ regardless of case. This means we can interpret the string 11011 in many ways:
					- ```
					  >>> x = '11011'
					  
					  >>> int(x), int(x, 2), int(x, 8), int(x, 16)
					  
					  (11011, 27, 4617, 69649)
					  ```
-