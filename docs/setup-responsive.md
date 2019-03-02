## Setup CSS modules for React Native (with CSS Media Queries & CSS Viewport Units support)

Following library needs to be taken into use:

- [babel-plugin-react-native-classname-to-dynamic-style](https://github.com/kristerkari/babel-plugin-react-native-classname-to-dynamic-style) - Transforms `className` property to `style` property and matches dynamic styles (media queries and viewport units) at runtime with React Native.

### Step 1: Setup React Native CSS modules

- [Setup React Native CSS modules with CSS support](setup-css.md)
- [Setup React Native CSS modules with PostCSS support](setup-postcss.md)
- [Setup React Native CSS modules with Sass support](setup-sass.md)
- [Setup React Native CSS modules with Less support](setup-less.md)
- [Setup React Native CSS modules with Stylus support](setup-stylus.md)

### Step 2: Change the className Babel plugin to a dynamic one

Remove old one:

```sh
yarn remove babel-plugin-react-native-classname-to-style
```

Add new one:

```sh
yarn add babel-plugin-react-native-classname-to-dynamic-style --dev
```

### Step 3: Change Babel configuration

`.babelrc` (or `babel.config.js`)

```diff
  "plugins": [
-    "react-native-classname-to-style",
+    "react-native-classname-to-dynamic-style",
```

### Step 4: Add some CSS with Media Queries and Viewport Units to your project and use it inside a React component

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

### Step 5: Restart packager and clear cache

Restart React Native packager and clear it's cache (important) to see the styles that you added.

```sh
yarn start --reset-cache
```
