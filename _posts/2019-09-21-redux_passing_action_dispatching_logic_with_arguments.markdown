---
layout: post
title:      "Redux Passing Action Dispatching Logic with Arguments"
date:       2019-09-22 00:30:56 +0000
permalink:  redux_passing_action_dispatching_logic_with_arguments
---


The [React Redux docs](https://react-redux.js.org/using-react-redux/connect-mapdispatch) tell us:

> Pass Down Action Dispatching Logic to ( Unconnected ) Child Components
> 
> In addition, you also gain the ability to pass down the action dispatching functions to child ( likely unconnected ) components. This allows more components to dispatch actions, while keeping them "unaware" of Redux.
> 
> ...And this is what you should totally make full use of in your implementation.

Great!  But what about passing down a callback function that requires arguments when it is called?

I have a React Redux project with a Rails backend where a user can enter search terms.  The search sends a request to a secondary API which returns results that are displayed on my front end.  If logged in, the user can choose to save the search to their account on my backend and it will be displayed again the next time they log in.  To accomplish this, I have:
* A SearchContainer (parent, connected to Redux using connect and passing the function down to the child)
* A SearchResultCard (child, receiving the passed in function)
* A saveSearch action (that makes the call to the backend)

All was in order and otherwise working.  However, when I tried to make that final POST request to my API using a Redux Thunk action creator function, it returns with a `501 (Not Implemented)` error.  I followed the chain of my data through the `onClick` call and I discovered that my arguments were coming into my action as undefined.  But why???

SearchContainer.js — THE WRONG VERSION...

```
const SearchContainer = ({ user, search, saveSearch }) => {

  return (
    <div>
      <SearchForm />
      { Object.keys(search).length > 0 ? <h3>Search Result</h3> : ''}
      { Object.keys(search).length === 0 ? '' : <SearchResultCard user={user} search={search} saveSearch={() => saveSearch()} />}
    </div>
  );
}

const mapStateToProps = state => {
  return {
    user: state.user,
    search: state.search
  }
}

export default connect(mapStateToProps, { saveSearch })(SearchContainer);
```


SearchResultCard.js — THE WRONG VERSION...

```
const SearchResultCard = ({ user, search, saveSearch }) => {
  let searchParams = {
    street: search.street,
    cityState: `${search.city}, ${search.state}`,
    zipcode: search.zipcode
  }

  return (
    <div>
      SEARCH INFORMATION TO BE DISPLAYED
      { user ? <button onClick={() => saveSearch(searchParams)} >Save</button> : '' }
    </div>
  );
}

export default SearchResultCard;
```

searchActions.js — ENDED UP STAYING THE SAME...

```
...
export const saveSearch = (searchParams) => {
  console.log(searchParams)  // => undefined …wtf???
  return dispatch => {
    return fetch(baseUrl + ‘/search', {
      ...
    }
  }
};
```

I had the proper params in my `onClick` function in the SearchResultCard.  I was passing the action from the SearchContainer to the Card correctly and was getting to the proper action.  Why were the params undefined when I got to that action?…Why why why???  It took a bit of trouble shooting to realize it was due to how the action was being passed as a prop into the card.  I needed to pass the params with it.  I don't need to have them directly attached to the onClick call in the SearchResultCard, but they need to be added as arguments when the action function is being passed into the card as a prop.

The final working result:

SearchContainer.js

```
const SearchContainer = ({ user, search, saveSearch }) => {
  let searchParams = {
    street: search.street,
    cityState: `${search.city}, ${search.state}`,
    zipcode: search.zipcode
  }

  return (
    <div>
      <SearchForm />
      { Object.keys(search).length > 0 ? <h3>Search Result</h3> : ''}
      { Object.keys(search).length === 0 ? '' : <SearchResultCard user={user} saveSearch={() => saveSearch(searchParams)} />}
    </div>
  );
}

const mapStateToProps = state => {
  return {
    user: state.user,
    search: state.search
  }
}

export default connect(mapStateToProps, { saveSearch })(SearchContainer);
```


SearchResultCard.js

```
const SearchResultCard = ({ user, saveSearch }) => {

  return (
    <div>
      SEARCH INFORMATION TO BE DISPLAYED
      { user ? <button onClick={() => saveSearch()} >Save</button> : '' }
    </div>
  );
}

export default SearchResultCard;
```



The container attaches the arguments to the action call and the click is simply calling that action creator function.
