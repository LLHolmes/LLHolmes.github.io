---
layout: post
title:      "Initial Musings On React Hooks"
date:       2019-10-18 21:18:07 +0000
permalink:  initial_musings_on_react_hooks
---

I recently started exploring React Hooks in a React / Redux project.  Frankly, I’m torn on Hooks (just like everyone else it seems…).  They’re too new for any sort of concensus in the React community and I’m not sure I definitely haven't earned the right to tout my opinions like they’re law yet, but I wanted to talk about my first thoughts on using them and run through an example of how to build and use one.

Initial impression:  
* Hooks don’t work in React classes.  They are a way of using state in function components.  But you don’t need a class component to use Redux either, so for me that’s kind of a lateral move.  

* The benefit I see, is that a Hook subscribes to a state, so any change to that state is pushed into the component, versus being connected to the store.  You have to create the Hook, which adds some code, but *good gratious* do I hate how clunky it is to connect a component to the store...mappingStateToProps and importing `react-redux` and all...  With hooks you just import the function and go, which lets you clean up your components quite a bit — which I most *definitely* like.

* I also like how easy it seems to be to “hook into” window events.  I’m still improving on the pretty side of the front end, but it seems like it would make it would be way easier to create dynamic components based on window size, etc.

* Using Hooks *with* a Redux store, does seems like over kill though.  I’m not totally sold either way yet.  I see the benefits of both, but I need to get some more experience under my belt before I can give you more than initial musings.

Anyway, here’s my basic example for you to get a tiny taste of React Hooks and what makes them tick.  Let’s pretend we’re making a library app where our `store` contains an array of books, each with a title.  For simplicity, to avoid the logic of a whole form and Redux action, there's a button where you can add a book to the store that calls and action creator.  

You'll need two things for the Hook: the Hook itself and the component to impliment it.  Let’s look at the code and then explain the parts.


In hooks.js:
```
import * as React from 'react'
import { store } from '../store'

export const useBooks = () => {
    // Creating the Hook:
    const [books, setBooks] = React.useState(store.getState().books)

    // Using the Effect:
    React.useEffect(() => {
        const unsubscribe = store.subscribe(() => {
            const newBooks = store.getState().books
            if (newBooks === books) return
    // Making a change:
            setBooks(newBooks)
        })
    // Cleaning up:
        return () => unsubscribe()
    })

    return books;
}
```

**Creating the Hook:**  Hooks use the React local state (here the local state of the useBooks function).
Since I’m using Hooks in conjunction with my Redux store, `store.getState().books` is passed into `useState` as the *inital state*.

`useState` is a built-in Hook which returns two things: the current state value and a method that can be used to update the value of the state => [currentValue, updateMethod]

We rename them went creating the variables to `[books, setBooks]` for consistency and in case we want to declare multiple state values in one function. 

**Using the Effect:**  `useEffect` is a built in Hook that “serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in React classes, but unified into a single API,” per the [React Hooks Overview](https://reactjs.org/docs/hooks-overview.html).  It allows you to change the DOM, subscribe, or fetch.  `useEffect` basically tells React to “do this” after anything changes.  If a component is “subscribed" to the hook, the hook pushes the info to those components after it’s local state is changed.

**Making a Change:**  When I created the Hook, I used `store.getState().books` as the intial state.  As the initial state, the store is only ever used once to *create the Hook*.  When you effect the Hook, it won’t specifically change the store, so you still need to pass any major changes through an action to update the store.  But then if you change the store, the change in the store will then update the Hook because `useEffect` updates the local state of the Hook using `setBooks()`.

**Cleaning up:**  If you create a subscription though, you should also give it a way to “unsubscribe” as clean up for whenever the component is unmounted or rerendered.


In books.js:
```
import * as React from 'react’;
// Import the Hook:
import { useBooks } from './hooks';
// Trust me, there's an action to add a book to the store...
import { addBook } from "./bookActions";



const Book = () => {
    // Use the Hook:
    const books = useBooks()

    // Add to the store using the mystery Redux action:
    const handleSubmit = event => {
        addBook({title: "Best Title Ever"})
    };

    return (
        <div>
        {books.map((book, index) => <div key={index}>{book.title}</div>)}
        <button onClick={handleSubmit}>Add a Book</button>
        </div >
    );

};

export default Book;
```

**The Component:**  All you need to do in the component, is import the hook and give it a variable name to use it.  No `import { connect } from 'react-redux';`, no need to `connect` the component, `mapStateToProps`, or pass in any props into it.  Clean.  That part I really like.

Take a look for your self and let me know what you think.

More reading:
* [React docs Hooks Intro](https://reactjs.org/docs/hooks-intro.html)
* [Making sense of React Hooks from the man himself](https://medium.com/@dan_abramov/making-sense-of-react-hooks-fdbde8803889)
* [Using Redux With Hooks](https://thoughtbot.com/blog/using-redux-with-react-hooks)
