# **TypeScript**

[Guidelines](#guidelines)

[Docs](#docs)

[Other useful sources](#other-useful-sources)

## **Guidelines**
- [Use import type and export type](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-3-8.html#type-only-imports-and-export) : so there’s no remnant of type annotations and declarations at runtime
- [Use Interface until you need Type](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/basic_type_example#types-or-interfaces) : interface for public API's and data definition and type for React Component Props and State
- [Prefer Interfaces over Intersections](https://github.com/microsoft/TypeScript/wiki/Performance#preferring-interfaces-over-intersections) : for performance and readability reasons
- [Prefer base types over Unions](https://github.com/microsoft/TypeScript/wiki/Performance#preferring-base-types-over-unions) : for performance reasons for big unions
- [Avoid React.FC](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/function_components) : before the React 18 type updates, React.FunctionComponent provided an implicit definition of children, even if your component doesn't have or use any children

## **Docs**

- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/docs/advanced/patterns_by_usecase/)
- [TypeScript Documentation](https://www.typescriptlang.org/docs/)
- [TypeScript Deep Dive](https://basarat.gitbook.io/typescript/type-system/type-assertion#as-foo-vs-foo)
- [TSS React](https://docs.tss-react.dev/)
- [usehooks-ts](https://usehooks-ts.com/react-hook/use-fetch)

## **Other useful sources**

- [Performance](https://github.com/microsoft/TypeScript/wiki/Performance)
- [TypeScript and React: Styles and CSS](https://fettblog.eu/typescript-react/styles/)
- [Functional React components with generic props in TS](https://wanago.io/2020/03/09/functional-react-components-with-generic-props-in-typescript/)
