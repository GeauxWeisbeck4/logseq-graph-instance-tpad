tags:: digital-garden, notes-catalog, react, javascript, events, architecture
title:: Event Driven Architecture for React
date-created::   [[Mar 13th, 2025]] *17:06* 
page-type:: Note, Notes-Catalog
note-category:: Web Development
note-subcategories:: JavaScript, JavaScript Frameworks
note-topic:: React
note-level:: 3, Note 
page-id:: GEAUX-NOTES-01.01--01.01.01.01
description:: How we can create an event driven architecture for React, from a post on Dev.to
nav-level-up:: <- [[React Notes]] 
nav-next-note:: ->

- # Event Driven Architecture for React
	- How we can create an event driven architecture for React, from a post on Dev.to
- # Introduction
	- Are you tired of the endless tangle of props drilling and callback chains in your React applications? Does managing state and communication between deeply nested components feel like wrestling with spaghetti code?
	- An **event-driven architecture** can simplify your component interactions, reduce complexity, and make your app more maintainable. In this article, I’ll show you how to use a custom `useEvent` hook to decouple components and improve communication across your React app.
	- Let me walk you through it, let's start from
	- > Note: This mechanism is not intended as a replacement for a global state management system but an alternative approach to component communication flow.
	- ## The Problem: Prop Drilling and Callback Chains
		- In modern application development, managing state and communication between components can quickly become cumbersome. This is especially true in scenarios involving **props drilling**—where data must be passed down through multiple levels of nested components—and **callback chains**, which can lead to tangled logic and make code harder to maintain or debug.
		- These challenges often create tightly coupled components, reduce flexibility, and increase the cognitive load for developers trying to trace how data flows through the application. Without a better approach, this complexity can significantly slow down development and lead to a **brittle codebase**.
	- ### A Common Problem: Callback Complexity
		- Take this scenario, for example:
			- The Parent passes props to **Children A**.
			- From there, props are drilled down to **GrandChildren A/B** and eventually to **SubChildren N**.
			- If **SubChildren N** needs to notify the Parent of an event, it triggers a callback _that travels back up through each intermediate component_.
		- This setup becomes harder to manage as the application grows. Intermediate components often act as nothing more than middlemen, forwarding props and callbacks, which bloats the code and reduces maintainability.
		- ![PropsDrilling and CallbackChains](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fqv02a84oz8bnyecnuiyy.png)
		- To address **props drilling**, we often turn to solutions like global state management libraries (e.g., **Zustand**) to streamline data sharing. But what about managing callbacks?
		- This is where an **event-driven approach** can be a game-changer. By decoupling components and relying on events to handle interactions, we can significantly simplify callback management. Let’s explore how this approach works.
	- ## The Solution: Enter the Event-Driven Approach
		- ![Event lifecycle](https://media2.dev.to/dynamic/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fakspkpcn7xme6t9ymjsw.png)
		- Instead of relying on direct callbacks to communicate up the tree, **_an event-driven architecture decouples components and centralizes communication_**. Here’s how it works:
		- #### Event Dispatching
			- When **SubChildren N** triggers an event (e.g., onMyEvent), it doesn’t directly call a callback in the Parent.
			- Instead, _it dispatches an event that is handled by a centralized Events Handler_.
		- #### [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#centralized-handling)Centralized Handling
			- The **Events Handler** listens for the dispatched event and processes it.
			- It can notify the Parent (or any other interested component) or trigger additional actions as required.
		- #### [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#props-remain-downward)Props Remain Downward
			- Props are still passed down the hierarchy, ensuring that components receive the data they need to function.
			- > This can be solved with centralized state management tools like zustand, redux, but will not be covered in this article.
	- ## Implementation
		- But, _how do we implement this architecture?_
		- #### [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#useevent-hook)useEvent hook
			- Let's create a custom hook called **useEvent**, this hook will be responsible of handling event subscription and returning a dispatch function to trigger the target event.
			- As I am using typescript, I need to extend the window `Event` interface in order to create custom events:
			- ```
			  interface AppEvent<PayloadType = unknown> extends Event {
			    detail: PayloadType;
			  }
			  export const useEvent = <PayloadType = unknown>(
			    eventName: keyof CustomWindowEventMap,
			    callback?: Dispatch<PayloadType> | VoidFunction
			  ) => {
			    ...
			  };
			  ```
			- By doing so, we can define custom events map and pass custom parameters:
			- ```
			  interface AppEvent<PayloadType = unknown> extends Event {
			    detail: PayloadType;
			  }
			  export interface CustomWindowEventMap extends WindowEventMap {
			    /* Custom Event */
			    onMyEvent: AppEvent<string>; // an event with a string payload
			  }
			  export const useEvent = <PayloadType = unknown>(
			    eventName: keyof CustomWindowEventMap,
			    callback?: Dispatch<PayloadType> | VoidFunction
			  ) => {
			    ...
			  };
			  ```
			- Now that we defined needed interfaces, let's see the final hook code
			- ```
			  import { useCallback, useEffect, type Dispatch } from "react";
			  interface AppEvent<PayloadType = unknown> extends Event {
			    detail: PayloadType;
			  }
			  export interface CustomWindowEventMap extends WindowEventMap {
			    /* Custom Event */
			    onMyEvent: AppEvent<string>;
			  }
			  export const useEvent = <PayloadType = unknown>(
			    eventName: keyof CustomWindowEventMap,
			    callback?: Dispatch<PayloadType> | VoidFunction
			  ) => {
			    useEffect(() => {
			      if (!callback) {
			        return;
			      }
			      const listener = ((event: AppEvent<PayloadType>) => {
			        callback(event.detail); // Use `event.detail` for custom payloads
			      }) as EventListener;
			      window.addEventListener(eventName, listener);
			      return () => {
			        window.removeEventListener(eventName, listener);
			      };
			    }, [callback, eventName]);
			    const dispatch = useCallback(
			      (detail: PayloadType) => {
			        const event = new CustomEvent(eventName, { detail });
			        window.dispatchEvent(event);
			      },
			      [eventName]
			    );
			    // Return a function to dispatch the event
			    return { dispatch };
			  };
			  ```
			- The `useEvent` hook is a custom React hook for subscribing to and dispatching custom window events. It allows you to listen for custom events and trigger them with a specific payload.
			- What we are doing here is pretty simple, we are using the standard event management system and extending it in order to accommodate our custom events.
		- #### [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#parameters)Parameters:
			- `eventName` (string): The name of the event to listen for.
			- `callback` (optional): A function to call when the event is triggered, receiving the payload as an argument.
		- #### [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#features)Features:
			- **Event Listener**: It listens for the specified event and calls the provided `callback` with the event's `detail` (custom payload).
			- **Dispatching Events**: The hook provides a `dispatch` function to trigger the event with a custom payload.
		- #### [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#example)Example:
			- ```
			  const { dispatch } = useEvent("onMyEvent", (data) => console.log(data));
			  // To dispatch an event
			  dispatch("Hello, World!");
			  // when dispatched, the event will trigger the callback
			  ```
			- Ok cool but, what about a
	- ## [](https://dev.to/nicolalc/event-driven-architecture-for-clean-react-component-communication-fph?ref=dailydev#real-world-example)Real World Example?
		- Check out this StackBlitz (_if it does not load, please check it [here](https://stackblitz.com/edit/event-drive-arch?file=src%2FApp.tsx)_)