# **Tests**

[A simple component is simple to test](#a-simple-component-is-simple-to-test)

[Do NOT test implementation details](#do-not-test-implementation-details)

[Do NOT test third party libraries](#do-not-test-third-party-libraries)

[Tests should mock as little as possible](#tests-should-mock-as-little-as-possible)

[Snapshot testing](#snapshot-testing)

[Docs](#docs)

[Other useful sources](#other-useful-sources)

## **A simple component is simple to test**

- Component’s specs should not be more than 4 or 5 tests long
- Otherwise it means that there’s too much things to test and your component may do too much things
- Thus, it might be the time to refactor and split your component

## **Do NOT test implementation details**

Motivations :

- Testing implementation details doesn’t match what your end user care, do or see
- It creates false negatives when refactoring code or renaming variables :
  - If you change a handler’s name, your test will break even if your component still works
- It creates false positives when you change code that affects the expected render or behaviour :
  - If you test the handler and the associated component’s state change instead of testing the expected render for a given user event, your test might pass but your component may not do or render what it should

Do NOT :

- Use data-testid as much as possible, to be reliably accessible, your UI should be testable without use of data-testid
- Test the props, state or event handlers of a component : 
  - If you change your state or prop variable name, your test break even if the behaviour remains the same
  - If you change your event handler’s name, your test break even if the behaviour remains the same

DO :

- cleanup the component after each for...each test, because it's almost a live app and one test will affect another test

Sources :

- [Testing Implementation Details](https://kentcdodds.com/blog/testing-implementation-details#why-is-testing-implementation-detailsbad)
- [Why I Never Use Shallow Rendering](https://kentcdodds.com/blog/why-i-never-use-shallow-rendering#without-shallow-rendering)

## **Do NOT test third party libraries**

- It is not your job to test these libraries
- It is up to the creators of these libraries to test it
- If you are not sure if a library is tested you should not use it
- Or you can read the source code to see if the author included tests : you can download the source code and run these tests yourself
- You can also ask the author if their library is production ready or not

## **Tests should mock as little as possible**

- Mocks rely on arbitrary data you set that might not really reflect production data
- This is one more thing to maintain
- Try to rely on dev or staging database / environment data as much as possible
- The more kinds of data your test pass through, the more solid it is

## **Snapshot testing**

Pros :

- Its very quick and easy to implement and sometimes requires only a few lines of code
- You can see if our component is rendering correctly, you can see the DOM nodes clearly with the .debug() function

Cons :

- The only thing a snapshot test does is tell you whether the syntax of your code has changed since the last test
- Basic rendering of the app correctly is React’s job so you're going a little into testing a third party library territory
- Comparing diffs can be done with git version control : this should not be the job of snapshot testing
- A failed snapshot test doesn’t mean your app isn’t working as intended, only that your code has changed since the last time you ran the test
- This can lead to a lot of false negatives and a lack of trust in the test
- This can also lead to people just updating the test without looking too closely at it
- Snapshot testing also tells you if your JSX is syntactically correct, but again this can be easily done in the dev environment : running a snapshot test just to check syntax errors doesnt make any sense
- It can become hard to understand what’s happening in a snapshot test, since most people use snapshot testing with shallow rendering, which doesnt render child components so it doesnt give the developer any insights at all

Conclusion :

- Use snapshot testing sparingly as it can create a bottleneck of new things to maintain
- Use snapshot testing in molecules or organisms components to test where atom sub-components have changed (based on [Atomic Design](https://blog-ux.com/quest-ce-que-latomic-design/))
  - You change the ModalHeader sub-component that is used in several Modals but you don’t exactly know which Modals
  - Snapshot tests implemented in these Modals will break and tell you there are changes to review in these renders

Sources :

- [How to make your snapshot testing more effective](https://kentcdodds.com/blog/effective-snapshot-testing)

## **Docs**

- [Jest](https://archive.jestjs.io/docs/en/getting-started)
- [Vitest](https://vitest.dev/guide/)
- [React Testing Library](https://testing-library.com/docs/react-testing-library/example-intro/#full-example)
- [JS DOM matchers](https://github.com/testing-library/jest-dom#table-of-contents)
- [Testing playground](https://testing-playground.com/)
- [Cypress](https://docs.cypress.io/guides/end-to-end-testing/writing-your-first-end-to-end-test)

## **Other useful sources**

- [How to test React Hooks](https://www.freecodecamp.org/news/testing-react-hooks/)
- [How to Test React Components: the Complete Guide](https://www.freecodecamp.org/news/testing-react-hooks/)
- [Testing React.js Hooks And Components: The Missing Piece](https://marmelab.com/blog/2021/08/31/testing-react.html)