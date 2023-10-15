---
title: "Understanding The Concept of Web Reality"
description: "Web pages and applications are often constructed by composing various components together. Components can communicate with each other by passing data and triggering events"
tags: ["web components", "ssr"]
pubDate: "2022-07-08"
author: "David"
fullPost: "Read more"
---


<br></br>

## PART 1: COMPONENTS
Components are a **reuseable** and **mudular**(self contained) unit of code that encapsulate specific functionality, design or behaviour. They are more like the **building blocks of a web application's user interface and functionality**.

<br></br>

### Special features:
1. Modularity:
A component represents a specific piece of functionality or UI element, such as buttons, forms, headers, footers, or even more complex features like chat boxes or interactive charts. Components allow developers to break down a complex user interface or functionality into smaller, manageable parts.
2. Reusability:
Once created, components can be reused across different parts of a website or even in different projects. This reduces the need to rewrite code and speeds up development. Reusability is especially valuable for consistent branding and user experience throughout a website or application.
3. Encapsulation:
Components encapsulate their own logic, styles, and sometimes even data. This isolation prevents conflicts with other components and makes it easier to maintain and debug code. Changes made to one component are less likely to impact other parts of the application.
4. Composition:
Web pages and applications are often constructed by composing various components together. This compositional approach makes it easier to manage the complexity of the overall application by breaking it down into smaller, manageable pieces.
5. Hierarchical Structure:
Components can have a hierarchical structure, where components can contain other components, forming a tree-like arrangement. This structure is often referred to as the component tree, and it represents the layout and relationships of components within the application.
6. Communication:
Components can communicate with each other by passing data and triggering events. This interaction allows components to work together to provide a seamless user experience. Parent components can pass data down to their child components, and child components can emit events to notify parent components of certain actions.
7. State Management:
Many components maintain their own internal state, which represents data that can change over time. State allows components to respond to user interactions and update their display accordingly. Some frameworks provide tools for managing state globally across the entire application.
8. UI and UX Consistency:
Components enable consistent design and user experience throughout an application. Design systems can be implemented using components to ensure that the same styles, layouts, and interactions are used consistently across the application.
9. Testing and Debugging:
Since components are isolated units, they are easier to test and debug. Developers can focus on testing individual components' behavior and interactions, making it simpler to identify and fix issues.

10. Performance Optimization:
Frameworks often optimize rendering by updating only the components that have changed, rather than re-rendering the entire page. This improves application performance and responsiveness.

<br></br>

## Example:
HTML Components (HTCs) are a legacy technology[1] used to implement components in script as Dynamic HTML (DHTML) "behaviors"[2] in the Microsoft Internet Explorer web browser. Such files typically use an .htc extension and the "text/x-component" MIME type.[3]

An HTC is typically an HTML file (with JScript / VBScript) and a set of elements that define the component. This helps to organize behavior encapsulated in script modules that can be attached to parts of a Webpage DOM.

<br></br>

```html
<body>
<ul>
  <li style="behavior:url(hilite.htc)">Example</li>
</ul>
</body>
```
<br></br>

In this example, the li element is given the behavior defined by "hilite.htc" (a file that contains JScript code defining highlight/lowlight actions on mouse over). The same hilite.htc can then be given to any element in the HTML page - thus encapsulating the behavior defined by this file.

<style>
    h1 {
        color: red;
    }

    p {
        color: yellow; 
        max-width: 400px;
    }
</style>