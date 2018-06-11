# react-training

ReactJS/Redux/Mobx/React Native interesting notes

- [Thinking in Components](#thinking-in-components)
- [Virtual DOM](#virtual-dom)

Have fun :smile:

## Thinking in components

There are some guides to design React components/containers from mockup before development

### Atomic Web Design

![Atomic Design](/atomic-design.png?raw=true)

Atomic design is methodology for creating design systems.

*The idea is grabbed from chemistry.*

There are five distinct levels in atomic design:
* Atoms - labels, icons
* Molecules - styled labels
* Organisms - inputs, icon buttons, search inputs, 
* Templates - most of the React components (parameterized input)
* Pages - fullfilled template

Let's take **Airbnb** for example.

![Airbnb Example](/airbnb-structured.png?raw=true)

* Red and Blue - organisms
* Green - molecules

Nice articles:
* [Principles](http://bradfrost.com/blog/post/atomic-web-design)
* [Nice demo to understand principles](http://demo.patternlab.io/)
* [Boilerplate](https://arc.js.org/)
* [How should I separate components](https://reactarmory.com/answers/how-should-i-separate-components)

## Virtual DOM

Full rerendering when data is changed (rerender only comopnents that need to update).

*Virtual DOM is a tree of objects that is stored in memory that calls real DOM API*

### How Virtual DOM works

1. index.html is loaded.
2. bundle.js with react.js is loaded.
3. react.js generates `tree of objects` - Virtual DOM.
4. react.js `calls HTML DOM API` to render html by Virtual DOM.
5. react.js `subscribes to all HTML DOM events`.
6. When user clickes button the `event is fired`.
7. react.js handles this event - it `generates new Virtual DOM`.
8. react.js `compares old Virtual DOM with new Virtual DOM` (by special O(N) algorithm).
9. If there are some changes, it `replaces parts` of old Virtual DOM with new Virtual DOM.
10. Only after comparison react.js `calls HTML DOM API` to render new Virtual DOM.

### Reconciliation

1. 2 components with same class and id will generate the same tree object. e.g. React creates only one object for virtual dom for one class. And it uses the reference to it in other places.
2. 2 components of different classes will generate different trees.
3. Unique keys for list elements are important `<span key={4}>` (only single list item is updated, no need to update all list). If keys are not presented in code, React will rerender full list.

React uses `remove and add` if node is changed and `update` if attributes changed only.
See [Simple Virtual DOM example](/VirtualDOM.html)

### Lists

* It's easy for React to add element to the end.
* It's hard for React to add element to the beginning.
* There are some tricks with keys.

### React-Factory

`React-element` - React object for Virtual DOM.
`React-factory` - Function that allows to create React element.
`React-component` - Composite of React elements and components.

### React-Fiber

Change of comparison algorithm. Still in development (will be in `16.4` or `17.x`).

+:
1. Asyncrounous `render`.
2. Rendering prioritization. (Allow to render important things 1st, not so important later).
3. Use of previous result. Kind of virtual DOM cache.
4. Interrupt the execution.

### Performance Recommendations

Virtual DOM can be really slow! So:
1. Always add keys to list items.
2. Do not add unknown attributes to existing DOM elements (`<div myCustomAttribute />`) because of always force-update with custom atrtibutes.
3. Read docs and be up-to-date.

## Lifecycle

*It is possible to extend your React component by `MyNewBla extends MyAbstractBla`*

![React Lifecycle](/lifecycle.png?raw=true)

First render flow:
* `getDefaultProps()` - deprecated
* `getInitialState()` 
* `componentWillMount()` - deprecated, will be removed soon.
* `render()` - after this method
* `componentDidMount()` - good place for first `API call`
* `componentWillUnmount()`

Update flow (when `.setProps` or `.setState` or `.forceUpdate` is called):
* `componentWillReceiveProps(nextProps)` - 
* `shouldComponentUpdate(nextProps, nextState)` - can improve performance, e.g. there is no need to update component when some special prop is updated
* `componentWillUpdate(nextProps, nextState)`
* `render()` - 
* `componentDidUpdate(nextProps, nextState)`

*90% of React components are stateless*

![React Lifecycle Phases](/lifecycle-phases.jpg?raw=true)

*Force update might be useful for performance if you want to rerender smth quickly without `componentWillReceiveProps` and heavy `shouldComponentUpdate`* 

## Redux

* Middlewares
* Store
* Reducers
* 

### Store

* Stores information in state.
* Dispatch actions to appropriate reducer (get list of reducers, send current state and current action)

*Reducer case should always return something new*

* create
* subscribe
* unsubscripe
* dispatch

*If you use `Immutable`, try not to use `.toJS()`. Bad for performance*

## Project structures

Small app structure

```
- ducks(actions, selectors, reducers)
- utils
- services
- helpers
- views
```

Package by view structure

```
- registration-view
  - selectors
  - reducers
  - utils
  - services
  - helpers
- login-view
  - selectors
  - reducers
  - utils
  - services
  - helpers
```

Plugin structure

```
- registration-feature
  - actions
  - views
  - reducers
  - selectors
  - services
  - global/interface/api - calls separate feature actions
- login-feature
  - actions
  - views
  - reducers
  - selectors
  - services
  - global/interface/api - calls separate feature actions
```

Supervisor-plugin structure

```
- core/supervisor/(router + redux middleware) - manipulates features and integrations
- registration-feature
  - views
  - ducks
  - services
  - global/interface/api
- login-feature
  - views
  - ducks
  - services
  - global/interface/api
```

## FAQ
* How to initialize state? (best practices)