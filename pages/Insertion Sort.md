tags:: algorithms, data-structures, sorting-algorithms, insertion-sort

- category:: [[Data Structures and Algorithms]]
- algorithm-type:: #sorting-algorithm
- title:: Insertion Sort
- Imagine you are playing a game of cards. As you pick each card from the deck, you carefully examine it and decide its correct position among the previously sorted cards in your hand. You take into account factors such as the card's value, suit, and any specific rules or strategies of the game.
- Once you have determined the card's correct position, you insert it into its rightful place, ensuring that the overall order of the cards in your hand is maintained. This meticulous process of placing each card in its proper position within the existing sorted list is similar to the concept of Insertion Sort in computer science. Just like in the card game, Insertion Sort builds the final sorted list one element at a time, carefully considering and placing each element in its correct position within the already sorted portion of the list.
- By repeating this careful process for all the elements in the initial unsorted list, the result is a fully sorted list. In summary, Insertion Sort closely resembles the careful and methodical approach of sorting cards in a game, where each card is thoughtfully inserted in its appropriate position to construct the final sorted list accurately and efficiently. This process ensures that the elements are meticulously organized and that the final sorted list is constructed with precision and thoroughness.
- # Selection Sort Use Cases #card
  collapsed:: true
	- Insertion Sort is a sorting algorithm that works in a similar way to how people sort a hand of playing cards. It follows a step-by-step process to ensure that the cards are arranged in the correct order. First, the algorithm maintains a "hand" that is initially empty. As each new card (or element from the list) is introduced, it is compared to the cards already in the hand. The algorithm finds the appropriate position for the new card by shifting the existing cards to the right until it finds the correct spot. This process is repeated for each card in the original list. By inserting each card into the hand in its proper order, the algorithm gradually builds a sorted hand of cards. Once all the cards have been inserted, the hand represents the sorted list.
- # Insertion Sort Advantages #card
  collapsed:: true
	- The beauty of Insertion Sort lies in its simplicity and efficiency. It is a straightforward algorithm that can be easily understood and implemented. Despite its simplicity, it is capable of efficiently sorting small to moderate-sized lists. Insertion Sort is a card sorting mechanism that mimics how people sort playing cards. It builds a sorted hand by inserting each card into its proper position. This algorithm is known for its simplicity and efficiency in sorting small to moderate-sized lists.
- # Insertion Sort Performance #card
	- Insertion Sort is a simple sorting algorithm that has an average and worst-case time complexity of $O(n^2)$. While this may seem inefficient, Insertion Sort excels in scenarios where the list is partially sorted. In fact, in the best case scenario, when the list is already sorted, Insertion Sort's time complexity becomes an impressive $O(n)$. This is because Insertion Sort only needs to process each element in the list once, without requiring any swaps. Thus, Insertion Sort's performance greatly depends on the initial state of the list, making it a valuable choice in certain situations.
- # Insertion Sort Code
	- ```
	  ```