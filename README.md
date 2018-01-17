# React Native CSS modules

A guide for using [CSS modules](https://github.com/css-modules/css-modules) (with some limitations) for both Web React and React Native.

## Why?

React Native does not offer any kind of built-in support for loading CSS from `.css` files and using it for styling. Many of us are already using CSS modules in an existing project and want to continue using CSS modules when developing React Native apps.

So far the only way has been to use React Native's `style` property or any of the available CSS-in-JS libraries.

The idea for React Native CSS modules comes from these projects that have made a lot of work for supporting CSS and CSS modules in React Native: [css-to-react-native](https://github.com/styled-components/css-to-react-native) and [react-native-sass-classname](https://github.com/daniloster/react-native-sass-classname). A big thanks to them!

## Features

* You can share your CSS modules between React Native and React Web by using `className` property in React Native, and by using [React Native for Web](https://github.com/necolas/react-native-web) for the browser.
* You can use CSS or Sass files.
* Hot loading for CSS/Sass files.
* [Platform-specific extensions](https://facebook.github.io/react-native/docs/platform-specific-code.html#platform-specific-extensions) for CSS, e.g. `.ios.css`, `.android.css`, `.native.css`.
* [Supports two syntaxes for using multiple classes](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style#multiple-classes) that work with React Native CSS modules and regular CSS modules.

## But there are some limitations...

First of all, React Native has a very limited support for styling. There are some styling properties that do not exist in regular CSS and some properties work differently than in CSS, so don't treat it as something that is 100% compatible with regular CSS.

The supported styling depends on the element that you want to style. The best thing is to have a look at this cheat sheet when ever your are writing your styles: https://github.com/vhpoet/react-native-styling-cheat-sheet

If you plan to use the same CSS files for both React Native and Web, then I suggest that you build your app "React Native first". It is much easier to build the app with React Native's styling limitations, and then make it work for web using [React Native for Web](https://github.com/necolas/react-native-web) and by adding some Web specific CSS.

### Current limitations for CSS modules in React Native

Many of these will be fixed in the near future.

* No support for other pre/postprocessors like [Less](http://lesscss.org/) or [PostCSS](http://postcss.org/) yet on React Native (planned).
* No hot loading for Sass files that are imported with `@import` yet.
* No `:global` or `:local` keywords for CSS modules with React Native.
* No support for using [classnames](https://github.com/JedWatson/classnames) module for multiple classnames (`classnames` outputs classnames as a string).
* No way to pass options to Sass (`node-sass`) yet (planned).
* No Typescript types that allow you to use `className` for React Native elements yet (planned).
* No way to use both `.css` and `.scss` extensions in a single project yet (planned).
* CSS styling is limited to what React Native supports for styling: https://github.com/vhpoet/react-native-styling-cheat-sheet

## Example App

Have a look at the example app to see how you can use CSS modules for both React Native and Web using the same code.

* [react-native-css-modules-example](https://github.com/kristerkari/react-native-css-modules-example)

## Setup CSS modules for React Native

The following modules are used to implement CSS modules support for React Native:

* [react-native-css-transformer](https://github.com/kristerkari/react-native-css-transformer) (or [Sass version](https://github.com/kristerkari/react-native-sass-transformer))
* [babel-plugin-react-native-platform-specific-extensions](https://github.com/kristerkari/babel-plugin-react-native-platform-specific-extensions)
* [babel-plugin-react-native-classname-to-style](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style)

### Step 1: Install depencies to run React Native

Make sure that you have `react-native-cli` installed and [XCode](https://developer.apple.com/xcode/)/[Android Studio](https://developer.android.com/studio/index.html) installed and working.

* Go to "Building Projects with Native Code" tab and follow the guide: https://facebook.github.io/react-native/docs/getting-started.html

### Step 2: Create a new React Native app and test that it works

e.g.

```sh
react-native init AwesomeProject
cd AwesomeProject
```

Start packager:

```sh
npm start
```

Run project on iOS simulator:

```sh
react-native run-ios
```

### Step 3: Install dependencies for React Native CSS modules

For CSS:

```sh
npm install babel-plugin-react-native-classname-to-style babel-plugin-react-native-platform-specific-extensions react-native-css-transformer --save-dev
```

For Sass:

```sh
npm install babel-plugin-react-native-classname-to-style babel-plugin-react-native-platform-specific-extensions react-native-sass-transformer node-sass --save-dev
```

### Step 4: Setup your project's `.babelrc`

For CSS:

```
{
  "presets": ["react-native"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["css"]
      }
    ]
  ]
}
```

For Sass:

```
{
  "presets": ["react-native"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["scss", "sass"]
      }
    ]
  ]
}
```

### Step 5: Setup `rn-cli.config.js` in your project

Add this to your `rn-cli.config.js` (make one if you don't have one already):

For CSS:

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-css-transformer");
  },
  getSourceExts() {
    return ["css"];
  },
};
```

For Sass:

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-sass-transformer");
  },
  getSourceExts() {
    return ["scss", "sass"];
  },
};
```

### Step 6: Add some CSS to your project and use it inside a React component

mystyles.css:

```css
.blue {
  color: blue;
  font-size: 30px;
}
```

Add style import and `BlueText` component to `App.js`:

```jsx
/**
 * Sample React Native App
 * https://github.com/facebook/react-native
 * @flow
 */

import React, { Component } from "react";
import { Platform, StyleSheet, Text, View } from "react-native";
import myStyles from "./mystyles.css";

const instructions = Platform.select({
  ios: "Press Cmd+R to reload,\n" + "Cmd+D or shake for dev menu",
  android:
    "Double tap R on your keyboard to reload,\n" +
    "Shake or press menu button for dev menu",
});

const BlueText = () => {
  return <Text className={myStyles.blue}>Blue Text</Text>;
};

export default class App extends Component<{}> {
  render() {
    return (
      <View style={styles.container}>
        <BlueText />
        <Text style={styles.welcome}>Welcome to React Native!</Text>
        <Text style={styles.instructions}>To get started, edit App.js</Text>
        <Text style={styles.instructions}>{instructions}</Text>
      </View>
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
    backgroundColor: "#F5FCFF",
  },
  welcome: {
    fontSize: 20,
    textAlign: "center",
    margin: 10,
  },
  instructions: {
    textAlign: "center",
    color: "#333333",
    marginBottom: 5,
  },
});
```

### Step 7: Restart packager and clear cache

Restart React Native packager and clear it's cache (important) to see the styles that you added.

```sh
npm start -- --reset-cache
```

If it throws an error for missing `node_modules/react-native/local-cli/cli.js`, just run `npm install` and then try again.

## Setup Web compatibility for React Native CSS modules

This section needs to be written...

What you need is:

* [Webpack](https://webpack.js.org/) (or some other bundler)
* [React Native for Web](https://github.com/necolas/react-native-web)

Have a look at the [webpack.config.js](https://github.com/kristerkari/react-native-css-modules-example/blob/master/webpack.config.js) of the example app.
