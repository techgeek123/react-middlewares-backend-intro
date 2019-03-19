# React and JSX

## Learning Competencies
By the end of this day you should be able to 
- Understanding the fundamentals of React 
- Understand the React syntax
- React Environment setup
- Compile JSX into plain Javascript

## Overview

### ReactJS introduction

React is a front-end library developed by Facebook. It is used for handling the view layer for web and mobile apps. ReactJS allows us to create reusable UI components.
According to React official documentation, following is the definition: 

> React is a library for building composable user interfaces. It encourages the creation of reusable UI components, which present data that changes over time. Lots of people use React as the V in MVC. React abstracts away the DOM from you, offering a simpler programming model and better performance. React can also render on the server using Node, and it can power native apps using React Native. React implements one-way reactive data flow, which reduces the boilerplate and is easier to reason about than traditional data binding.


### Story

React was created by Jordan Walke, a software engineer at Facebook. He was influenced by XHP, an HTML component framework for PHP. It was first deployed on Facebook's newsfeed in 2011 and later on Instagram.com in 2012. It was open-sourced at JSConf US in May 2013. React Native, which enables native iOS, Android and UWP development with React, was announced at Facebook's React.js Conf in February 2015 and open-sourced in March 2015. On April 18, 2017, Facebook announced React Fiber, a new core algorithm of React framework library for building user interfaces. React Fiber will become the foundation of any future improvements and feature development of the React framework.

### Why to use React JS 

Few of the advantages of react are given below:

- Uses virtual DOM which is Javascript object. This will improve apps performance since Javascript virtual DOM is faster than the regular DOM. 
- Can be used on client and server side as well as with other frameworks. 
- Component and Data patterns improve readability which helps to maintain larger apps.

Wondering what React can do, you can view them at [React examples](https://tutorialzine.com/2014/07/5-practical-examples-for-learning-facebooks-react-framework)

### Code Along

First of all, we will setup the React environment about which you **MUST READ** [here](https://scotch.io/tutorials/setup-a-react-environment-using-webpack-and-babel)

### JSX

React uses JSX for templating instead of regular JavaScript. It is not necessary to use it, but there are some pros that comes with it.

- JSX is faster because it performs optimization while compiling code to JavaScript.
- It is also type-safe and most of the errors can be caught during compilation.
- JSX makes it easier and faster to write templates if you are familiar with HTML.

### Using JSX

Look at the code from App.jsx where we are returning `div`.

App.jsx
```js
import React from 'react';

class App extends React.Component {
   render() {
      return (
         <div>
            Hello World!!!
         </div>
      );
   }
}

export default App;
```

#### Nested Elements

If you want to return more elements, you need to wrap it with one container element. Notice how we are using `<div>` as a wrapper for h1, h2 and p elements.

App.jsx
```js
import React from 'react';

class App extends React.Component {
   render() {
      return (
         <div>
            <h1>Header</h1>
            <h2>Content</h2>
            <p>This is the content!!!</p>
         </div>
      );
   }
}

export default App;
```

#### Attributes

You can use your own custom attributes in addition to regular HTML properties and attributes. When you want to add custom attribute, you need to use `data-` prefix. In example below we added `data-myattribute` as an attribute of `p` element.

```js
import React from 'react';

class App extends React.Component {
   render() {
      return (
         <div>
            <h1>Header</h1>
            <h2>Content</h2>
            <p data-myattribute = "somevalue">This is the content!!!</p>
         </div>
      );
   }
}

export default App;
```
### Expressions

JavaScript expressions can be used inside of JSX. You just need to wrap it with curly brackets {}. Example below will render `2`:
```js
import React from 'react';

class App extends React.Component {
   render() {
      return (
         <div>
            <h1>{1+1}</h1>
         </div>
      );
   }
}

export default App;
```

You can not use if else statements inside JSX but you can use conditional (ternary) expressions instead. In example below variable i equals to 1 so the browser will render true, if we change it to some other value it will render false.
```js
import React from 'react';

class App extends React.Component {
   render() {

      var i = 1;

      return (
         <div>
            <h1>{i == 1 ? 'True!' : 'False'}</h1>
         </div>
      );
   }
}

export default App;
```

### Styling
React recommends using inline styles. When you want to set inline styles, you need to use camelCase syntax. React will also automatically append px after the number value on specific elements. You can see below how to add myStyle inline to h1 element.
```js
import React from 'react';

class App extends React.Component {
   render() {

      var myStyle = {
         fontSize: 100,
         color: '#FF0000'
      }

      return (
         <div>
            <h1 style = {myStyle}>Header</h1>
         </div>
      );
   }
}

export default App;
```

### Comments
When writing comments you need to put curly brackets {} when you want to write comment within children section of a tag.
```js
import React from 'react';

class App extends React.Component {
   render() {
      return (
         <div>
            <h1>Header</h1>
            {//End of the line Comment...}
            {/*Multi line comment...*/}
         </div>
      );
   }
}

export default App;
```

## Exploration

- [React Tutorial](https://survivejs.com/react/getting-started/introduction-to-react/)
- [Thinking in React](https://medium.com/@nimelrian/thinking-in-react-a-paradox-statement-33c19e2eb9e2)
- [React video tutorial](https://egghead.io/courses/the-beginner-s-guide-to-reactjs)
- [JSX Tutorial](https://www.grotto-networking.com/code/WebDev/Lectures/JSXIntro.html)