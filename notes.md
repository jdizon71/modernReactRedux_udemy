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

### Summary

* Class-based components vs Functional Components
* State
  * Whenever a change is made to `state`, the component automatically re-renders along any children the component contains as well
  * As this was a *React-only* application, we have been dealing with *component-level state*
    * Both `App` and `SearchBar` both had their own, separate `state`
    * When we start working with Redux, we will be working with *application-level state*
* Using callbacks to manipulate data

## Modeling Application State
### Foreword on Redux

* **Redux** is not the only techology that we'll be using
  * There's also **Webpack**, **Redux Router**, **React Router**, **Redux Promise**, **Redux Thunk**, **Hot Reloadiing**, etc.

### What is Redux?
### More on Redux
### Even More on Redux!

* Redux is a predictable, *state* container for JavaScript applications
* Redux deals with the data of the application
  * It is a collection of all the data that describes the app
  * Includes both *hard data* and *meta data*
* React deals with the *views* of the application
  * Translates data into something that can be displayed on the screen as something that the user can interact with

* The difference with creating a React/Redux application is that we *centralize* all the application's data inside of a single object
  * With other JavaScript libraries, you have separate collections of data
  * Redux keeps all the application's data within a single object, known as the *state*
    * The *state*, in regards to Redux, is *application-level state*

* One of the most important things when creating a Redux application is how to design your *state*
* To reiterate, all the data that describes the application is contained within a single JavaScript object
  * It is maintained by Redux
  * Referred to as the application state

### Reducers

* A **reducer** is function that returns a piece of the application state
* Because our application can have many different pieces of state, we can also have many different reducers
  * Our bookList application will have two pieces of `state`: a `books ` and the currently `activeBook`
    * We will also have two different reducers, `booksReducer` and `activeBook reducer`, for each piece of `state`
    * Also note; our application state will be a plain JavaScript object
    * You can think of reducers as producing the value of the application's state

* When creating file structure for redux apps, it's helpful to *prepend* file names with the *type of file* it is
* To make use of reducers, we need to make sure to `export` the reducer so other files can have access to it

* A reducer is created using a two-step process:
  1. Create the reducer
  2. Wire the reducer into the application

### Containers - Connecting Redux to React

* To connect Redux with React , you use a different library called `react-redux`
  * To make use of the library, we're going to change one of our components to be a `container`
    * A `container` is a React component that has a *direct connection to the state managed by Redux*
      * Containers can also be referred to as *smart components*

### Containers Continued

* In general, we want the most parent component that cares about a particular piece of state to be a container
  * Looking at the structure of our app, we can make the case that `books` and `activeBook` should both be containers since they both would need data from Redux to populate

* In general, we only want to create containers out of components that care about a *particular* piece of state
* Also, only the *most-parent* component that uses a particular piece of state needs to be connected to redux

### Implementation of a Container Class

* To connect React and Redux, we need to use the `react-redux` library
* Whenever we connect a component and Redux, the component becomes a *container*, or a *smart component*

* `connect` is a function that takes a function and a component and produces a container
  * The container is aware of the `state` that is contained within Redux
* `mapStateToProps()` is important because it takes the `state` as an argument and returns an object
  * The object that is returned is available to our component as `this.props`
  * This function is the glue between React and Redux

* Whenever our application's state changes, the container will instantly re-render with a new list of books
* Whenever the application state changes, the object in the state function will be assigned as props to the component

### Containers and Reducers Review

  * Redux constructs the application `state`
  * React provides the *views* for rendering the application
  * Redux and React are two separate and are only connected by the `react-redux` library
  * The application `state` is generated by *reducer functions*
  * Our `booksReducer` is a function that contains an array of book objects
  * We nested the `booksReducer` inside of `combineReducers()` in the `index.js` file
  * `combineReducers` adds a *key*, `books:`, to the *global application state* whose value is the return-value of `booksReducer`
  * Next, we created a component, `BookList`, but upgraded it to a container because we need the container to be aware of the state within the redux part of the application
  * The way we promoted `BookList` from a component to a container by first `import`ing the `connect()` from `react-redux` library
  * Then, we defined the `mapStateToProps()` and connected that function to our container, `BookList`, using the `connect()`
  * We chose to connect `BookList` to connect to the Redux *store* because only `BookList` really cares about the list of books
  * Then, we rendered the `BookList` within `app.js`
  * Redux generates a `state` object that contained our `books` and then mapped the `state` as `props` to the container `BookList`
  * Because `state` is updated through `booksReducer`, the `BookList` container re-renders with that list of books

  * A container is a normal react component that gets bonded to the application `state`
  * Whenever the application `state` changes, the container re-renders as well

