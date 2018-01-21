## Setup CSS modules for React Native

The following modules are used to implement CSS modules support for React Native:

* [react-native-css-transformer](https://github.com/kristerkari/react-native-css-transformer) (or [PostCSS version](https://github.com/kristerkari/react-native-postcss-transformer), [Sass version](https://github.com/kristerkari/react-native-sass-transformer)) - Transforms CSS code to a React Native compatible style object and handles live reloading
* [babel-plugin-react-native-platform-specific-extensions](https://github.com/kristerkari/babel-plugin-react-native-platform-specific-extensions) - Transforms ES6 `import` statements to platform specific `require` statements if the platform specific files exist on disk.
* [babel-plugin-react-native-classname-to-style](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style) - Transforms `className` property to `style` property.

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

```json
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

```json
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
