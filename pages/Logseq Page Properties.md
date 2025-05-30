tags:: logseq, notes-catalog, digital-garden, geaux-flow, docs

- notes-category:: Productivity
- notes-subcategories:: Digital Gardens, Knowledge Management
- note-level:: 3, Note
- note-topic:: Logseq
- note-subtopic:: Features
- page-type:: Notes Catalog, Note
- description:: Page properties are ways we can set the frontmatter in our Logseq notes which is very useful when we go to add them to a project later on down the road.
- alias:: Logseq Docs - Page Properties
- date-created::  [[Mar 9th, 2025]] *12:45*
- page-id:: GEAUX-NOTES-04.01-LOGSEQ-01.00.01.02
- navigation:: [[Digital Garden Home]] | [[Notes Catalog]] | [[Geaux Flow]] | [[Journals]] | [[Content]]
- nav-level-up:: [[Logseq Basic Features]]
- nav-next-note:: [[Future Logseq Note]]
- # Logseq Page Properties
	- How we can create relationships, frontmatter in pages, and much more
- # Table of Contents
	- ## Properties
		- ### type:
			- [Feature](https://docs.logseq.com/#/page/feature)
		- ### platforms:
			- [All Platforms](https://docs.logseq.com/#/page/all%20platforms)
		- ### description:
			- Annotates any block or page with multiple pairs of values e.g. `rating:: 8` or `name:: foo`. Building block for organizing graphs
	- ## Usage
		- ### Property naming rules:
			- To enter a property, type `::` to autocomplete a property name. After entering a property name, autocomplete a property value that is _specific_ to that property. To see this in action, [see this demo](https://www.loom.com/share/27804e1bcd7b4e4bbf71ec14956c42fe).
		- ### Markdown:
			- syntax: `property:: value`
		- ### org-mode:
			- syntax: `#+PROPERTY: value`
	- ## Functionality
		- _Page properties_ are defined by putting them into the first block of the page (_frontmatter_).
		- _Block properties_ are defined by putting them into any other block.
		- Any page or block can have multiple pairs of properties.
		- Properties with no value are not visible or queryable e.g. `property::`
		- ### Property values
			- Property values can be a mix of almost any text, links, page links and tags. This means you can write like anywhere else in Logseq:
				- ```
				      description:: [[Logseq]] is the fastest #triples #[[text editor]]
				    ```
			- The pages `Logseq,`, `triples` and `text editor` are all linked property values through the `description` property.
			- Built-in properties `alias` and `tags` also have an additional way of recognizing pages through comma separation:
				- ```
				      tags:: motor, steering wheel
				    ```
			- `motor` and `steering wheel` are automatically treated as page references. If there is no comma, the single value is also treated as a page ref. See the configuration section below to enable this behavior for specific properties.
			- To prevent a property value from having any links, wrap it within quotes (`"`):
				- ```
				      description:: "[[Logseq]] is the fastest #triples #[[text editor]]"
				    ```
	- ## Configuration in [[config.edn]]
		- `:property-pages/enabled?` - Boolean which determines if properties have their own pages. This is enabled by default
		- `:property-pages/exclude-list` - Specific properties to exclude from having property pages
		- `:property/separated-by-commas` - Properties that also identify page references with comma separated values like `:tags` e.g. `tags:: foo, bar`
		- `:ignored-page-references-keywords` - Properties that do not allow page referencing. Avoids the need to have to quote the property values with `""` every time
	- ## Background
		- Example data for this section
			- **Properties** have multiple use cases including:
				- Selecting (querying) specific pages/blocks:
					- For example, let's query all the blocks with the property `type` and the value `book`:
					- TODO Find other use cases for **Properties**
	- ## Additional Links
		- [[Logseq Built-in Properties]]
-