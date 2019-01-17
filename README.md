# React Best Practices

This repo is for the purposes of a high-level view of React architecture and best practices. As this repo grows, more ideas will be introduced and comparisons will be made with pros and cons.

Being that this is a public repo, I hope that people will not only take what they can from the repo's information, but contribute as well. Feel free to submit pull requests and I will add your information to the document.

Forking this repo is great if you want to create your own repo with your decisions, using this as a starting point.

## Architecture

### Routing

**React Router**  
https://github.com/ReactTraining/react-router

Guide:  
https://reacttraining.com/react-router/web/guides/quick-start

### Functional Paradigm

- Pure Components
  - fn(state, props) == UI
  - Local state suggestions from: https://medium.com/@robftw/characteristics-of-an-ideal-react-architecture-883b9b92be0b
    - No local state other than:
      - Animation/triggering of CSS Classes
      - UI control that doesn't involve business logic or dependent on other components
      - Extremely trivial UI related variables that won't cause side effects
      - Standalone libraries
- Flux pattern (uni-directional)
  - **Redux:** https://redux.js.org/

### State Structure

**From:** https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape

Normalized data structures by Id (Relational Table Model)

```javascript
{
  posts: {
    byId: {
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
    allIds: ["post1", "post2"],
  },
  comments: {
    byId: {
      "comment1": {
        id: "comment1",
        author: "user2",
        comment: "...",
      },
      "comment2": {
        id: "comment2",
        author: "user3",
        comment: "...",
      },
      "comment3": {
        id: "comment3",
        author: "user3",
        comment: "...",
      },
      "comment4": {
        id: "comment4",
        author: "user1",
        comment: "...",
      },
      "comment5": {
        id: "comment5",
        author: "user3",
        comment: "...",
      },
    },
    allIds: ["comment1", "comment2", "comment3", "comment4", "comment5"],
  },
  users: {
    byId: {
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
    allIds: ["user1", "user2", "user3"]
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
│   ├── /redux/  // See "Ducks"
│   ├── /scenes/
│   |   ├── /App/
│   |   |   ├── /_/
│   |   |   ├── /App.js
│   |   |   └── /index.js
|   |   ├── /MyScene/
|   |   |   ├── /__tests__/
|   |   |   ├── /_/
|   |   |   |   ├── /MySubcomponent/
|   |   |   |       ├── /__tests__/
|   |   |   |       ├── /MySubcomponent.js
│   |   |   |       └── /index.js
│   |   |   ├── /MyScene.js
│   |   |   └── /index.js
│   ├── /shared/
|   |   ├── /SharedAppSpecificComponent/
|   |       ├── /__tests__/
|   |       ├── /SharedAppSpecificComponent.js
|   |       └── /index.js
│   └── /types/
|
|
├── /lib
|   ├── /Button/
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
├── /redux/
    └── /MyDuck/
    |   ├── /__tests__/
    |   ├── actions.js
    |   ├── index.js
    |   ├── operations.js
    |   ├── reducers.js
    |   ├── selectors.js
    |   ├── types.js
    |   └── utils.js
    └── commonTypes.js
```

https://medium.freecodecamp.org/scaling-your-redux-app-with-ducks-6115955638be

## Best Practices and Coding Style

### Naming Convention

#### Components

**From:** https://hackernoon.com/react-components-naming-convention-%EF%B8%8F-b50303551505

[Domain]|[Page/Context]|ComponentName|[Type]

Examples:

ACommunityAddToShortListButton - [Domain][ComponentName][Type]  
SideBar - [ComponentName]  
SideBarSwitch - [ComponentName][Type]  
ChatConversation - [Page/Context][ComponentName]  
ChatConversationName - [Page/Context][ComponentName]  

### Component Creation

- Write a stateless functional component first
  - if (component requires state, life-cycle, or Redux) Make stateful class
  - if (component is comprised of multiple components or smaller pieces of functionality) Create container component
  - Move shared components out
    - if (app dependent) Keep in shared/MyComponent
    - if (!app dependent or primitive) Move to lib folder
- Avoid re-rendering component if possible

## Styling

### Precompilers

**SASS**  
https://sass-lang.com/

Gives you the ability to nest your CSS, create mixins, among other things.

Guide for SASS in React:  
https://hugogiraudel.com/2015/06/18/styling-react-components-in-sass/

**Styled Components**  
https://www.styled-components.com/

## Testing

### Libraries

**Jest**  
https://jestjs.io/

Most popular React testing library. Comes with snapshot comparisons which is very useful for testing pure components.

**Sinon**  
https://sinonjs.org/

Spies, stubs and mocks

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

## Other Libraries

### State Management Helpers

**Reselect**  
https://github.com/reduxjs/reselect

Creates a selector where the first functions passed in compute props for a final function. If none of those props have changed, then that function is not run and the result from the previous invocation is returned.

This keeps the state from needlessly causing components to re-render.

**Normalizr**  
https://github.com/paularmstrong/normalizr

Especially useful for taking in schemas of data input and producing an "entities" object and a "result" object. "Entities" is of the structure above in "State Structure" where it is a normalized relational list. "Result" is the list of ids.

### Form Helpers

**Redux Forms**  
https://redux-form.com/8.1.0/

Uses Redux to maintain the state of your forms through custom components, abstracted actions and reducers.

**Formik**  
https://github.com/jaredpalmer/formik

Creates an ephemeral state held by its components that abstracts away all the boilerplate that goes into creating and maintaining forms.

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

### Functional Programming Helpers

**Ramda**  
https://github.com/ramda/ramda

### Miscellaneous Helpers

**Moment**  
https://github.com/moment/moment/

Date/Time related formatting

**date-fns**  
https://date-fns.org/

Date/Time formatting with a functional paradigm and localization.

## Resources and Articles

**Good Starting Points:**  
https://github.com/markerikson/react-redux-links/blob/master/react-architecture.md

### Topics A-Z

**Folder Structure:**  
https://github.com/kylpo/react-playbook/blob/master/component-architecture/3_Multiple-Components.md

**Presentational vs Container Components:**  
https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0

**Routing:**  
https://medium.com/@algfry12/react-router-best-practices-9c564388f4d3

**Symbolic Linking (i.e. get rid of import '../../../etc.')**  
https://medium.com/@sherryhsu/how-to-change-relative-paths-to-absolute-paths-for-imports-32ba6cce18a5

**Utilities:**  
https://blog.bitsrc.io/11-javascript-utility-libraries-you-should-know-in-2018-3646fb31ade