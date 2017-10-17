# React Continued

## Testing Components with Enzyme and Jest

- Testing React components can come in many different forms, but the most common way is to use AirBnb's Enzyme.
- Enzyme essentially creates JSON representations of your components (snapshots), which are then checked against rendering that happens when the test is run.
- For this testing we will need a few items to be installed:
    - `enzyme`
    - `enzyme-adapter-react-16`
    - `react-test-renderer`
- Let's see an example of testing our main App component:

```javascript
import React from "react";
import { shallow, configure } from "enzyme";
import Adapter from "enzyme-adapter-react-16";

configure({ adapter: new Adapter() });

import App from "./App";

describe("App Component", () => {

    it("Matches snapshot", () => {
        expect(shallow(<App />)).toMatchSnapshot();
    });

});
```

## Wine List Lab Part 3

- In this lab you will write tests for your components in the wine list.
- Your job is to write the appropriate tests and run them via Jest.

## Introduction to Redux

- There are many patterns in React, but one that is gaining popularity among all others is the Redux pattern.
- Redux is a pattern and a library that allows state to be managed in one large store instead of state being managed at a component level.
- Let's install the required dependencies:
    - `redux`
    - `react-redux`
    - `redux-thunk`
- In order to work with Redux, the first thing we must do is create a store:

```javascript
import { createStore } from "redux";

const store = createStore(reducerHere);
```

- This is the most basic store configuration, but normally you would want your store to also configure middleware to make Redux aware of routing changes and to use features like thunks:

```javascript
import { createStore, compose, applyMiddleware } from "redux";
import createHistory from "history/createBrowserHistory";
import { routerMiddleware } from "react-router-redux";
import thunk from "redux-thunk";

const history = createHistory();
const reactRouterMiddleware = routerMiddleware(history);

return createStore(
    reducerHere,
    undefined,
    compose(applyMiddleware(thunk, reactRouterMiddleware))
);
```

## Redux Reducers

- Since one of the core benefits of Redux is that it keeps track of when and where state has changed, it is important that the state itself is immutable.
- The principle behind Redux is that when a state change is desired, state is first copied, and the alteration is performed on the duplicate state.
- This process happens in what is known as a "reducer", which is simply a function that is responsible for creating a new version of the state based on some changes.
- Let's take an example reducer:

```javascript
export default (state = initialState, action) => {
    switch (action.type) {
        case ("SHOW_GREETING"): {
            return Object.assign({}, state, {
                greeting: action.payload
            });
        }
    }
}
```

- The sole purpose of the reducer as you can see is to duplicate the state, alter it, and return a new state.
- Since Redux is often used with multiple reducers, Redux comes with a tool to combine all of your reducers into one:

```javascript
import { combineReducers } from "redux";

export default combineReducers({
    firstReducer,
    secondReducer
});
```

- This can then be passed to the `createStore` method that we saw before.

## Redux Actions

- In order to trigger or "dispatch" any of these state changes (as outlined by the reducers), we must incorporate actions.
- Actions usually perform some kind of operation, such as an AJAX call, and then call upon the reducer to change the state with a payload attached.
- Let's take an example based on the reducer above:

```javascript
export const showGreeting = () => {
    return (dispatch) => {
        dispatch({
            type: "SHOW_GREETING",
            payload: "Hello World!"
        });
    }
}
```

- Notice that in this `showGreeting` function we are returning another function with `dispatch` as a parameter.
- This is called a "thunk", and is a common way of writing Redux actions because it is compatible with both asynchronous and synchronous operations.
- Note: This requires the use of `redux-thunk` middleware that we configured when we initialized our store.
- We will now implement Redux into our user manager application.

## Testing Redux

#### Reducer Tests

- Reducer tests are simple to write because they consist of simply calling your reducer methods with an action type and checking for a new object created with the data supplied.
- Let's see an example with the reducer above:

```javascript
describe("GREETING REDUCER", () => {
    
    it("SHOW_GREETING", () => {
        const stateChange = greetingReducer(null, {
            type: "SHOW_GREETING",
            payload: "Hello World!"
        });
        
        expect(stateChange)
        .toEqual(expect.objectContaining({
            greeting: "Hello World!"
        }));
    });
    
});
```

#### Action Tests

- Action tests are a bit more difficult, since you are actually testing what is going on inside of the action creators instead of some final result.
- This is also further complicated by the fact that these action creators are usually performing asynchronous operations such as HTTP requests, which require mocking.
- Let's first take a simple example with the greeting application:

```javascript
import configureMockStore from "redux-mock-store";
import thunk from "redux-thunk";
import * as actions from "./greetingActions";

const mockStore = configureMockStore([thunk]);

describe("GREETING ACTIONS", () => {
    
    it("Creates SHOW_GREETING", () => {
        store
        .dispatch(actions.showGreeting());
        
        return expect(store.getActions())
        .toEqual([{ type: "SHOW_GREETING", payload: "Hello World!" }]);
    });
    
});
```

- Notice that `store.getActions()` gives us insight into what is occuring inside of the action creator.

#### Action Tests with HTTP Requests

- When testing action creators that perform HTTP requests, you generally do not want your tests to perform real network requests.
- In order to prevent this from happening, interceptors are used.
- The package we will be using to make this happen is called `axios-mock-adapter`:

```javascript
import MockAdapter from "axios-mock-adapter";
const mockHttp = new MockAdapter(axiosInstance);

describe("USER ACTIONS", () => {

    it("Creates USER_FETCH_SUCCESS", () => {
        mockHttp
        .onGet("/users")
        .reply(200, []);
        
        // Rest of your test here
    });

});
```

## Wine List Lab Part 4

- In this lab you will convert your wine list lab into a Redux pattern.
- Your job is to set up reducers and actions which will change the application's state to trigger re-rendering of the components.
- **Bonus:** Write reducer tests.
- **Super Bonus:** Write action tests.

## Extra Practice: Movie Search Lab

- For this lab we will be building an application that can search for movies in a database and display the results.
- The frontend is already done for you here: https://github.com/arun-projects/Movie-Search-App.
- Your job is to create a React application that searches the OMDB API for movies and returns results to the UI.