# Vue.js component development guide

## Environment

- Our projects have predefined linting and styling rules that work out of the box with **Visual Studio Code** with the [Prettier extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) installed.

## Components

- The components should be self-contained and well-written using natural, idiomatic code. Everything needed for the component should be in one file: the component file.
- Information displayed by components must be fully configurable using properly typed props that have defined default values.
- In `Home.vue`, include example usage of each component, with example data.

## Styling

- Do not use inline CSS in templates. Use properly scoped SCSS in the style section of the file.
- Stick to using CSS3 for variables, not SCSS.
- Use the provided icons, fonts, and colours, if applicable.

## Responsiveness

Components and pages must be **fully responsive** and mobile first. This means that implementations should first be tested on mobile devices followed by larger screens.

- Components should fully scale to the width of their container
- Always use `display: flex;`.
- Always use `%` and `rem`.
- Margins, paddings, and font sizes must be defined using `rem`.

## Frameworks

- Do not use Vuetify, Bootstrap, or any other UI frameworks. Components should avoid third party code unless absolutely necessary.
- You may use a JavaScript library to provide extra functionality **if required**. Examples of acceptable libraries are `progressbar.js`, `leaflet.js`, `hammer.js`, and `three.js`.
