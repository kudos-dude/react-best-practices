# React Best Practices

**Update 11/6/20:** I have taken on a personal project with React and React Native. I will soon be updating this repo with new techniques and updated opinions from the internet on what best practices are out there.

Next update will include:  
**React Native**  
**RxJS**  
**SHARED Redux Model as independent repo (used in both React and React Native on the same project)**  
**...latest and greatest in React and React Native**  

Thank you for all the stars! I will do my best to make this repo live up to its title.

## Architecture

### Patterns

**Flux Pattern (uni-directional)**  
https://code-cartoons.com/a-cartoon-guide-to-flux-6157355ab207  

### Functional Paradigm

#### Functional Programming

- [Immutability](https://medium.com/dailyjs/the-state-of-immutability-169d2cd11310)
  - No side-effects
  - Predictable data
- [Functional methods](https://medium.com/dailyjs/the-state-of-immutability-169d2cd11310)
  - map/filter/reduce
    - [w3Schools array functions](https://www.w3schools.com/jsref/jsref_obj_array.asp)
    - [Array Push() Methode](https://www.interviewbit.com/snippet/37969eca625210ab4137)
- [Higher Order Components/Functions](https://medium.freecodecamp.org/higher-order-components-the-ultimate-guide-b453a68bb851)
  - Reusability and Composition

#### Pure Components

- fn(state, props) == UI
- Local state suggestions from: https://medium.com/@robftw/characteristics-of-an-ideal-react-architecture-883b9b92be0b
  - No local state other than:
    - Animation/triggering of CSS Classes
    - UI control that doesn't involve business logic or dependent on other components
    - Extremely trivial UI related variables that won't cause side effects
    - Standalone libraries

### Routing

**React Router**  
https://github.com/ReactTraining/react-router

Guide:  
https://reacttraining.com/react-router/web/guides/quick-start

### State Management

**Redux**  
https://redux.js.org/

**Redux DevTools**  
[Chrome Extension](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en)

This is a must have for Redux development.

Great article if you want to hone your skills:  
https://codeburst.io/redux-devtools-for-dummies-74566c597d7

#### Middleware

**Redux Thunk**  
https://github.com/reduxjs/redux-thunk

"Thunks are the recommended middleware for basic Redux side effects logic, including complex synchronous logic that needs access to the store, and simple async logic like AJAX requests."

**Redux-Sagas**  
https://github.com/redux-saga/redux-saga

Similar concept to thunk using Middleware to control side-effects in a React-Redux application, but with the added bonus of using ES6 Generators to control async flow and increase readability.

[More on Generators](https://redux-saga.js.org/docs/ExternalResources.html)

### State Structure

Normalized data structures by Id (Relational Table Model)

```javascript
{
  entities: {
    Post: {
      itemById: {
        "post1" : {
          id: "post1"
          author: "author1"
          body: "..."
          comments: ["comment1", "comment2"]
        },
        "post2" : {
          id: "post2"
          author: "author2"
          body: "..."
          comments: ["comment3", "comment4", "comment5"]
        },
      },
      items: ["post1", "post2"],
      meta: {},
    },
    User: {
      itemsById: {
        "user1": {
          username: "user1",
          name: "User 1",
        },
        "user2": {
          username: "user2",
          name: "User 2",
        },
        "user3": {
          username: "user3",
          name: "User 3",
        },
      },
      items: ["user1", "user2", "user3"],
      meta: {},
    },
    Comment: {
      itemsById: {
          "comment1": {
            userId: "user1",
            postId: "post1",
            body: { ... },
          },
          "comment2": {
            userId: "user2",
            postId: "post2",
            body: { ... },
          },
          "comment3": {
            userId: "user3",
            postId: "post3",
            body: { ... },
          },
          "comment4": {
            userId: "user4",
            postId: "post4",
            body: { ... },
          },
          "comment5": {
            userId: "user5",
            postId: "post5",
            body: { ... },
          },
        },
        items: ["comment1", "comment2", "comment3", "comment4", "comment5"],
        meta: {}
    },
  },
  
  
  simpleDomainData1: {...},
  simpleDomainData2: {...},
}
```

Removes the deeply nested structures as your state grows in size and keeps components from re-rendering accidentally.

### Folder Structure

```
.
├── /android/
├── /ios/
|
├── /assets
│   ├── /fonts/
│   ├── /images/
│   ├── /videos/
|
├── /app or /src
│   ├── /ducks/  // See "Ducks"
│   ├── /scenes/
│   |   ├── /App
│   |   |   ├── /_/
│   |   |   ├── /App.js
│   |   |   └── /index.js
|   |   ├── /MyScene/
|   |   |   ├── /__tests__/
|   |   |   ├── /_
|   |   |   |   ├── /MySubcomponent
|   |   |   |       ├── /__tests__/
|   |   |   |       ├── /MySubcomponent.js
│   |   |   |       └── /index.js
│   |   |   ├── /MyScene.js
│   |   |   └── /index.js
│   ├── /shared/
|   |   ├── /SharedAppSpecificComponent
|   |   |   ├── /__tests__/
|   |   |   ├── /SharedAppSpecificComponent.js
|   |   |   └── /index.js
│   |   └── /types.js
|   ├── /store/ // configureStore.js + middleware
|   └── /utils/ // helper functions, etc.
|
├── /lib
|   ├── /Button
|       ├── /__tests__/
|       ├── /Button.js
|       └── /index.js
|
├── /tools/  //Build scripts, other tools
├── /node_modules/
└── package.json
```

https://github.com/kylpo/react-playbook/blob/master/component-architecture/5_Example-App-Structure.md

**/lib/ explanation:**
Every component that is not "app specific" or a *primitive component* can be a candidate to be moved to the /lib/ folder. This allows for the possible side effect of hand-rolling your own component library as you code!

Primitive components are those that take in only primitives as their props and are functionally pure.

#### Redux "Ducks"

```
.
├── /ducks/
    ├── /MyDuck/
    |   ├── /__tests__/
    |   ├── actions.js
    |   ├── index.js
    |   ├── reducers.js
    |   ├── selectors.js
    |   ├── types.js
    |   ├── urls.js
    |   └── utils.js
    ├── /utils/ // Reducer specific utils
    ├── rootReducer.js
    └── types.js
```

https://medium.freecodecamp.org/scaling-your-redux-app-with-ducks-6115955638be

## Coding Style

### Component Creation

- Write a stateless functional component first
  - if (component requires state or life-cycle) Consider Hooks
  - if (component is comprised of multiple components or smaller pieces of functionality) Create container component
  - Move shared components out
    - if (app dependent) Keep in shared/MyComponent
    - if (!app dependent or primitive) Move to lib folder
- Avoid re-rendering component if possible

## Styling

**Semantic UI React**  
https://github.com/Semantic-Org/Semantic-UI-React

A JQuery-free version of Semantic UI that comes with React-based components. Semantic UI is great for themeing as well.

## Testing

### Libraries

**Jest**  
https://jestjs.io/

Most popular React testing library. Comes with snapshot comparisons which is very useful for testing pure components.

**Note:** Function spies are now included included in Jest, so I've removed `sinon` for now

**Enzyme**  
https://airbnb.io/enzyme/

A popular addition to Jest that provides additional functionality to traversing through component trees,
as well as helper functions for testing, triggering user events being an example.

### Strategies

**From:** https://github.com/kylpo/react-playbook/blob/master/best-practices/react.md

Write component tests that accomplish the following goals (from [Getting Started with TDD in React](https://semaphoreci.com/community/tutorials/getting-started-with-tdd-in-react?utm_source=javascriptweekly&utm_medium=email))

- It renders
- It renders the correct thing
  - Default props
  - Varied props
- It renders the different states
- Test events
- Test edge cases
  - e.g. something that uses an array should be thrown an empty array

## Build and Deploy

### Starter kits

https://reactjs.org/community/starter-kits.html

This list contains many popular choices, but your choice is yours. Getting a good starter kit depends entirely on what you're looking to get out of your project and team.

### Build Libraries

**Babel**  
https://babeljs.io/

Transpiles your javascript down from the latest standards to backwards compatible, browser friendly source. Used for transpiling JSX to JS.

**Webpack**  
https://webpack.js.org/

Bundles your files into static assets that are ready for production deploy. Many extensible plugins available.

**ESLint**  
https://eslint.org/

Keeps your code nice and tidy by yelling at you with squigglies. You can link this to your IDE.

Great ESLint configs:  
[prettier-eslint](https://github.com/prettier/prettier-eslint)  
[eslint-config-airbnb](https://www.npmjs.com/package/eslint-config-airbnb)  

## Source Control

### Branch Naming Strategies

**From:** http://www.guyroutledge.co.uk/blog/git-branch-naming-conventions/  

`type/component-name/title`

Common Types:

- feature
- fix
- hot-fix
- refactor
- update
- upgrade
- remove

Component Name example:  
`login-form`

Title example:  
`hoc-formik-input`

## Libraries

### Form Helpers

**Formik**  
https://github.com/jaredpalmer/formik

Creates an ephemeral state held by its components that abstracts away all the boilerplate that goes into creating and maintaining forms.

### State Management Helpers

**Reselect**  
https://github.com/reduxjs/reselect

Creates a selector where the first functions passed in compute props for a final function. If none of those props have changed, then that function is not run and the result from the previous invocation is returned.

This keeps the state from needlessly causing components to re-render.

### Styling

**classnames**  
https://github.com/JedWatson/classnames

Takes in conditionals that returns a space-delimited string for className.

```javascript
import classnames from 'classnames';

let classes = classnames('sd-date', {
  current: date.month() === this.props.month,
  future: date.month() > this.props.month,
  past: date.month() < this.props.month
});

return (
  <button className={classes} />

  vs

  <button className={`button-default-style ${isActive ? "active" : ""}`} />
);
```

## IDEs

**Visual Studio Code**  
https://code.visualstudio.com/

Microsoft has opensourced this light-weight and highly extensible IDE. My personal favorite.
