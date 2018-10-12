#REACT

Hello World
```bash
# https://reactjs.org/tutorial/tutorial.html#setup-for-the-tutorial
npm install -g create-react-app
create-react-app my-app
npm start  #http://localhost:3000/

```

## **React component class**, or **React component type**

* We use components to tell React what we want to see on the screen. 
```javascript
class ShoppingList extends React.Component {
  render() {
    return (
      <div className="shopping-list">
        <h1>Shopping List for {this.props.name}</h1>
        <ul>
          <li>Instagram</li>
          <li>WhatsApp</li>
          <li>Oculus</li>
        </ul>
      </div>
    );
  }
}

// Example usage: <ShoppingList name="Mark" />
```

* A component takes in parameters, called props (short for “properties”), 
* and returns a hierarchy of views to display via the render method.

* The *render* method returns a description of what you want to see on the screen. 
  React takes the description and displays the result. 

## JSX  
* is a short way to describe the view, instead of writing it like this:
	```JavaScript
		return React.createElement('div', {className: 'shopping-list'},
		  React.createElement('h1', /* ... h1 children ... */),
		  React.createElement('ul', /* ... ul children ... */)
		);
	```


* In particular, render returns a **React element**, which is a lightweight description 
  of what to render. Most React developers use a special syntax called **“JSX”** which makes these structures easier to write. 
  The `<div />` syntax is transformed at build time to `React.createElement('div')`. 
* JSX comes with the full power of JavaScript. You can put any JavaScript expressions within braces inside JSX.  

* The code above is equivalent to:





















#Service Workers
A service worker is a script that your browser runs in the background, 
separate from a web page, opening the door to features that don't need a web page or user interaction. 
Can be used for:<br>
* push notifications
* background sync

The reason this is such an exciting API is that it allows you to support offline experiences, giving developers complete control over the experience.

Before service worker, there was AppCache. There are a number of issues with the AppCache API that service workers were designed to avoid.
Properties:<br>
* Webworkers is a JavaScript Workerit can't access the DOM directly. 
* Instead, a service worker can communicate with the pages it controls by responding to messages sent via the postMessage interface, and those pages can manipulate the DOM if needed.
* Service worker is a programmable network proxy, allowing you to control how network requests from your page are handled.
* It's terminated when not in use, and restarted when it's next needed, so you cannot rely on global state within a service worker's onfetch and onmessage handlers. If there is information that you need to persist and reuse across restarts, service workers do have access to the IndexedDB API.
* Service workers make extensive use of promises, so if you're new to promises, then you should stop reading this and check out Promises, an introduction.