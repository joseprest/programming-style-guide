# Vue.js component development guide

## Environment

- Our projects have predefined linting and styling rules that work out of the box with **Visual Studio Code** with the [Prettier extension](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode) installed.

## Components

- Everything needed for the component should be in one file: the component file.
- Information displayed by components must be fully configurable using properly typed props that have defined default values.
- Include example usage of each component, with example data, in the associated vue.

## Styling

- Do not use inline CSS in templates. Use properly scoped SCSS in the style section of the file.
- Stick to using the provided CSS3 variables, and do not use SCSS to create any new ones.
- Use the provided icons, fonts, and colours, if applicable.
- Use `npm run lint --fix` to automatically fix styling errors. The project should have no errors or warnings.

## Responsiveness

- Components and pages must be **fully responsive** and mobile first. This means that implementations should first be tested on mobile devices followed by larger screens.
- Each component should automatically scale to its container's width (width: 100%) and automatically adjust its height to display its content correctly.  
- Always use `display: flex;`.
- Always use `%` and `rem`.
- Margins, paddings, and font sizes must be defined using `rem`.

## Frameworks

- Do not use Vuetify, Bootstrap, or any other UI frameworks. Components should avoid third party code unless absolutely necessary.
- You may use a JavaScript library to provide extra functionality **if required**. Examples of acceptable libraries are `progressbar.js`, `leaflet.js`, `hammer.js`, and `three.js`.
