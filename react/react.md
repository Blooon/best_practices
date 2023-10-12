[< Back to README](../README.md)

# **React**

[Components](#components)

- [Naming](#naming)

- [Implementation](#implementation)

[Contexts](#contexts)

[Hooks and HOCs](#hooks-and-hocs)

[Styles](#styles)

## **Components**

### **Naming**

- You component’s name must always be **PascalCase** :
  - As well as TypeScript Types and Interfaces and JavaScript Classes
  - Any other entity should be **camelCase**
  - Same rules apply to the respective files of these entities
    - No helpers, utils or config file name should be PascalCase
    - Nor contain JSX and therefore should not bear .jsx or .tsx extension
  - Component’s file must always bear exact name of default exported Component
  - Only one component per file as much as possible
    - Unless it’s a really tiny Component that should remain private (in the scope of the default Component)
- You component’s name should be **unique** across the whole app :
  - Otherwise, it either means one of both is badly named or both component almost do the same thing and can be refactored
  - Better have 3 components with long names but we can identify in the blink of eye, rather than 3 components having the same name within 3 differents contexts that easily become confused
- You component’s name should be **precise** and **relevant** :
  - Avoid using meaningless words like Block, Component, Entry or any too generic word
- You component’s name must be **concistent** :
  - Concistency appears when keeping the same logic everywhere for the same matter
  - Choose some names for some kind of data entity or UI element and stick to it
    - even if the naming is wrong
    - or refactor them all
    - e.g. : if you make no difference between Dialog and Modal, choose one and use it to name all your modals
- You component’s name should inform as much as possible on **what is does**, with **which data** **/ model** and **what it renders** :
  - You can follow this pattern : `<Purpose / Action><Context / Work or Data Entity><UI Element>`
    - Purpose / Action : what the component is for or does
    - Context / Work or Data Entity :
      - what work or data entity it is working on
      - this should be null for atomic components in src/components
      - but not necessarly for molecule or organism components in src/components (e.g. a CloseTicketModal or a BookmarkCard that you'ld reuse in several places in the app)
    - UI Element : what kind of UI element the component is, this is important to quickly identify what is the main render of the component, you can base on your UI library like [Material UI](https://v5-0-6.mui.com/components/autocomplete/) to get the right UI element name

### **Implementation**

- Use fonction components rather than class components
  - Fonction components are easier to read and thus maintain
  - You can use hooks in fonction components
  - Class components should only be used if you need a fine control over the component’s lifecycle (for exemple if you need to use shouldComponentUpdate or prevState and prevProps in componentDidUpdate)
- Component should be pure as much as possible, particularly atom components
- Component should only do one thing or serve one purpose
- Component should be as tiny as possible
  - Split your component in many sub-components if needed
  - Better have many components with separation of concerns and renderings rather than big view defined in one component
- [Use composition over inheritance](https://fr.legacy.reactjs.org/docs/composition-vs-inheritance.html)

## **Contexts**

- First, [Before You Use Context](https://react.dev/learn/passing-data-deeply-with-context#before-you-use-context)
- Context is powerful and useful as it can reduce props drilling and therefore can reduce some boilerplate giving some sub-components access to some props or functions
- However :
  - It makes you component harder to encapsulate and thus to test
  - It adds rigidity to your code making refactoring harder
  - It makes you data or props flow harder to follow, as context adds hidden (or implicitly available) states or props, unlike the classic parent-children two-way binding of props
  - It adds unecessary complexity if you create a context for something used in only one single place in the app
- If you want to make some state and setState accessible to many sub-components, first try to [Lift up state](https://react.dev/learn/sharing-state-between-components#lifting-state-up-by-example)
- If you have too many props descending too deep in the tree, then try :
  - to use [Containment Composition](https://fr.legacy.reactjs.org/docs/composition-vs-inheritance.html)
  - to use a [render prop](https://fr.legacy.reactjs.org/docs/render-props.html)
- Finally, if those tricks don’t work for you :
  - You should consider either splitting or refactoring your component first
  - And then keep in mind that it’s better to have an easier to read and thus debug data / prop flow, rather than some too complex pattern that brilliantly does the job but you’re the only one that can debug it in a minute

## **Hooks and HOCs**

- First, [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect)
- Use hooks rather than HOCs
  - HOCs make your code hard to read and thus maintain
  - HOCs often create wrappers to encapsulate their children which can overload the component tree too much making the component tree hard to read :
    - withReducer(withGraphQLRequest(withState(withUserData(withStyle(Component)(style))))
  - HOCs are often defined at the bottom of the file which :
    - makes it easy to be missed by someone looking at your component for the first time
    - forces you to jump between top and bottom of the file to understand the component’s workflow
- Create your own hooks in src/hooks to encapsulate behaviours
  - The same useEffect used twice in different components should be refactored
  - [How to reuse logic with custom hook](https://react.dev/learn/reusing-logic-with-custom-hooks)
- You can name you useEffect and useMemo functions to improve readability and debuggability

```jsx
// name your useEffect function

// therefore you don't need to read the useEffect to know what it does
// this will also log the function name in you stack trace if it crashes

useEffect(function useFetchTags() {
  fetchTags(contractNumber)
  .then(response => {
    const tags = isNotEmpty(response) ? response.map(tag => ({ value: tag, label: tag })) : [];
    setTags(tags);
  })
  .catch(e => setError(e.message);
}, [contractNumber]);
```

```jsx
// the same works for useMemo

const packages = useMemo(function extractPackagesInfoFromMaintenance() { 
  const packages = {};

  for (const [packageName, packageVersion] of Object.entries(packagesWithVersion)) {
    packages[packageName] = {
      name: packageName,
      version: packageVersion
    };

    const appVersionHistory = maintenance.lastUpdateCall.appVersionHistorical.find(appVersionHistory => appVersionHistory?.packageName === packageName)?.history;

    if (isNotEmpty(appVersionHistory)) {
      const [lastUpdateLog, ...history] = appVersionHistory;

      packages[packageName].history = history.map(({since, versionName}) => ({updateDate: tsToFullDate(since), version: versionName}));

      const packageLastUpdateDate = lastUpdateLog?.since;
      if (packageLastUpdateDate) packages[packageName].lastUpdateDate = tsToFullDate(packageLastUpdateDate);
    }
  }

  return packages;
}, [maintenance]);
```

## **Styles**

- Try to define styles in the file where they are used as much as possible
  - Things of same concern should be in the same place
- Use CSS file imports
