# React Native CSS modules

**Quick links:** **[Features](#features)** • **[Documentation](https://github.com/kristerkari/react-native-css-modules#documentation)** • **[Examples](#example-apps)**

<a href="https://facebook.github.io/react-native/"><img src="images/react-native-logo.png" width="160"></a><img src="images/plus.svg" width="100"><a href="https://github.com/css-modules/css-modules"><img src="images/css-modules-logo.svg" width="170"></a>

A guide for using [CSS modules](https://github.com/css-modules/css-modules) (with some limitations) for both Web React and React Native.

## Why?

React Native does not offer any kind of built-in support for loading CSS from `.css` files and using it for styling. Many of us are already using CSS modules in an existing project and want to continue using CSS modules when developing React Native apps.

So far the only way has been to use React Native's `style` property or any of the available CSS-in-JS libraries. Now you can use `className` property and keep your styles in separate CSS files.

## Features

* You can share your CSS modules between React Native and React Web by using `className` property in React Native, and by using [React Native for Web](https://github.com/necolas/react-native-web) for the browser.
* Supports [CSS](https://github.com/kristerkari/react-native-css-transformer), [PostCSS](https://github.com/kristerkari/react-native-postcss-transformer) and [Sass](https://github.com/kristerkari/react-native-sass-transformer).
* Hot loading for CSS/PostCSS/Sass files.
* [Platform-specific extensions](https://facebook.github.io/react-native/docs/platform-specific-code.html#platform-specific-extensions) for CSS, e.g. `.ios.css`, `.android.css`, `.native.css`.
* [Supports 2 syntaxes for using multiple CSS classes](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style#multiple-classes) that work with React Native CSS modules and regular CSS modules.
* [Typescript types that are compatible with React Native CSS modules](https://github.com/kristerkari/react-native-types-for-css-modules)
* [Custom stylelint config for React Native CSS modules](https://github.com/kristerkari/stylelint-config-react-native-css-modules)

## Example Apps

Have a look at the example apps to see how you can use CSS modules for both React Native and Web using the same code.

* [React Native CSS modules example app](https://github.com/kristerkari/react-native-css-modules-example)
* [React Native CSS modules with CSS media queries example app](https://github.com/kristerkari/react-native-css-modules-with-media-queries-example)
* [React Native CSS modules with Typescript example app](https://github.com/kristerkari/react-native-css-modules-with-typescript-example)

## Documentation

* [Setup React Native CSS modules with CSS support](docs/setup-css.md)
* [Setup React Native CSS modules with PostCSS support](docs/setup-postcss.md)
* [Setup React Native CSS modules with Sass support](docs/setup-sass.md)
* [Setup React Native CSS modules + CSS media queries](docs/setup-media-queries.md)
* [Setup React Native CSS modules + Typescript](docs/setup-typescript.md)
* [Use CSS and Sass in the same project](docs/multiple-transformers.md)
* [Setup Web compatibility for React Native CSS modules](docs/web-compatibility.md)
* [Stylelint config for React Native CSS modules](https://github.com/kristerkari/stylelint-config-react-native-css-modules)
* [List of CSS properties supported by React Native](https://github.com/vhpoet/react-native-styling-cheat-sheet)
* [List of React Native CSS modules limitations](docs/limitations.md)

---

## Special thanks

The idea for React Native CSS modules comes from these projects that have made a lot of work for supporting CSS and CSS modules in React Native: [css-to-react-native](https://github.com/styled-components/css-to-react-native) and [react-native-sass-classname](https://github.com/daniloster/react-native-sass-classname). A big thanks to them!
