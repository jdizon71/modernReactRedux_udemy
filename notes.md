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
