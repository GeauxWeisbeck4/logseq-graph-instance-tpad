tags:: python, no-starch-press, programming-books

- problem-number::  dmopc15c7p2
- problem-set:: DMOJ
- # Problem One - Word Count Problem
	- ## Problem Statement
		- ### The Challenge
			- Count the number of words provided. For this problem, a word is any se-
			  quence of lowercase letters. For example, hello is a word, but so are non-
			  English “words” like bbaabbb.
		- ### Input
			- The input is one line of text, consisting of lowercase letters and spaces.
			  There is exactly one space between each pair of words, and there are no
			  spaces before the first word or after the last word.
			  The maximum length of the line is 80 characters.
		- ### Output
			- Output the number of words in the input line
	- ## Topics
		- ### Strings
			- Representing strings
			- String operators
			- ### String methods
				- ```
				  # Makes everything upper or lower case
				  >>> 'abc'.upper()
				  # ABC
				  >>> 'ABC'.lower()
				  # abc
				  
				  # Removes trailing or leading spaces
				  >>> '   abc'.strip()
				  # abc
				  >>> 'abc    '.strip()
				  # abc 
				  >>> '   abc   '.strip()
				  # abc
				  
				  # We can call it with a string as an argument to remove it
				  >>> 'abc'.strip('a')
				  # bc
				  >>> 'abca'.strip('a')
				  # bc
				  >>> 'abca'.strip('ac')
				  # b
				  ```