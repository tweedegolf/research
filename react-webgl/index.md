# Introduction

With React you can greatly improve the performance of the DOM. React has a declarative component structure that acts as an abstraction layer of the DOM; you don’t create DOM elements directly, but instead you create light-weight React components that React renders to real DOM elements in a very optimized way.

React maintains a virtual DOM: changes to the DOM are initially carried through in the virtual DOM only. Then React calculates the minimal set of changes that have to be applied to the real DOM by diffing between the virtual DOM and the current real DOM.

React’s declarative component structure provides a very clean way for controlling elements of a GUI and as such it has been implemented in other libraries as well. See this impressive list.

Some libraries are just React components, for instance a countdown clock that can be used as a component in any React project. Other libraries create a React wrapper around functionality that doesn’t have much to do with DOM manipulation. For instance react-canvas renders components to the Canvas element, and react-three wraps 3D objects in a React component and thus allows you to manipulate the 3D scene as if it were a DOM.


# React-canvas

The library react-canvas was made by Flipboard to optimize the web version for mobile devices of their app. Their main concern was that scrolling a webpage on a mobile device is very slow, so they decided to render the list of magazine items to a canvas element.

The library defines a React component for images, for text and for a list of items. An item consists of an image and a text and a list of items can be infinitely long.

The canvas has the size of the screen and scrolling of the list is done by repainting the list at the subsequent different y-positions.

Repainting the canvas is very fast because the items are cached in an off-screen canvas after the first time they are rendered and by doing so they can be drawn back to the list very efficiently.

The diffing logic of React is used to determine which items must be drawn.


# Using React for a drawing tool

The react-canvas library is very much tailored to the Flipboard use case. It lacks all elementary methods that are needed for a drawing tool such as methods for rotating, scaling and drawing a line. React-canvas could be extended with such methods, but there may be better libraries available that use React’s component structure for rendering content to the Canvas element. As a starting point we look into the libraries in this list.


Links:

https://github.com/Flipboard/react-canvas

https://medium.com/front-end-development-2/my-reaction-to-react-canvas-e181a0189d4c

https://github.com/Izzimach/react-three

https://github.com/Izzimach/react-pixi




