# Overview

- An application, such as a PWA storefront, uses state data to render dynamic content to the user.
- Interactions, such as clicking on a button or loading the page, modify the state and update the appearance or behavior of the application.

## Local versus global state

- Local and global are the two different types of state a component can depend on 

- Local state data is any data scoped within a component or its children components.
- This type of data is not shared with a component's parent or peer data.
- Otherwise, that data should be [[lifted]]

- Example

- Global state data is any data made availlable to components in the entire application.
- Components that depend on a global state value subscribe to changes for that value and re-render themselves.
- Most components do not depend on the entire global state
- Instead a component only uses small pieces of the entire global state

- Shopping cart data is an example of global state data that components in different levels of the application use and modify

## Common state managent technologies

- There are many libraries and framework features that implement state management 
- This section describes two such technologies used in the PWA Studio project.

### Redux

- [[Redux]] is a state management design pattern and library.
- It promotes the idea of a global object tree that contains the state of the whole application.
- This object is known as a [[store]].

- The store is a read-only object which can only be updated by dispatching a [[reducer]] function.
- Reducer functions accept the current state and an [[action]] object as parameter and returns the next state

- Application components are able to [[dispatch]] various actions to update the state. 
- Components can also [[subscribe]] to state changes to update their apperance or behavior.

- Early versions of PWA Studio used the Redux library directly as the primary mechanism for managing application state, and the Redux pattern can be seen in hooks such as [[useRestResponse()]]

- Currenly, PWA Studio abstracts away its Redux implementation details using Peregrine hooks and context providers.
- This opens up the possibility of the project replacing Redux in Peregrine with another state management library without breaking state dependent components, such as those in Venia.
- PWA Studio allows you to customize reducers and enhancers.
- The following example uses `combineReducers()` to combine the default Peregrine reducers with custom reducers specific to the project and uses the combined reducers when creating the Redux store.

### React hooks

- Since PWA Studio favors using function components over classes, it uses many of React's [[built-in hooks]] in its Venia and Peregrine libraries.
- The Peregrine library also provides [[custom React hooks]] for storefront developers.
- These hooks contain common storefront logic such as state management.

### State management in PWA Studio

- State management in PWA Studio is a mix of the Redux library, React hooks, and React context providers.
- The Redux library is the underlying technology that powers state management behind the scenes, but components do not interact with the global store directly.
- Instead, components that need global state data use React hooks and context providers to read or update the current state.

### Context providers

- React components look and behave as a result of their props.
- Normally this means an application needs to explicitly pass state data as a prop down the React application tree to components that need that data .
- This is known as prop drilling

- To avoid prop drilling, React provides the [[Context]] feature.
- The Context feature allows an application to define a value and make it available to its descendants without passing it down the tree

- A Context object contains a Provider and Consumer property.
- A `Context.Provider` component defines the shared data for its children, and a corresponding `Context.Consumer` acquires the data and subscribes to any changes

- PWA Studio uses the Context feature to provide application state data to storefront components through the [[PeregrineContextProvider]] component.
- Wrapping an application with the `PeregrineContextProvider` lets its components access different slices of the entire application state.

### Global state slices

- Peregrine exposes global state data in slices through the `PeregrineContextProvider` component and custom React hooks.
- A state data slice is a subset of values from the global state.
- Each slice contains data about a specific part of the application, such as the shopping cart state or user session state.

- To access a global state slice, wrap the `PeregrineContextProvider` around the main application 

- Next, import the appropriate [[context hook]] and decompose the array returned by the hook function call.
- The decomposed array yields the state data and an API object to update that state.

- Example