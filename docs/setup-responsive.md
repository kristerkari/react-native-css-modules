## Setup CSS modules for React Native (with CSS Media Queries & CSS Viewport Units support)

_This example only shows how to setup CSS support using the CSS transformer. Have a look at the setup documentation if you need PostCSS, Sass, Less or Stylus support (you need to use a different transformer and file extensions)._

Following libraries are needed:

- [react-native-css-transformer](https://github.com/kristerkari/react-native-css-transformer) - Transforms CSS to a React Native compatible style object and handles live reloading
- [babel-plugin-react-native-platform-specific-extensions](https://github.com/kristerkari/babel-plugin-react-native-platform-specific-extensions) - Transforms ES6 `import` statements to platform specific `require` statements if the platform specific files exist on disk.
- [babel-plugin-react-native-classname-to-dynamic-style](https://github.com/kristerkari/babel-plugin-react-native-classname-to-dynamic-style) - Transforms `className` property to `style` property and matches dynamic styles (media queries and viewport units) at runtime with React Native.

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
yarn add babel-plugin-react-native-classname-to-dynamic-style babel-plugin-react-native-platform-specific-extensions react-native-css-transformer --dev
```

### Step 4: Setup Babel configuration

#### For React Native v0.57 or newer

`.babelrc` (or `babel.config.js`)

```json
{
  "presets": ["module:metro-react-native-babel-preset"],
  "plugins": [
    "react-native-classname-to-dynamic-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["css"]
      }
    ]
  ]
}
```

---

#### For React Native v0.56 or older

`.babelrc`

```json
{
  "presets": ["react-native"],
  "plugins": [
    "react-native-classname-to-dynamic-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["css"]
      }
    ]
  ]
}
```

---

#### For Expo

`babel.config.js` (older Expo versions use `.babelrc`)

```js
module.exports = function(api) {
  api.cache(true);
  return {
    presets: ["babel-preset-expo"],
    plugins: [
      "react-native-classname-to-dynamic-style",
      ["react-native-platform-specific-extensions", { extensions: ["css"] }]
    ]
  };
};
```

### Step 5: Setup Metro bundler configuration

#### For React Native v0.57 or newer / Expo SDK v31.0.0 or newer

Add this to `metro.config.js` in your project's root (create the file if you don't have one already):

```js
const { getDefaultConfig } = require("metro-config");

module.exports = (async () => {
  const {
    resolver: { sourceExts }
  } = await getDefaultConfig();
  return {
    transformer: {
      babelTransformerPath: require.resolve("react-native-css-transformer")
    },
    resolver: {
      sourceExts: [...sourceExts, "css"]
    }
  };
})();
```

If you are using [Expo](https://expo.io/), you also need to add this to `app.json`:

```json
{
  "expo": {
    "packagerOpts": {
      "config": "metro.config.js"
    }
  }
}
```

---

#### For React Native v0.56 or older

If you are using React Native without Expo, add this to `rn-cli.config.js` in your project's root (create the file if you don't have one already):

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("react-native-css-transformer");
  },
  getSourceExts() {
    return ["js", "jsx", "css"];
  }
};
```

---

#### For Expo SDK v30.0.0 or older

If you are using [Expo](https://expo.io/), instead of adding the `rn-cli.config.js` file, you need to add this to `app.json`:

```json
{
  "expo": {
    "packagerOpts": {
      "sourceExts": ["js", "jsx", "css"],
      "transformer": "node_modules/react-native-css-transformer/index.js"
    }
  }
}
```

### Step 6: Add some CSS with media queries to your project and use it inside a React component

`styles.css`:

```css
.container {
  flex: 1;
  justify-content: center;
  align-items: center;
  background-color: #f5fcff;
}

.blue {
  color: blue;
  font-size: 3vmax;
}

@media (orientation: landscape) {
  .blue {
    color: red;
    font-size: 10px;
  }
}
```

Add style import and `BlueText` component to `App.js`:

```jsx
import React, { Component } from "react";
import { Text, View } from "react-native";
import styles from "./styles.css";

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
