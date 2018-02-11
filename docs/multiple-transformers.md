### Using CSS and Sass in the same project

`.babelrc`

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

`rn-cli.config.js`

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("./transformer.js");
  },
  getSourceExts() {
    return ["scss", "sass", "css"];
  },
};
```

`transformer.js`

```js
// For React Native version 0.52 or later
var upstreamTransformer = require("metro/src/transformer");

// For React Native version 0.47-0.51
// var upstreamTransformer = require("metro-bundler/src/transformer");

// For React Native version 0.46
// var upstreamTransformer = require("metro-bundler/build/transformer");

var sassTransformer = require("react-native-sass-transformer");
var cssTransformer = require("react-native-css-transformer");

module.exports.transform = function({ src, filename, options }) {
  if (filename.endsWith(".scss") || filename.endsWith(".sass")) {
    return sassTransformer.transform({ src, filename, options });
  } else if (filename.endsWith(".css")) {
    return cssTransformer.transform({ src, filename, options });
  } else {
    return upstreamTransformer.transform({ src, filename, options });
  }
};
```
