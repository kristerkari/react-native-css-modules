## Setup CSS variables support

_! Please make sure that you are using the latest version of Sass, LESS, or Stylus transformer before doing the setup. !_

Using [postcss-css-variables](https://github.com/MadLittleMods/postcss-css-variables#readme) you can use most of CSS variables features, including selector cascading with some caveats, because this can only see the CSS, not the potentially dynamic HTML and DOM the CSS is applied to.

If you are already using `react-native-css-transformer`, then need to switch to use `react-native-postcss-transformer` (please refer to the setup documentation) and add a PostCSS config with [postcss-css-variables](https://github.com/MadLittleMods/postcss-css-variables#readme) plugin.

### For Typescript users

If you are using the typed transformers (e.g. generates `mystyles.d.scss` typings for Sass files), then you can use the normal transformers for Sass/Less/Stylus and a typed transformer for PostCSS. This is because with CSS variables you are using two transformers together and only one of the transformers needs to create the types file.

For example when using Sass: `react-native-sass-transformer` + `react-native-typed-postcss-transformer`.

### Step 1: Setup React Native CSS modules

- [Setup React Native CSS modules with CSS support](setup-css.md)
- [Setup React Native CSS modules with PostCSS support](setup-postcss.md)
- [Setup React Native CSS modules with Sass support](setup-sass.md)
- [Setup React Native CSS modules with Less support](setup-less.md)
- [Setup React Native CSS modules with Stylus support](setup-stylus.md)

### Step 2: Install PostCSS dependencies

Install [PostCSS](https://postcss.org/), [postcss-css-variables](https://github.com/MadLittleMods/postcss-css-variables#readme) plugin, and [react-native-postcss-transformer](https://github.com/kristerkari/react-native-postcss-transformer).

```sh
yarn add postcss postcss-css-variables react-native-postcss-transformer --dev
```

### Step 3: Configure PostCSS

Add `postcss-css-variables` to your PostCSS configuration with [one of the supported config formats](https://github.com/michael-ciniawsky/postcss-load-config), e.g. `package.json`, `.postcssrc`, `postcss.config.js`, etc.

### Step 4: Setup `transformer.js`

_This example is for Sass, if you are using Less or Stylus, switch `react-native-sass-transformer` to `react-native-less-transformer`/`react-native-stylus-transformer` and change the file extensions `.scss` and `.sass` to `.less`/`.styl`._

To make CSS variables work, we first need to render Sass to CSS, and then pass that to PostCSS.

Create a `transformer.js` file to your project's root and do the following:

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
var postCSSTransformer = require("react-native-postcss-transformer");

module.exports.transform = function({ src, filename, options }) {
  if (filename.endsWith(".scss") || filename.endsWith(".sass")) {
    return sassTransformer
      .renderToCSS({ src, filename, options })
      .then(css =>
        postCSSTransformer.transform({ src: css, filename, options })
      );
  } else {
    return upstreamTransformer.transform({ src, filename, options });
  }
};
```

### Step 5: Setup Metro config

In `metro.config.js` point the `babelTransformerPath` to `transformer.js` file:

```diff
-babelTransformerPath: require.resolve("react-native-sass-transformer")
+babelTransformerPath: require.resolve("./transformer.js")
```
