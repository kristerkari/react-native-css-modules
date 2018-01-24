# React Native CSS modules

<img src="images/react-native-logo.png" width="160"><img src="images/plus.svg" width="100"><img src="images/css-modules-logo.svg" width="170">

A guide for using [CSS modules](https://github.com/css-modules/css-modules) (with some limitations) for both Web React and React Native.

## Why?

React Native does not offer any kind of built-in support for loading CSS from `.css` files and using it for styling. Many of us are already using CSS modules in an existing project and want to continue using CSS modules when developing React Native apps.

So far the only way has been to use React Native's `style` property or any of the available CSS-in-JS libraries.

The idea for React Native CSS modules comes from these projects that have made a lot of work for supporting CSS and CSS modules in React Native: [css-to-react-native](https://github.com/styled-components/css-to-react-native) and [react-native-sass-classname](https://github.com/daniloster/react-native-sass-classname). A big thanks to them!

## Features

* You can share your CSS modules between React Native and React Web by using `className` property in React Native, and by using [React Native for Web](https://github.com/necolas/react-native-web) for the browser.
* Supports [CSS](https://github.com/kristerkari/react-native-css-transformer), [PostCSS](https://github.com/kristerkari/react-native-postcss-transformer) and [Sass](https://github.com/kristerkari/react-native-sass-transformer).
* Hot loading for CSS/PostCSS/Sass files.
* [Platform-specific extensions](https://facebook.github.io/react-native/docs/platform-specific-code.html#platform-specific-extensions) for CSS, e.g. `.ios.css`, `.android.css`, `.native.css`.
* [Supports two syntaxes for using multiple classes](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style#multiple-classes) that work with React Native CSS modules and regular CSS modules.
* [Typescript types that are compatible with React Native CSS modules](https://github.com/kristerkari/react-native-types-for-css-modules)

## But there are some limitations...

First of all, React Native has a very limited support for styling. There are some styling properties that do not exist in regular CSS and some properties work differently than in CSS, so don't treat it as something that is 100% compatible with regular CSS.

The supported styling depends on the element that you want to style. The best thing is to have a look at this cheat sheet when ever your are writing your styles: https://github.com/vhpoet/react-native-styling-cheat-sheet

If you plan to use the same CSS files for both React Native and Web, then I suggest that you build your app "React Native first". It is much easier to build the app with React Native's styling limitations, and then make it work for web using [React Native for Web](https://github.com/necolas/react-native-web) and by adding some Web specific CSS.

### Current limitations for CSS modules in React Native

Many of these will be fixed in the near future.

* No hot loading for Sass files that are imported with `@import` yet.
* No `:global` or `:local` keywords for CSS modules with React Native.
* No support for using [classnames](https://github.com/JedWatson/classnames) module for multiple classnames (`classnames` outputs classnames as a string).
* No way to pass options to Sass (`node-sass`) yet (planned).
* CSS styling is limited to what React Native supports for styling: https://github.com/vhpoet/react-native-styling-cheat-sheet

---

## Example App

Have a look at the example app to see how you can use CSS modules for both React Native and Web using the same code.

* [react-native-css-modules-example](https://github.com/kristerkari/react-native-css-modules-example)

---

## Documentation

* [Setup React Native CSS modules with CSS support](docs/setup-css.md)
* [Setup React Native CSS modules with PostCSS support](docs/setup-postcss.md)
* [Setup React Native CSS modules with Sass support](docs/setup-sass.md)
* [Install Typescript types that are compatible with React Native CSS modules](https://github.com/kristerkari/react-native-types-for-css-modules#installation)
* [Use CSS modules with Typescript or use CSS and Sass in the same project](docs/multiple-transformers.md)
* [Setup Web compatibility for React Native CSS modules](docs/web-compatibility.md)
