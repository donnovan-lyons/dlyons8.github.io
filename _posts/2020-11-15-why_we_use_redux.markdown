---
layout: post
title:      "Why we use Redux"
date:       2020-11-15 19:24:51 -0400
permalink:  why_we_use_redux
---

Recently I was asked about my React/Redux portfolio project in a job interview, why I decided to use Redux if React was already so stateful? What was the justification? In that moment, I realized that the benefits of Redux in it's integration with React wasn't so clear to me, and I fumbled. Although in the end I managed to explain that I am no longer too clear on the matter because I'd shifted my focus from frontend to backend, and also that the primary reason behind my use of integrating Redux into my React application was to learn and understand how it works, I left the interview a little ashamed, because at one point or another, I knew the answer.

Nevertheless, moments like these are our greatest teachers. They remind us of the importance of going back and reviewing material in order to solidify that knowledge. Today, I'm going to explain to you, the reader, why it is that we should use Redux.

### Let's Talk About React

React is one of the most popular front-end libraries in the world. It allows developers to build components with state encapsulated inside of them, that are passed down data from parent component to child component.

It's relevance comes into play when we're talking about the Document Object Model, or DOM. The DOM basically treats the HTML document as a tree structure where each node of the tree represents a part of the document. JavaScript enables us to manipulate the DOM dynamically, i.e. without refreshing the page, allowing us to build highly complex interactive websites.

So if we have JavaScript, why do we need the React library? The answer is that the more complex and interactive the website, the more noticeable the performance, because when these dynamic changes occur, the browser is recalculating the CSS, how the DOM tree is laid out, and then it gives us the display. What React does is give us a smart way to make these updates that improve performance.

#### How React Improves Performance

When we write components that return JSX elements, these JSX elements represent DOM nodes which become HTML on the page when the DOM is rendered. During the initial rendering, React builds a <strong>current</strong> tree. When updates are made that would cause a re-render in React, another tree, a <strong>workInProgress</strong> tree is created representing what the DOM would look like, and then our <strong>current</strong> tree is updated all at once. So for example, an interaction with the DOM results in multiple parts of the interface being updated, e.g. clicking a button that increments a quantity in different components e.g. a shoppping list where an increase in quantity would result in increase in subtotal, tax, and total amount, along with an increase in quantity of the item. Once all of the updates are computed, the <strong>workInProgress</strong> tree is used to update the DOM and the <strong>current</strong> tree is updated to reflect the new updates together. This avoid excessive re-rendering, thereby increasing performance. On top of grouping the updates together and rendering them all at once, React also has a diffing algorithm which identifies what the current DOM looks like and what it will look via the <strong>current</strong> and <strong>workInProgress</strong> trees, identifying where changes need to be made in order to avoid any unnecessary rebuilding.

### Where Redux Comes In

Now the part of the React component that stores all of the property values that can change upon interaction with the DOM is called <strong>state</strong>. Each component has it's own object which stores these variables and upon interacting with that component, the state changes and thereby re-renders the said component and passes down the props down and up(through callbacks) throughout the component tree. In a smaller scale, it's easier to manage an application where there aren't so many parent and child components, but when that application gets bigger and one needs to obtain a higher visual of how our components are handling and sharing data with one another, it becomes a little more unclear. That's where Redux comes in.

The solution provided by Redux is essentially to establish a single source of truth where all of our necessary data is stored in a separate JavaScript object outside of our components. This enables us to grab any data that we need from any component that wants to access it! This allows for interaction between siblings component to be easier, whereas in React it's taboo, and we're in essence using this Redux object that stores our variable data as props to our components. Any changes made to our Redux state triggers React component cycles, and so there we have it:

Redux places all of our data in one place. The state. This allows for what I consider a seamless flow of information.