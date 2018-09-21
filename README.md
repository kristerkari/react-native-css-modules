# React Native CSS modules

**Quick links:** **[Features](#features)** • **[Documentation](https://github.com/kristerkari/react-native-css-modules#documentation)** • **[Examples](#example-apps)** • **[Development](#development)**

<a href="https://facebook.github.io/react-native/"><img src="images/react-native-logo.png" width="160"></a><img src="images/plus.svg" width="100"><a href="https://github.com/css-modules/css-modules"><img src="images/css-modules-logo.svg" width="170"></a>

## Why?

React Native does not offer any kind of built-in support for loading CSS from `.css` files and using it for styling.

So far the only way has been to use React Native's `style` property or any of the available CSS-in-JS libraries. Now you can use `className` property and keep your styles in separate CSS files.

## Features

- :tada: You can share your CSS modules between React Native and React Web by using `className` property in React Native, and by using [React Native for Web](https://github.com/necolas/react-native-web) for the browser.
- :ok_hand: Supports [CSS](https://github.com/kristerkari/react-native-css-transformer), [PostCSS](https://github.com/kristerkari/react-native-postcss-transformer), [Sass](https://github.com/kristerkari/react-native-sass-transformer), [Less](https://github.com/kristerkari/react-native-less-transformer) and [Stylus](https://github.com/kristerkari/react-native-stylus-transformer).
- :fire: CSS Hot loading (live reloading).
- :computer: Supports responsive CSS features: CSS Media Queries and CSS Viewport Units.
- :globe_with_meridians: [Platform-specific extensions](https://facebook.github.io/react-native/docs/platform-specific-code.html#platform-specific-extensions) for CSS, e.g. `styles.ios.css`, `styles.android.css`, `styles.native.css`.
- :tophat: Support for `styleName` attribute that allows you to use CSS class names as strings, and allows hyphens in class names.
- :package: [Typescript typings that are compatible with React Native CSS modules](https://github.com/kristerkari/react-native-types-for-css-modules)
- :mag: [Custom stylelint config for React Native CSS modules](https://github.com/kristerkari/stylelint-config-react-native-css-modules)

## Development

To see which new features are being planned and what is in progress, please have a look at:
[the development board](https://github.com/kristerkari/react-native-css-modules/projects/1)

If you want to suggest a new feature or report a bug, please open a new issue.

## Example Apps

Have a look at the example apps to see how you can use CSS modules for both React Native and Web using the same code.

- **[Example app](https://github.com/kristerkari/react-native-css-modules-example)**
- **[CSS Media Queries example app](https://github.com/kristerkari/react-native-css-modules-with-media-queries-example)**
- **[CSS Viewport Units example app](https://github.com/kristerkari/react-native-css-modules-with-viewport-units-example)**
- **[Example app with styleName syntax](https://github.com/kristerkari/react-native-css-modules-stylename-example)**
- **[Typescript example app](https://github.com/kristerkari/react-native-css-modules-with-typescript-example)**

## Documentation

#### :books: Basic setup

- **[Setup CSS modules with CSS support](docs/setup-css.md)**
- **[Setup CSS modules with PostCSS support](docs/setup-postcss.md)**
- **[Setup CSS modules with Sass support](docs/setup-sass.md)**
- **[Setup CSS modules with Less support](docs/setup-less.md)**
- **[Setup CSS modules with Stylus support](docs/setup-stylus.md)**
- **[Setup CSS modules with Responsive CSS support (CSS Media Queries & CSS Viewport Units)](docs/setup-responsive.md)**

#### :books: Other setup docs

- **[Setup recommended linting (ESLint & stylelint)](docs/setup-linting.md)**
- **[Setup CSS modules with Typescript support](docs/setup-typescript.md)**
- **[Setup CSS modules with styleName attribute (use className as a string)](docs/setup-stylename.md)**
- **[Use CSS and Sass in the same project](docs/multiple-transformers.md)**
- **[Setup browser compatibility](docs/web-compatibility.md)**

#### :books: Other documentation

- **[Frequently Asked Questions](docs/faq.md)**
- **[Stylelint config for React Native CSS modules](https://github.com/kristerkari/stylelint-config-react-native-css-modules)**
- **[List of CSS properties supported by React Native (out of date)](https://github.com/vhpoet/react-native-styling-cheat-sheet)**

---

## Special thanks

The idea for React Native CSS modules comes from these projects that have made a lot of work for supporting CSS and CSS modules in React Native: [css-to-react-native](https://github.com/styled-components/css-to-react-native) and [react-native-sass-classname](https://github.com/daniloster/react-native-sass-classname). A big thanks to them!
