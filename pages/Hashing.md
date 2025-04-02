tags:: data-structures, algorithms, searching-operations, efficiency, hashing

- category:: [[Data Structures and Algorithms]]
- title:: [[Hashing]]
- algorithm-type:: [[Searching Operations and Efficiency]]
- # Hashing
	- Through hashing, we turn complex data into something simpler and more manageable, known as a hash value or hash code. This code acts like a unique tag for the original data, making it way easier and faster to store and find information. This all happens thanks to a clever hash function, a type of mathematical wizardry, which churns out these hash codes quickly and consistently.
	- The real magic of hashing is how it gives us almost instant access to data, no matter how big or complicated the dataset is. It slashes the time needed for search operations, making it a must-have in loads of areas, like databases, quick-access caches, and even in security through cryptography. Hashing lets us zip through huge data sets with ease, opening doors to new solutions and making tricky problems a breeze.
	- Hashing is a widely used technique in computer science that allows for efficient storage and retrieval of data. It works by converting a range of key values into a range of index values using a special function called a hash function. This hash function takes a key as input and produces a transformed value, known as the hash code. This hash code is then used as an index to store the original data associated with the key.
	- The main goal of hashing is to minimize the search time, regardless of the size of the data. By using a hash code as an index, the data can be stored in a way that allows for quick and easy retrieval. This is especially important when working with large datasets, as it helps to ensure that the search process remains efficient.
	- In summary, hashing is a powerful technique that enables efficient storage and retrieval of data by converting key values into index values using a hash function. By minimizing the search time, it allows for quick access to data regardless of its size.
	- ## Simplified Example
		- Imagine you have a large bookshelf, and you wish to quickly find books based on their titles. Instead of searching each book one by one (linear search style), you decide to organize them alphabetically and create an index that says which shelf contains books starting with a specific letter. Now, if you want a book with a title starting with 'M', you'd directly go to the 'M' shelf. That's a rudimentary form of hashing!
		- # `simple_hash.py`
			- ```python
			  def simple_hash(key, array_size):
			      # Return index derived from hash of the key
			      return sum(ord(char) for char in key) % array_size 
			  
			  # Create an empty shelf w/ 26 slots for each alphabet letter
			  bookshelf = [None] * 26
			  
			  def add_book(title, bookshelf):
			      index = simple_hash(title, len(bookshelf))
			      if bookshelf[index] is None:
			          bookshelf[index] = [title]
			      else:
			          bookshelf[index].append(title)
			          
			  def find_book(title, bookshelf):
			      index = simple_hash(title, len(bookshelf))
			      if bookshelf[index]:
			          return title in bookshelf[index]
			      return False
			  
			  add_book("Moby Dick", bookshelf)
			  print(find_book("Moby Dick", bookshelf))
			  ```