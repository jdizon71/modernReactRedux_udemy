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

### Class-based Components

* So far, we've been creating *functional components*
  * They have been components created using functions

```
const SearchBar = () => { // functional component syntax
  return <input />;
};
```

* We can also create *class components*
  * Used whenever we want a component to have some internal record-keeping
  * Has the ability to be more self-aware and what's happened since it's been rendered
    * i.e. - Our `SearchBar` component needs to be aware when a user has typed something inside of it and what the user typed

* To create a class-based component, we use ES6 classes
  * ES6 classes are JavaScript objects that have properties and methods  

* Every class-based component we create must have a defined `render()` to return JSX
* `extends` allows our class-based components to *inherit* additional functionality from other classes

*Syntactic Sugar Below*

```
import React from 'react';
class SearchBar extends React.Component {}

// By using ES6 syntax, we can instead just write

import React, { Component } from 'react';
class SearchBar extends Component {}

```

### Handling User Events

* Handling *events* in React is a two-step process:
  1. Declare an *event handler*; the event handler should be a function that is called whenever the event occurs
    * `onInputChange()` is defined within `SearchBar` component and its purpose is to monitor user input
  2. We pass the event handler to the element that we want to monitor for events
    * `return <input onChange = { this.onInputChange } />;`
      * *NOTE:* Whenever we are writing JSX and we use JavaScript variables, we wrap it with curly braces

* All HTML `input` elements *emit a change event* whenever a user interacts with them by typing something in
  * `onChange` comes from the Browser; it's vanilla HTML

* All browser events that get triggered by native elements -i.e. `input`, `div`, `span`, `button`, etc. - get stored in the `event` object whenever we use an event handler
  * You can name it anything you want, it doesn't have to be `event`

*Syntactic Sugar Below*

```
  render() {
    return <input onChange = { this.onInputChange } />;
  }

  onInputChange(event) {
    console.log(event.target.value);
  }

// By using ES6 syntax, we can instead just write

render() {
  return <input onChange = { event => console.log(event.target.value) } // with single arguments, you don't need to use parens
}
```

### Introduction to State

* **State** is a plain JavaScript object that is used to record and react to user events
* Each *class-based component* we define has its own state object
  * Whenever a component's state is changed, the component immediately re-renders and also forces all of its children to re-render as well
  * *NOTE:* Functional components DO NOT have state; only class-based components can have state

* Before we can use state inside of a component, we need to **initialize the state object**
* All JavaScript classes have a special function called `constructor()`
  * The `constructor()` is the *first and only* function called automatically whenever a new instance of the class is created
  * It is reserved for doing some setup inside of the class; such as: initializing variables, initializing state, etc.  
  * `super(props)` - Because `SearchBar extends Component`, `Component` has its own `constructor()`
    * Whenever we want to define a method that is already defined within a parent class, we can call that parent method on the parent class by calling `super()`

```
this.state = { term: '' };
```

  * To *initialize state*:
    1. Create a new object `{}`
    2. The object will also contain properties that we want to record on the state
      * In our example, `term` refers to *search term*; i.e. the user searches for `funny cat videos`
    3. Assign the new object to `this.state`

### More on State

* `this.state` - only used within the `constructor()`
  * To **manipulate** state anywhere else, we'll use `this.setState()`
    * We pass this method an object that contains the new state we want to have/give to our component
    * By using `this.setState()`, we are able to maintain continuity
      * Behind the scenes, React does a lot of handling with the State object
      * If we change the value with `this.state.term`; React won't know that the state has changed
      * If you simply want to **reference** state, you can use `this.state.term`

### Controlled Components

* A **controlled field** is a *form element*, like an `input` whose value is set by the state; instead of the other way around
  * Right now, the user input *tells* the state what it should be. Whenever the input changes, it tells the state it needs to change also
  * It should actually be the other way around; the state should tell the input what the current value should be

*Controlled Component*

```
<input
  value = { this.state.term }
  onChange = { event => this.setState({ term: event.target.value }) } />
```

  * `input` is now a *controlled component* or a *controlled form element*
    * Its `value` is set by `state`; so its value only changes when the state changes

