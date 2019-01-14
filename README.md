# React Best Practices #

A comprehensive refrence guide to kickstart your React architecting career!

## Architecture ##

### Routing ###

### Functional Paradigm ###

- Pure Components
  - fn(state, props) == UI
  - No local state other than:
    - Animation/triggering of CSS Classes
    - UI control that doesn't involve business logic or dependent on other components
    - Extremely trivial UI related variables that won't cause side effects
    - Standalone libraries
- Flux pattern (uni-directional)

### State Structure ###

**From:** https://redux.js.org/recipes/structuring-reducers/normalizing-state-shape

Normalized data structures by Id (Relational Table Model)

```
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

### Folder Structure ###

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
├── /app
│   ├── /redux/
│   ├── /scenes/
│   |   ├── /App/
│   |       ├── /_/
│   |       ├── /App.js
│   |       └── /index.js
│   ├── /shared/
│   └── /types/
|
|
├── /lib
|
├── /tools/
├── /node_modules/
└── package.json
```

#### Redux "Ducks" ####

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

## Best Practices and Coding Style ##

### Rules of Thumb ###

#### Naming Convention ####

**Components**
**From:** https://hackernoon.com/react-components-naming-convention-%EF%B8%8F-b50303551505

[Domain]|[Page/Context]|ComponentName|[Type]

#### Component Creation ####

- Write a stateless functional component first
  - if (component requires state, life-cycle, or Redux) Make stateful class
  - if (component is comprised of multiple components or smaller pieces of functionality) Create container component
  - Move shared components out
    - if (app dependent) Keep in shared/MyComponent
    - if (!app dependent i.e. primitive) Move to lib folder
- Avoid re-rendering component if possible

#### Redux Work ####

- Use "Duck's" pattern
  - Details here: https://medium.freecodecamp.org/scaling-your-redux-app-with-ducks-6115955638be

## Styling ##

## Testing ##

**From:** https://github.com/kylpo/react-playbook/blob/master/best-practices/react.md

Write component tests that accomplish the following goals (from [Getting Started with TDD in React](https://semaphoreci.com/community/tutorials/getting-started-with-tdd-in-react?utm_source=javascriptweekly&utm_medium=email))

- it renders
- it renders the correct thing
  - default props
  - varied props
- it renders the different states
- test events
- test edge cases
  - e.g. something that uses an array should be thrown an empty array

## Resources ##

**Good Starting Points:**
https://github.com/markerikson/react-redux-links/blob/master/react-architecture.md

**Presentational vs Container Components:**
https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0

**Folder Structure:**
https://github.com/kylpo/react-playbook/blob/master/component-architecture/3_Multiple-Components.md

**Routing:**
https://medium.com/@algfry12/react-router-best-practices-9c564388f4d3
