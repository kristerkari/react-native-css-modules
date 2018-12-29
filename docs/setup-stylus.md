## Setup CSS modules for React Native (with Stylus support)

Following libraries are needed:

- [react-native-stylus-transformer](https://github.com/kristerkari/react-native-stylus-transformer) - Transforms Stylus to a React Native compatible style object and handles live reloading
- [babel-plugin-react-native-platform-specific-extensions](https://github.com/kristerkari/babel-plugin-react-native-platform-specific-extensions) - Transforms ES6 `import` statements to platform specific `require` statements if the platform specific files exist on disk.
- [babel-plugin-react-native-classname-to-style](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style) - Transforms `className` property to `style` property.

### Step 1: Install depencies to run React Native

Make sure that you have `react-native-cli` installed (`npm install -g react-native-cli`) and [XCode](https://developer.apple.com/xcode/) (for iOS development) / [Android Studio](https://developer.android.com/studio/index.html) (for Android development) installed and working.

- Go to "Building Projects with Native Code" tab and follow the guide: https://facebook.github.io/react-native/docs/getting-started.html

### Step 2: Create a new React Native app and test that it works

e.g.

```sh
react-native init AwesomeProject
cd AwesomeProject
```

Start packager:

```sh
yarn start
```

Run project on iOS simulator:

```sh
react-native run-ios
```

### Step 3: Install dependencies for React Native CSS modules

```sh
yarn add babel-plugin-react-native-classname-to-style babel-plugin-react-native-platform-specific-extensions react-native-stylus-transformer stylus --dev
```

### Step 4: Setup your project's `.babelrc`

#### For React Native v0.57 or newer

```json
{
  "presets": ["module:metro-react-native-babel-preset"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["styl"]
      }
    ]
  ]
}
```

---

#### For React Native v0.56 or older

```json
{
  "presets": ["react-native"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["styl"]
      }
    ]
  ]
}
```

### Step 5: Setup `rn-cli.config.js` in your project

#### For React Native v0.57 or newer

Add this to `rn-cli.config.js` in your project's root (create the file if you don't have one already):

```js
const { getDefaultConfig } = require("metro-config");

module.exports = (async () => {
  const {
    resolver: { sourceExts }
  } = await getDefaultConfig();
  return {
    transformer: {
      babelTransformerPath: require.resolve("react-native-stylus-transformer")
    },
    resolver: {
      sourceExts: [...sourceExts, "styl"]
    }
  };
})();
```

---

#### For React Native v0.56 or older

Add this to `rn-cli.config.js` in your project's root (create the file if you don't have one already):

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-stylus-transformer");
  },
  getSourceExts() {
    return ["js", "jsx", "styl"];
  }
};
```

...or if you are using [Expo](https://expo.io/), in `app.json`:

```json
{
  "expo": {
    "packagerOpts": {
      "sourceExts": ["js", "jsx", "styl"],
      "transformer": "node_modules/react-native-stylus-transformer/index.js"
    }
  }
}
```

### Step 6: Add some Stylus to your project and use it inside a React component

`styles.styl`:

```styl
.container {
  flex: 1;
  justify-content: center;
  align-items: center;
  background-color: #f5fcff;
}

.blue {
  color: blue;
  font-size: 30px;
}
```

Add style import and `BlueText` component to `App.js`:

```jsx
import React, { Component } from "react";
import { Text, View } from "react-native";
import styles from "./styles.styl";

const BlueText = () => {
  return <Text className={styles.blue}>Blue Text</Text>;
};

export default class App extends Component<{}> {
  render() {
    return (
      <View style={styles.container}>
        <BlueText />
      </View>
    );
  }
}
```

### Step 7: Restart packager and clear cache

Restart React Native packager and clear it's cache (important) to see the styles that you added.

```sh
yarn start --reset-cache
```
