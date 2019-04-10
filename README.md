# ntr.ui

**ntr.ui** is a multi-framework, cross-platform UI framework.

## Why

Most UI frameworks and UI applications are tied to a single framework and its ecosystem. This vendor lock-in is unnecessary and makes it very difficult to switch.

ntr.ui provides a set of basic tools for building:

- 1) UI apps that are not tied to a single framework
- 2) UI component libraries that support several frameworks
- 3) design systems

Finally, ntr.ui provides its own UI component library.

## Structure

- `@ntr/reset`: A configurable CSS reset
- `@ntr/base`: Base


## Basic idea

### UI component libraries

- A UI component library exports a set components, styles and behaviours for these components and additional global styles.
- UI components are in general purely functional, i.e. stateless.
- Some UI components come with a default implementation of stateful behaviour (e.g. tabs), but these algorithms should be kept separately, so they can be re-used and combined easily.
- All UI components should also be exposed as a flat hierarchy from the package, i.e. `myPackage/MyComponent` will have MyComponent as a default export.
- The Props of these components should be exposed as a type `Props` on the namespace of the Component, i.e.:

```ts

const Box: FC<Box.Props> = () => { /* ... */ }}

namespace Box {
    export interface Props {}
}

export default Box;
```

- The different output libraries should have the same structure and expose the same components, just 

```ts
// Box.descriptor.ts

const BoxDescriptor: ComponentDescriptor = { /*...*/ }
namespace BoxDescriptor {
	export interface Props {}
}
export default boxDescriptor

// Box.react.ts
import reactify from "@ntr/react-base/reactify"
import BoxDescriptor from "./Box.descriptor.ts

const Box = reactify
namespace Box {
    export interface Props extends BoxDescriptor.Props {}
}
```

### UI apps

- A UI app typically executes one (or multiple) mount operations.
- The mount operation is initially configured as required with the details of where to mount (e.g. the HTML element to use, or in React Native, the native "mount point")
- The mount operation is a single function accepting UI information (`mount: (jsx: ???) => void`)
- To make the selection of the UI elements and mounting library interchangable, they need to provide the same interface and be selected at compile time or run time.
- Here, we have two options:
  - Either, the components are made available through some kind of abstract factory / component repository.
  - Or, the components are imported from a library, which is subsequently overwritten, by the concrete target library for the platform in use (e.g. using references in the package.json or through webpack config or similar build tools)