*How does is this actually working?*
  1. First, the app starts up and renders an instance of `SearchBar`
  2. Since a new instance of `SearchBar` is created, its `constructor()` is invoked
    * `this.state.term = ''`
  3. The component renders, via the `render()`
  4. `value` is, initially, an empty string; `value = { this.state.term }`
  5. The component waits for some user input to invoke the event handler
  6. When the user does enter some text, the `state` is updated; `this.setState({ term: event.target.value })`
    * At this point, the value of `input` HAS NOT CHANGED *this is the main key-point*
  7. Whenever `setState()` is called, the **component immediately re-renders***
  8. So, when the `render()` is invoked again, `value` gets updated to equal the current value of `this.state.term`

* In terms of React, this is the proper way to treat data

## Ajax Requests with React
### YouTube Search Response

* **Downwards Data Flow** - only the *most-parent* component should be responsible to fetch data; either from an API, Redux, etc.
  * Right now, `App` is the most-parent component that we have; so this is the component that should fetch the data from the YouTube API

```
YTSearch({ key: API_KEY, term: 'boosted board' }, (data) => console.log(data) );
```

### Refactoring Functional Components to Class Components

**Functional Component**

```
const App = () => {
  return (
    <div>
      <SearchBar />
    </div>
  );
};
```

**Class-based Component**

```
class App extends Component { // ES6 syntax replaces `function` keyword with fat arrow
  constructor(props) {
    super(props)

    this.state = { videos: [] }

    YTSearch({ key: API_KEY, term: 'boosted board' }, (videos) => { // `video` is arbitrary; you can whatever variable name you want
      this.setState({ videos });
      });
    }

    render() {
      return ( // For multiline JSX, it's best, but not required to use () to encapsulate the entire expression
      <div>
      <SearchBar />
      </div>
      );
    }
  };
```

* In ES6, if both key/value are the same variable name, you can simply write the variable name once

```
this.setState({ videos });
```

### Props

* Passing data between a parent-component and a child-component is referred to as *passing props* in React
  * The *data* that is passed between the parent-component and the child component is referred to as **props**

```
<VideoList videos={ this.state.videos } />
```

  * In this example, `videos` is the prop
  * Anytime `App` re-renders, like when we set `state` in the component, `VideoList` will get the new list of `videos` as well

* When using a *functional component*, the `props` object will arrive to the component as an *argument*

```
const VideoList = (props) => {
  const videos = props.videos; // example how to access the videos from the `props` object
```

**NOTE:** - In a *functional component*, the `props` object is an argument; in a *class-based component*, props are available anywhere via `this.props`

### Building Lists with Map

* Instead of using the basic JavaScript `for` loop, it's best to use *enumerables*; such as `map`, `filter`, `reduce`, etc.

### List Item Keys

* If we look closer into the response data that we get back for each video, we can see that every video has an `etag` property
  * We can use this as a *unique identifier* and pass this with every `videoListItem`

```
return <VideoListItem  key={ video.etag } video={ video } />
```

### Video List Items

```
const VideoListItem = (props) => {
const video = props.video;

// ES6 syntax

const VideoListItem = ({ video }) => {
```

### Video Selection

* To allow the user the ability to *click* on a video, we're going to use the concept of a *selcted video* to the `App` component's `state`
* The selected video will be a `video` object and it will always be passed into `VideoDetail`
  * Instead of `this.state.video`, we would be able to write `this.state.selectedVideo`
* To update `selectedVideo`, we'll *pass* a callback - `onVideoSelect()` - from `App` to `VideoList`, then from `VideoList` to `VideoListItem`
  * Whenever the `VideoListItem` is clicked, it will *run* the callback with the `video` that belongs to it

* Passing callbacks between parent/child components is great for small communication between the two components
  * It is rare to go more than 2 levels deep when passing callbacks

### Searching for Videos

* We're going to take a similar approach for hooking up the `searchBar` as we did with `selectedVideo`
* We will *pass* a callback, `videoSearch`, from the `App` component down to the `searchBar` component
  * The callback will take a `searchTerm`, which will be a string, and make a new `YTSearch` request
    * When we get a response, it will update the `state` with the new list of videos

### Throttling Search Term

* To fix the lag and the behavior of video list updating with every character input, we're going to use a library called `lodash`
  * Within `lodash`, we're going to use a *utility method* called `debounce()`
    * `debounce()` can be used to *throttle* how often a function is called

```
$ npm install --save lodash
```
