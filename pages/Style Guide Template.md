template:: Style Guide

- tags:: ui-design, design-system, style-guide, design, projects, geaux-flow
-
- # Style Guide
  template:: style-guide
  template-including-parent:: false
	- property:: style guide
	- # Heading 1
		- ## Heading 2
			- ### Heading 3
				- #### Heading 4
					- ##### Heading 5
	-
	- # Text Formatting
		- Lorem ipsum dolor sit amet, consectetur adipiscing elit. -> Pellentesque lacinia cursus ipsum, non blandit est sollicitudin non. Proin fringilla enim tortor, quis finibus quam dictum nec. Nunc eu tincidunt erat, non pretium nibh.
			- inline block reference + number list
			  logseq.order-list-type:: number
			- Block embed + number list
			  logseq.order-list-type:: number
		-
		- **Bold text**
		-
		- *Italic text* _with underscores_
		-
		- ***Italic and bold***
		-
		- ~~Strikethrough~~
		-
		- ==Highlight with equal sign== and ^^Highlight with carets^^
		-
		- [Link](https://logseq.com/)
		-
		- [Link to another page in graph]([[TODO]])
		-
		- > Blockquote
		-
		- `Inline Code`
		-
		- ```clojure
		  (println "This is code")
		  ```
		-
		- $$Formula E=mc^2$$
	-
	- # Studying
		-
		- This is a {{cloze cloze}}
		-
		- This is a flashcard #card
			- And this is the answer
	-
	-
	- # Tasks
		- TODO This is a todo item
		- DOING This is an item you're doing
		  :LOGBOOK:
		  CLOCK: [2023-04-28 Fri 19:17:07]
		  CLOCK: [2023-04-28 Fri 19:17:10]
		  :END:
		- DONE This item is done
		- CANCELLED This Item is cancelled
		- NOW This task is now
		  :LOGBOOK:
		  CLOCK: [2023-04-28 Fri 19:17:48]
		  CLOCK: [2023-04-28 Fri 19:17:51]
		  :END:
		- WAIT This task is waiting
		- LATER This task is for later
		- This task is scheduled
		  SCHEDULED: <2023-04-28 Fri 19:30>
	-
	- # Links and Embeds
		- [URL Link](https://logseq.com/)
		- [Link to another page in graph (hover to preview)]([[TODO]])
		- ((6470139d-f5f1-4fce-b8d9-a670ee336fa8))
		- {{embed ((6470139d-d728-4637-ac20-d336cf2c6ec1))}}
	-
	- # Media
		- ![document.pdf](file:///C:\Users\user\Documents\document.pdf)
		-
		- ![image](https://asset.logseq.com/static/img/logo.png)
		-
		- ![](http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4)
		-
		- ![](https://www.kozco.com/tech/piano2-CoolEdit.mp3)
	-
	- # Query
		- {{query }}
	-
	-
	- # Table
		- | Syntax      | Description |
		  | ----------- | ----------- |
		  | Header      | Title       |
		  | Paragraph   | Text        |
	-
	- # Org Mode Alerts
		- #+BEGIN_TIP
		  This is a tip!
		  #+END_TIP
		- #+BEGIN_NOTE
		  This is a note
		  #+END_NOTE
		- #+BEGIN_IMPORTANT
		  This is important!
		  #+END_IMPORTANT
		- #+BEGIN_CAUTION
		  This is caution...
		  #+END_CAUTION
		- #+BEGIN_PINNED
		  This is pinned
		  #+END_PINNED
		- #+BEGIN_WARNING
		  This is a warning
		  #+END_WARNING
		- #+BEGIN_EXAMPLE
		  This is an example
		  #+END_EXAMPLE
		- #+BEGIN_CENTER
		  This is text in the center
		  #+END_CENTER
	-
	- # Draw
		- [[draws/2023-05-04-18-51-27.excalidraw]]