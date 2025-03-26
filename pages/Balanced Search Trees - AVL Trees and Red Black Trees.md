# Balanced Search Trees: AVL Trees and Red-Black Trees
	- Balanced search trees are essential for maintaining efficient search, insertion, and deletion operations in dynamic datasets. Unlike unbalanced binary search trees, which can degrade to linear time complexity in the worst case, balanced trees guarantee logarithmic time complexity for these operations. This lesson will delve into two fundamental types of balanced search trees: AVL trees and Red-Black trees, exploring their properties, balancing mechanisms, and performance characteristics.
	- ## AVL Trees
		- AVL trees, named after their inventors Adelson-Velsky and Landis, are self-balancing binary search trees where the heights of the two child subtrees of any node differ by at most one. This height balance is maintained after each insertion or deletion by performing rotations.
		- ### Balance Factor
			- The balance factor of a node in an AVL tree is the difference between the height of its left subtree and the height of its right subtree. In a valid AVL tree, the balance factor of every node must be -1, 0, or 1.
				- **Balance Factor = height(left subtree) - height(right subtree)**
		- ### Rotations
			- Rotations are the key operations used to maintain the balance of an AVL tree. There are four types of rotations:
				- 1.  **Right Rotation (Single Right Rotation):** Used when a node's balance factor is +2 and its left child's balance factor is +1 or 0.
				- 2.  **Left Rotation (Single Left Rotation):** Used when a node's balance factor is -2 and its right child's balance factor is -1 or 0.
				- 3.  **Left-Right Rotation (Double Left-Right Rotation):** Used when a node's balance factor is +2 and its left child's balance factor is -1. This involves a left rotation on the left child followed by a right rotation on the node.
				- 4.  **Right-Left Rotation (Double Right-Left Rotation):** Used when a node's balance factor is -2 and its right child's balance factor is +1. This involves a right rotation on the right child followed by a left rotation on the node.
		- ### Insertion
			- When inserting a new node into an AVL tree, we first perform a standard binary search tree insertion. Then, we trace the path from the inserted node back to the root, updating the balance factors of the nodes along the path. If we encounter a node with a balance factor of +2 or -2, we perform the appropriate rotation to rebalance the tree.
			- **Example:**
			- Let's consider inserting the following sequence of numbers into an initially empty AVL tree: 10, 20, 30, 40, 50, 25.
				- 1.  **Insert 10:** The tree consists of a single node, 10.
				- 2.  **Insert 20:** 20 becomes the right child of 10. The balance factor of 10 becomes -1.
				- 3.  **Insert 30:** 30 becomes the right child of 20. The balance factor of 10 becomes -2, and the balance factor of 20 becomes -1. A left rotation is performed on node 10. The tree now has 20 as the root, 10 as the left child, and 30 as the right child.
				- 4.  **Insert 40:** 40 becomes the right child of 30. The balance factor of 20 becomes -1, and the balance factor of 30 becomes -1.
				- 5.  **Insert 50:** 50 becomes the right child of 40. The balance factor of 20 becomes -2, the balance factor of 30 becomes -1, and the balance factor of 40 becomes -1. A left rotation is performed on node 20. Then another left rotation is performed on node 30. The tree now has 40 as the root, 20 as the left child, and 50 as the right child. 20 has 10 and 30 as children.
				- 6.  **Insert 25:** 25 becomes the right child of 20. The balance factor of 40 remains 0, the balance factor of 20 becomes 0, and the balance factor of 25 becomes 0.
		- ### Deletion
			- Deletion in an AVL tree is more complex than insertion. After deleting a node, we again trace the path from the deleted node's parent back to the root, updating balance factors. Rebalancing may require multiple rotations, as a single rotation might not restore the balance of the entire tree.
			- **Example:**
			- Consider an AVL tree with nodes 10, 20, 30, 40, 50, 25 (as constructed in the insertion example). Let's delete node 10.
				- 1.  **Delete 10:** Node 10 is deleted. The balance factor of 20 becomes 1.
				- 2.  No rotation is needed as the balance factors are within the allowed range (-1, 0, 1).
			- Now, let's consider deleting node 40 from the AVL tree with nodes 20, 25, 30, and 50.
				- 1.  **Delete 40:** Node 40 is deleted. The balance factor of 50 becomes 0. The balance factor of 20 becomes -1.
				- 2.  No rotation is needed as the balance factors are within the allowed range (-1, 0, 1).
		- ### Code Snippet (Conceptual - Python)
			- ```
			  class Node:
			      def __init__(self, key):
			          self.key = key
			          self.left = None
			          self.right = None
			          self.height = 1  # Height of the node
			  def height(node):
			      if node is None:
			          return 0
			      return node.height
			  def update_height(node):
			      node.height = 1 + max(height(node.left), height(node.right))
			  def balance_factor(node):
			      if node is None:
			          return 0
			      return height(node.left) - height(node.right)
			  def rotate_right(y):
			      x = y.left
			      T2 = x.right
			      # Perform rotation
			      x.right = y
			      y.left = T2
			      # Update heights
			      update_height(y)
			      update_height(x)
			      return x  # New root
			  def rotate_left(x):
			      y = x.right
			      T2 = y.left
			      # Perform rotation
			      y.left = x
			      x.right = T2
			      # Update heights
			      update_height(x)
			      update_height(y)
			      return y  # New root
			  def insert(root, key):
			      # 1. Perform the normal BST insertion
			      if root is None:
			          return Node(key)
			      if key < root.key:
			          root.left = insert(root.left, key)
			      else:
			          root.right = insert(root.right, key)
			      # 2. Update the height of the current node
			      update_height(root)
			      # 3. Get the balance factor
			      balance = balance_factor(root)
			      # 4. If the node is unbalanced, then try out the 4 cases
			      # Case 1 - Left Left
			      if balance > 1 and key < root.left.key:
			          return rotate_right(root)
			      # Case 2 - Right Right
			      if balance < -1 and key > root.right.key:
			          return rotate_left(root)
			      # Case 3 - Left Right
			      if balance > 1 and key > root.left.key:
			          root.left = rotate_left(root.left)
			          return rotate_right(root)
			      # Case 4 - Right Left
			      if balance < -1 and key < root.right.key:
			          root.right = rotate_right(root.right)
			          return rotate_left(root)
			      return root
			  ```
		- ### Practical Considerations
			- AVL trees provide guaranteed logarithmic time complexity for search, insertion, and deletion. However, the frequent rotations can add overhead, especially during insertion and deletion operations. This makes AVL trees suitable for applications where search operations are more frequent than insertions and deletions.
	- ## Red-Black Trees
		- Red-Black trees are another type of self-balancing binary search tree. They use a different approach to balancing compared to AVL trees, relying on color properties of nodes to ensure logarithmic height.
		- ### Properties of Red-Black Trees
			- A Red-Black tree satisfies the following properties:
				- 1.  Every node is either red or black.
				- 2.  The root is black.
				- 3.  Every leaf (NIL) is black.
				- 4.  If a node is red, then both its children are black.
				- 5.  For each node, all simple paths from the node to descendant leaves contain the same number of black nodes (black-height).
		- ### Rotations and Color Flips
			- Red-Black trees use rotations similar to AVL trees (left and right rotations) but also employ color flips to maintain balance. A color flip involves changing the color of a node and its children.
		- ### Insertion
			- When inserting a new node into a Red-Black tree, the new node is always colored red. This may violate the Red-Black tree properties, so we need to perform rotations and color flips to restore the properties.
			- **Example:**
			- Let's insert the following sequence of numbers into an initially empty Red-Black tree: 10, 20, 30, 40, 50, 25.
				- 1.  **Insert 10:** The tree consists of a single black node, 10.
				- 2.  **Insert 20:** 20 becomes the right child of 10 and is colored red.
				- 3.  **Insert 30:** 30 becomes the right child of 20 and is colored red. This violates the Red-Black tree properties. A left rotation is performed on node 10, and colors are flipped. 20 becomes the root (black), 10 becomes the left child (red), and 30 becomes the right child (red).
				- 4.  **Insert 40:** 40 becomes the right child of 30 and is colored red.
				- 5.  **Insert 50:** 50 becomes the right child of 40 and is colored red. This violates the Red-Black tree properties. A left rotation is performed on node 20. Then another left rotation is performed on node 30. The tree now has 40 as the root (black), 20 as the left child (red), and 50 as the right child (red). 20 has 10 and 30 as children.
				- 6.  **Insert 25:** 25 becomes the right child of 20 and is colored red.
		- ### Deletion
			- Deletion in a Red-Black tree is more complex than insertion. It involves finding a replacement node (usually the inorder successor or predecessor) and then performing rotations and color flips to maintain the Red-Black tree properties.
		- ### Code Snippet (Conceptual - Python)
			- ```
			  class Node:
			      def __init__(self, key, color="red"):
			          self.key = key
			          self.left = None
			          self.right = None
			          self.color = color  # "red" or "black"
			          self.parent = None
			  def rotate_left(root, x):
			      y = x.right
			      x.right = y.left
			      if y.left != None:
			          y.left.parent = x
			      y.parent = x.parent
			      if x.parent == None:
			          root = y
			      elif x == x.parent.left:
			          x.parent.left = y
			      else:
			          x.parent.right = y
			      y.left = x
			      x.parent = y
			      return root
			  def rotate_right(root, y):
			      x = y.left
			      y.left = x.right
			      if x.right != None:
			          x.right.parent = y
			      x.parent = y.parent
			      if y.parent == None:
			          root = x
			      elif y == y.parent.left:
			          y.parent.left = x
			      else:
			          y.parent.right = x
			      x.right = y
			      y.parent = x
			      return root
			  def insert(root, key):
			      node = Node(key)
			      y = None
			      x = root
			      while x != None:
			          y = x
			          if node.key < x.key:
			              x = x.left
			          else:
			              x = x.right
			      node.parent = y
			      if y == None:
			          root = node
			      elif node.key < y.key:
			          y.left = node
			      else:
			          y.right = node
			      if node.parent == None:
			          node.color = "black"
			          return root
			      if node.parent.parent == None:
			          return root
			      root = fix_insert(root, node)
			      return root
			  def fix_insert(root, k):
			      while k.parent != None and k.parent.color == "red":
			          if k.parent == k.parent.parent.left:
			              uncle = k.parent.parent.right
			              if uncle != None and uncle.color == "red":
			                  k.parent.color = "black"
			                  uncle.color = "black"
			                  k.parent.parent.color = "red"
			                  k = k.parent.parent
			              else:
			                  if k == k.parent.right:
			                      k = k.parent
			                      root = rotate_left(root, k)
			                  k.parent.color = "black"
			                  k.parent.parent.color = "red"
			                  root = rotate_right(root, k.parent.parent)
			          else:
			              uncle = k.parent.parent.left
			              if uncle != None and uncle.color == "red":
			                  k.parent.color = "black"
			                  uncle.color = "black"
			                  k.parent.parent.color = "red"
			                  k = k.parent.parent
			              else:
			                  if k == k.parent.left:
			                      k = k.parent
			                      root = rotate_right(root, k)
			                  k.parent.color = "black"
			                  k.parent.parent.color = "red"
			                  root = rotate_left(root, k.parent.parent)
			          if k == root:
			              break
			      root.color = "black"
			      return root
			  ```
		- ### Practical Considerations
			- Red-Black trees generally require fewer rotations than AVL trees, especially during insertion and deletion. This can lead to better performance in applications with frequent modifications. Red-Black trees are widely used in standard libraries and operating systems due to their good average-case performance and relatively simple implementation compared to other balanced tree structures.
	- ## Exercises
		- 1.  **AVL Tree Insertion:** Insert the following keys into an initially empty AVL tree: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10. Draw the tree after each insertion, showing the balance factors and any rotations performed.
		- 2.  **AVL Tree Deletion:** Starting with the AVL tree you created in Exercise 1, delete the following keys in order: 5, 3, 7. Draw the tree after each deletion, showing the balance factors and any rotations performed.
		- 3.  **Red-Black Tree Insertion:** Insert the following keys into an initially empty Red-Black tree: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10. Indicate the color of each node after each insertion and any rotations or color flips performed.
		- 4.  **Red-Black Tree Deletion:** Research and describe the steps involved in deleting a node from a Red-Black tree. Explain the different cases that need to be considered and how rotations and color flips are used to maintain the Red-Black tree properties.
		- 5.  **Comparative Analysis:** Create a table comparing AVL trees and Red-Black trees based on the following criteria: balancing mechanism, frequency of rotations, implementation complexity, and typical use cases.
	- ## Summary and Next Steps
		- This lesson covered the fundamental concepts of AVL trees and Red-Black trees, two important types of balanced search trees. We explored their properties, balancing mechanisms (rotations and color flips), and practical considerations. Understanding these data structures is crucial for designing efficient algorithms and data management systems.
		- In the next lesson, we will explore B-Trees and their applications in databases. B-Trees are particularly well-suited for storing large amounts of data on disk and are widely used in database indexing.