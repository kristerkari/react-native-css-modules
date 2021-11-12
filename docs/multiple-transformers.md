### Using CSS and Sass (or some other CSS preprocessor) in the same project

### Step 1: Setup React Native CSS modules

- [Setup React Native CSS modules with CSS support](setup-css.md)
- [Setup React Native CSS modules with PostCSS support](setup-postcss.md)
- [Setup React Native CSS modules with Sass support](setup-sass.md)
- [Setup React Native CSS modules with Less support](setup-less.md)
- [Setup React Native CSS modules with Stylus support](setup-stylus.md)

### Step 2: Modify Babel configuration

In your project's root folder:

#### For React Native v0.57 or newer / Expo SDK v31.0.0 or newer

Add more extensions to `.babelrc` (or `babel.config.js`):

```json
{
  "presets": ["module:metro-react-native-babel-preset"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["css", "scss", "sass"]
      }
    ]
  ]
}
```

... or if you are using [Expo](https://expo.io/), to `babel.config.js`

```js
module.exports = function (api) {
  api.cache(true);
  return {
    presets: ["babel-preset-expo"],
    plugins: [
      "react-native-classname-to-style",
      [
        "react-native-platform-specific-extensions",
        { extensions: ["css", "scss", "sass"] },
      ],
    ],
  };
};
```

---

#### For React Native v0.56 or older

Add more extensions to `.babelrc`:

```json
{
  "presets": ["react-native"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["css", "scss", "sass"]
      }
    ]
  ]
}
```

---

#### For Expo SDK v30.0.0 or older

Add more extensions to `.babelrc`:

```json
{
  "presets": ["babel-preset-expo"],
  "plugins": [
    "react-native-classname-to-style",
    [
      "react-native-platform-specific-extensions",
      {
        "extensions": ["css", "scss", "sass"]
      }
    ]
  ]
}
```

### Step 3: Modify Metro bundler configuration

In your project's root folder:

#### For React Native v0.57 or newer / Expo SDK v31.0.0 or newer

Configure `metro.config.js` to use a custom transformer file and add more extensions:

```js
const { getDefaultConfig } = require("metro-config");

module.exports = (async () => {
  const {
    resolver: { sourceExts },
  } = await getDefaultConfig();
  return {
    transformer: {
      babelTransformerPath: require.resolve("./transformer.js"),
    },
    resolver: {
      sourceExts: [...sourceExts, "css", "scss", "sass"],
    },
  };
})();
```

---

#### For React Native v0.56 or older

Configure `rn-cli.config.js` to use a custom transformer file and add more extensions:

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("./transformer.js");
  },
  getSourceExts() {
    return ["js", "jsx", "css", "scss", "sass"];
  },
};
```

---

#### For Expo SDK v30.0.0 or older

Configure `app.json` to use a custom transformer file and add more extensions:

```json
{
  "expo": {
    "packagerOpts": {
      "sourceExts": ["js", "jsx", "css", "scss", "sass"],
      "transformer": "./transformer.js"
    }
  }
}
```

### Step 4: configure transformer

Create a `transformer.js` file:

```js
// For React Native version 0.59 or later
var upstreamTransformer = require("metro-react-native-babel-transformer");

// For React Native version 0.56-0.58
// var upstreamTransformer = require("metro/src/reactNativeTransformer");

// For React Native version 0.52-0.55
// var upstreamTransformer = require("metro/src/transformer");

// For React Native version 0.47-0.51
// var upstreamTransformer = require("metro-bundler/src/transformer");

// For React Native version 0.46
// var upstreamTransformer = require("metro-bundler/build/transformer");

var sassTransformer = require("react-native-sass-transformer");
var cssTransformer = require("react-native-css-transformer");

module.exports.transform = function ({ src, filename, options }) {
  if (filename.endsWith(".scss") || filename.endsWith(".sass")) {
    return sassTransformer.transform({ src, filename, options });
  } else if (filename.endsWith(".css")) {
    return cssTransformer.transform({ src, filename, options });
  }
  return upstreamTransformer.transform({ src, filename, options });
};
```