### Actions and Action Creators

* *Actions* and *action creators* give us the ability to change our state
* Almost everything in a redux application is triggered by a user; either directly or indirectly

* An *action creator* is a function that returns an *action*
* The *action* is automatically sent to all reducers
  * The *action object* contains a `type`; this describes the type of action the object is
  * The *action object* also contains some data that further describes the action
* The reducers can then choose to return a different piece of `state`, depending on what the action is
  * Within the reducers, it's normal to setup a `switch` statement
  * Depending on the `type` of action, a different piece of code will run
  * A reducer doesn't have to react to every different action
    * If it does not care about a particular action, it can just return its current `state`
  * If a reducer does care about a particular action, it will return a new value of `state`
* The newly returned piece of `state` gets piped into the application `state`
* The application `state` then gets piped backed into the react application; this causes all the components to re-render

### Binding Action Creators

* First, we're going to create an *action creator*, `selectBook()`
* Then, we're going to `import` the action creator, `selectBook()` into our `BookList` container
* Next, we're going to import the function, `bindActionCreators` from the `redux` library
  * This will be the connect between our action creator and our container
  * It will make sure our *action* that is generated by our action creator ends up flowing through all the different reducers
* We create a new function, `mapDispatchToProps()`, that will flow the return value of `selectBook`, which is an `action`, to all reducers by using `dispatch`
  * `dispatch` is a function that will take *actions* and send them through all the different reducers within the application
* The return value of `mapDispatchToProps` will end up being the value of `props` on the `BookList` container

### Creating an Action

* First, we're going to add a *click event-handler* to our *list items*
  * Every time a `li` gets clicked, it will call our *action creator*, `selectBook()`
* Actions usually have 2 properties
  1. `type`
    * describes the purpose of the action
  2. `payload`
    * updated piece of `state`

### Consuming Actions in Reducers

* Now, we're going to create our `activeBookReducer()`
* A *reducer* should receive 2 arguments:
  1. The current `state`
    * NOT referring to entire application state
    * Refers to only the piece of `state` the reducer is responsible for
  2. An `action`
* This is because reducers should only be called whenever an action occurs
* We want to prevent our reducer from being invoked every time an action is dispatched - *sometimes actions are dispatched that doesn't concern our specific reducer*
  * To do this, we're going to set a *base case* that will simply return the current `state`
    * Our base case will be to simply `return state`
* Most reducers are set up with JS `switch` statements
  * the `switch` statement will look at an action's `type` and run different logic depending on the `type` it receives
* We default our `state` to be `null` for the initial situation when the app is first loaded
* **NOTE**: It is important to make sure we never mutate our current `state` to produce a new version of `state`
* Finally, we'll add our `ActiveBook` reducer to our `combineReducers()`
* **NOTE:** Any *key* we add to the object in `combineReducers` gets added to the entire application state

* To wire up our `BookList` container with the *redux store*
* The pattern is the same:
  * `import` the `connect` function from the `react-redux` library
  * Then we connect our application state with the `props` of the `BookList` container
    * We do this by defining the function `mapStateToProps()` and connecting it to `BookDetail`
      * The return value of this function will get mapped to the `props` of `BookDetail`
        * `book: state.activeBook`
          * `state.activeBook` is being referenced from the `ActiveBook` reducer that is creating that piece of state
  * Lastly, we `export` the `BookDetail` container

### Reducers and Actions Review

* Redux is in charge of managing application state
* Application state is a single, JS object
* Application state is completely different and separate from component state
  * *THEY HAVE NO CONNECT WHATSOEVER*
* Application state is formed by *reducers*
* All *reducers* get tied together by the `combineReducers()`
* Each *key* within the object returned by `combineReducers()`, we assign one reducer which is responsible for creating that particular piece of state
  * That piece of state refers to the corresponding piece within the application state
* Reducers are in charge of changing the application state over time
* The way reducers do this is by using *actions*
* Whenever an action is *dispatched*, it flows through all the different reducers of the application
* Each reducer has the option to return a different piece of state based on the `type` of action that it received
* *Action creators* are simple functions that return an action
* An *action* is a plain JS object
* Actions MUST have a `type` defined, but they can OPTIONALLY have a `payload` or any properties
  * `payload` is only convention, but not requirement

* Our application flow:
  * We tied an *action creator* to each `book` list item
  * When a user clicks on a `book` list item, it called the action creator which `dispatch`es an *action*
  * That action automatically gets sent to all *reducers*
  * The reducer, `ActiveBook`, that cares about that particular action returns a new piece of state, `activeBook`
  * That new piece of state gets assembled into the application state
  * The application state then gets injected into all the different containers
  * The containers re-render the views with the updated piece of state
