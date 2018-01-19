### Using multiple transformers (e.g. Typescript + CSS modules)

You might already be using a Typescript transformer in your project, so you need to create extra config in order to use it together with CSS or Sass.

Here's an example of using CSS, Sass and Typescript:

`rn-cli.config.js`

```js
module.exports = {
  getTransformModulePath() {
    return require.resolve("./transformer.js");
  },
  getSourceExts() {
    return ["ts", "tsx", "scss", "sass", "css"];
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
var typescriptTransformer = require("react-native-typescript-transformer");

module.exports.transform = function({ src, filename, options }) {
  if (filename.endsWith(".scss") || filename.endsWith(".sass")) {
    return sassTransformer.transform({ src, filename, options });
  } else if (filename.endsWith(".css")) {
    return cssTransformer.transform({ src, filename, options });
  } else if (filename.endsWith(".ts") || filename.endsWith(".tsx")) {
    return typescriptTransformer.transform({ src, filename, options });
  } else {
    return upstreamTransformer.transform({ src, filename, options });
  }
};
```
