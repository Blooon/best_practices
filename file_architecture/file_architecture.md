# **File architecture**

[Project sub-repositories](#project-sub-repositories)

- [Assets](#assets)

- [Components](#components)

- [Contexts / HOCs / Hooks](#contexts--hocs--hooks)

- [API / GraphQL](#api--graphql)

- [Services](#services)

- [Tests](#tests)

- [Types](#types)

- [Utils](#utils)

- [Views or Pages](#views-or-pages)

- [Example of readable file architecture](#example-of-readable-file-architecture)

[Imports / Exports](#imports--exports)

## **Project sub-repositories**

- Your project repository should be cut down in clear sub-repository architecture
- Things other devs will be looking for should be easily findable without knowing the file tree, and things should be easily recognizable from the first look within the `src` folder
- Things of same nature should be gathered
- Only things of really high specificity should be “hidden” in the sub-repository where it’s used
  - Once that thing becomes shared across multiple components or files, it means this piece of code is not that specific anymore
  - It should be extracted and put in a file higher in the file tree to keep it access more global
  - This helps you to keep a cleaner and more modulable code base

There are some noticeable sub-repositories you should have in you React project :

### **Assets**

- Contains all your media assets used in your app
- However, highly requested assets on app first load like fonts are better place in you public folder to increase their availability

### **Components**

- Try to follow [Atomic Design](https://blog-ux.com/quest-ce-que-latomic-design/) principles
- One component per file
- Any reusable atom / molecule component should be moved in src/components
- Design your src/components repository as if it was a library
  - This improves reusability and readability of the architecture of your components
- Similar UI components should be gathered together
  - This improves readability of imports and gets your src/components repository clean and closer to what it could be if it was a library
    - Gather EditButton.tsx and DeleteButton.tsx in src/components/buttons and export theme in index.ts
    - Gather DateInput.tsx and TextInput.tsx in src/components/inputs
  - Repositories gathering components must be camel case not be confused with composed component repositories
- Export your components from src/components/index.ts
  - This lets you focus on what you want to produce in your views
  - You don’t care which sub-repo your component comes from
  - If your components are properly named, your shouldn’t have any namespace conflict and you shouldn’t need their sub-repo origin to get what they are / do

### **Contexts / HOCs / Hooks**

- Gather your Contexts, HOCs and Hooks in their own contexts, hocs and hooks folder as hey are React shareable utils across components and views
- Context.jsx files should export their custom Provider and useContext hook to facilitate context usage
- You can either define them in the same file or cut in 3 files under the same sub-repo

```
src
└── contexts
    └── SnackbarContext
        ├── SnackbarContext.tsx
        ├── SnackbarProvider.tsx
        ├── useSnackbar.tsx
        └── index.ts
```

// Context declaration and definition

// Custom provider that contains and **should encapsulate logic** to manage context state and provide usage methods and state

// Custom hook that either directly provide context state if it's simple enought, or lighten the context state or methods to ease usage

// Re-export everything in index.ts

### **API / GraphQL**

- Contains your backend API or GraphQL requests and their type declarations
- It requires its own repo since this is your main app service and will be the most work-on service
- Everything should match backend specs as much as possible to improve readability and collaboration

### **Services**

- Any third-party service that is not your main backend API should go here
- Ideally should also encapsulate as much as possible those services methods you use
  - This will help you to keep a homemade level of asbtraction within you app that belongs to your company, not to the package maintainer
  - This make your code less reliant to the package syntax
  - If you need to change method usage, you change it in one place
  - If you need to change the package, you change it in one place

### **Tests**

- No production code here !
- Tests can either be implemented in their own sub-repo
- Or they can be shipped with their execution file (`js`, `ts`, `jsx` or `tsx`)
- This second option may offer a better DX for components particularly

### **Types**

- Contains type definitions only
- Try to use import / export type statement as much as possible
- Ideally this entire repo is TS only and therefore completely unused at transpiled runtime of production code
- Type sub-repos should be structured by data entity rather than by feature
  - Data entities can be shared across multiple features
  - You can visually declare entity hierarchy and inheritance through the file tree
  - It helps you visualize the data scheme
  - It leaves features definition to views, where they belong, and helps you focus on UX-driven features in views implementation

### **Utils**

- It should be structures by data entity or data type (eg JS primitives) first
- Then by module or feature helpers on need
- Any piece of code reused at least twice accross the app should go here
- No JSX here, using JSX means you should make a Component, event for tiny task / rendering formatting
- Utils files can be use to encapsulate some little package helpers you’re using (ex : uuid generation package, etc…)

### **Views or Pages**

- Views components should be molecule or organism components that are context-reliant of the view
  - A component EditButton.tsx must not exist under src/views/Device
  - A component EditDeviceModal.tsx must not exist under src/components if it’s only used in the Device view
- You file architecture should reflect your render tree as much as possible
  - This allows to have a global idea of the render by only opening a repo without having to open any file
  - This also allows to identify quickly where the component you need to work on is placed in the hierarchy

```
├── ReseachView
│   ├── ResearchTable
│   │   ├── ResearchTableRow
│   │   │   ├── ResearchTableRowButton.tsx
│   │   │   ├── ResearchTableCells.tsx
│   │   │   ├── ResearchTableRow.tsx
│   │   │   ├── index.ts
│   │   ├── ResearchTableBody.tsx
│   │   ├── ResearchTableHead.tsx
│   │   ├── index.ts
│   ├── ResearchHeader.tsx
│   ├── index.ts
```

```tsx
<ResearchView>
  <ResearchHeader />
  <ResearchTable>
    <ResearchTableHead />
    <ResearchTableBody>
      <ResearchTableRow />
      <ResearchTableRow />
      <ResearchTableRow />
      // ...
    </ResearchTableBody>
  </ResearchTable>
</ResearchView>
```

- Composed components should be in their own repository with the same name
  - Default export of the repository in index.ts should be the aforesaid component
  - Repository should be Pascal case as it is your component’s definition location
  - Also export its sub-components if needed

```
├── ResearchTableRow
│   ├── ResearchTableRowButton.tsx
│   ├── ResearchTableCells.tsx
│   ├── ResearchTableRow.tsx
│   ├── index.ts
```

```tsx
// in index.ts
export {default as ResearchTableRow} from './ResearchTableRow.tsx'
```

```tsx
// in another component
import ResearchTableRow from '<path>/ResearchTableRow'
```

### **Example of readable file architecture**

```js
├── cicd                        // pipeline scripts
├── config                      // project configs (eslint, vite, vitest...)
├── doc                         // documentation
├── public                      // website public folder (public assets : fonts, etc..)
└── src                         // source code
    ├── assets                  // media assets (images...)
    ├── components              // business / view agnostic components
    ├── constants               // code constants
    ├── contexts                // React Contexts
    ├── graphql                 // GraphQL / API requests and types
    ├── hocs                      // React HOCs
    ├── hooks                   // React Hooks
    ├── layouts                 // reusable and sharable Layout components
    ├── locales                 // i18n locales
    ├── pages                   // static Pages components (404, error, loading...)
    ├── routes                  // app routes and routing
    ├── services                // third party services (Google Analytics, Google Maps, Jitsi, etc...)
    ├── styles                  // global app or services styles only (component and view styles should be shipped with them)
    ├── tests                   // test relative code (no production code here, tests can also be shipped with components)
    │   ├── specs               // test specs (contains the .spec.jsx files)
    │   │   ├── components
    │   │   ├── layouts
    │   │   ├── pages
    │   │   ├── views
    │   │   └── test_utils      // test helpers and utils functions (useful for testing only)
    │   └── helpers
    ├── theme                   // theme(s) config(s)
    ├── types                   // type definitions only
    ├── utils                   // app utils functions
    └── views                   // app views
```

## **Imports / Exports**

- Export components or methods in the same repo in index.ts :
  - This lighten import lines in other components or views that use them
  - This keeps your import and import type (for component’s props) in the same place
  - This allows you to emulate visibility for sub-components or sub-methods (like protected / private in Java)
- This also works for any module you want to create (utils, services, etc…)

```
└── components
    └── Button
        ├── Button.tsx
        ├── Button.stories.tsx
        ├── Button.styled.tsx
        ├── Button.module.scss
        ├── Button.spec.tsx
        └── index.ts
```

```tsx
// in components/Button/index.ts
export {default as default, type ButtonProps} from './Button';
```

```tsx
// in components/index.ts
export {default as Button, type ButtonProps} from './Button';
```

```tsx
// in a view using Button and other components
import {Button, Modal, Input} from './components';
```