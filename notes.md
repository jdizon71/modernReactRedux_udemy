# Modern React with Redux
## An Intro to React

### Environment Setup

```
$ git clone https://github.com/StephenGrider/ReduxSimpleStarter.git

$ npm install // Installs dependencies for project

$ npm start // Starts boilerplate package and runs a local server
```

### Project Setup

*How is this working?*
  * First, we write some JavaScript inside of the package
  * **Babel** and **Webpack** bundle all those files together, compiles everything to ES5, and runs a local server
    * `localhost:8080`

### A Taste of JSX

```
<script src="/bundle.js"></script>
```

  * The `bundle.js` file is the *compiled JavaScript* for the entire application
  * If you look in the `src` directory of the project application, you'll find a bunch of JavaScript files
    * **Webpack** and **Babel** take all those files, *compile* them into a single JS files, and make that compiled code available as `bundle.js`

*What is React?*
  * **React** is a JavaScript library that is used to produce HTML that is shown to a user in a web browser
    * So, when we write *react code*, we're writing *individual components* or **views**
      * **Components/Views** are snippets of code that produce HTML
      * When we write *react code*, we write multiple, different components and we nest these components together by placing one inside the other in different fashions to make really, complex applications relatively simple.
      * A **component** is a collection of JavaScript *functions* that produce HTML

```
const App = function() {
  return <div>Hi!</div>;
}
```

  * `const` - ES6 syntax to declare a **constant** variable
  * The `<div>Hi!</div>` is referred to as **JSX**
    * **JSX** is a subset, or dialect, of JavaScript that allows us to write HTML inside of JavaScript code
      * Underneath the hood, the HTML code is really just JavaScript
      * Webpack and Babel *transpiles* all this code into *vanilla JavaScript* that we write before it shows up on the browser

### More on JSX

* **JSX**, like ES6, cannot be *interpretted* by the browser
* We use JSX because it produces the actual HTML that gets inserted into the Dom when we *render* the component
* JSX can also be nested inside of itself like normal HTML

### Differences Between Component Instances and Component Classes

* When we create a component, we are creating a **class** of a component; a *type* of a component
  * `App` is a *type of component*
    * We can ultimately have many different *instances* of the `App` component
    * You can think of it as a factory, or blueprint, to create many different instances of that specific component that gets rendered to the DOM
    * **SUMMARY:** Components must be **instantiated** before getting rendered to the DOM

* When we write a JSX tag, i.e. `<tagName> </tagName>` or `<tagName />`, it will actually call `React.createElement()` for us
  * So whenever we write JSX, and we put the component name, the component name is a *component class*, but using it inside JSX makes it a *component instance*

**Component Class**

  ```
  const App = function() {
    return <div>Hi!</div>;
  }
  ```

**Component Instance**

```
<App></App>

// or

<App />
```

### Render Targets

```
ReactDOM.render(<App />, document.querySelector(".container"));
```

  * The `ReactDOM.render(newElement, existingHTMLNode)` takes 2 arguments:
    1. The `newElement` you want to render to the DOM
    2. An `existingHTMLNode` you want to insert the newElement into


### Component Structure

* A React application is made of up different components
* Each component is a JavaScript function or object that returns some amount of HTML
* Ultimately, we will create different components for different functions/purposes of our application
* React knows how to render many different components on the screen at one time and it does it very fast

* Specifically for our application, a *YouTube clone*, we will create different components for:
  * Search Bar
  * Video Player/Title/Description
  * List of Video Previews
  * Video Previews with Title // *We will create many instances of these and insert them into our List of Video Previews*
  * One, big component that will contain all of our smaller components // *This will be our App - index.js - component*

* *By building small components, we can easily resuse code throughout the entire application*
* **NOTE:** We want to create make **one component per file**

### YouTube Search API Setup

```
$ npm install --save youtube-api-search
```

### Export Statements

* You need to put `import React from 'react';` inside every component that will render JSX

```
export default SearchBar;
```

  * If we want to make our components available for other components to `import`, we need to `export` them
  * Also, if we want to `import` files that we write, we must include the *relative path* in the `import` statement

```
import SearchBar from './components/searchBar';
```
