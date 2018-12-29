## Setup browser compatibility for a project that uses React Native CSS modules

What you need is:

- [Webpack](https://webpack.js.org/) (or some other bundler)
- [React Native for Web](https://github.com/necolas/react-native-web)

### Step 1: Setup React Native CSS modules

- [Setup React Native CSS modules with CSS support](setup-css.md)
- [Setup React Native CSS modules with PostCSS support](setup-postcss.md)
- [Setup React Native CSS modules with Sass support](setup-sass.md)
- [Setup React Native CSS modules with Less support](setup-less.md)
- [Setup React Native CSS modules with Stylus support](setup-stylus.md)

### Step 2: Install Webpack + React Native for Web

#### Install Webpack dependencies

for React Native v0.56 or newer (uses Babel 7):

```sh
yarn add babel-loader babel-core@7.0.0-bridge.0 @babel/preset-env babel-preset-react@7.0.0-beta.3 webpack webpack-cli css-loader react-hot-loader style-loader webpack-dev-server --dev
```

for React Native v0.55 or older (uses Babel 6):

```sh
yarn add babel-loader babel-core babel-preset-env babel-preset-react webpack webpack-cli css-loader react-hot-loader style-loader webpack-dev-server --dev
```

#### Install React Native for Web dependencies

```sh
yarn add react-art react-dom react-native-web --save
```

### Step 3: Add Webpack config

In your project's root folder, add a file called `webpack.config.js`:

**For React Native v0.56:**

Use the config below, but change `babel-loader`'s presets to:

```js
presets: ["@babel/preset-env", "react", "react-native"];
```

**For React Native v0.55 or older:**

Use the config below, but change `babel-loader`'s presets to:

```js
presets: ["babel-preset-env", "react", "react-native"];
```

**For React Native v0.57 or newer:**

```js
const webpack = require("webpack");

module.exports = {
  entry: ["react-hot-loader/patch", "./index.web.js"],
  devServer: {
    hot: true
  },
  plugins: [
    new webpack.NamedModulesPlugin(),
    new webpack.HotModuleReplacementPlugin()
  ],
  module: {
    rules: [
      {
        test: /\.jsx?$/,
        exclude: /node_modules/,
        loader: "babel-loader",
        query: {
          babelrc: false,
          presets: [
            "@babel/preset-env",
            "react",
            "module:metro-react-native-babel-preset"
          ],
          plugins: ["react-hot-loader/babel"]
        }
      },
      {
        test: /\.css$/,
        use: [
          {
            loader: "style-loader"
          },
          {
            loader: "css-loader",
            options: {
              modules: true,
              localIdentName: "[path]___[name]__[local]___[hash:base64:5]"
            }
          }
        ]
      }
    ]
  },
  resolve: {
    alias: {
      "react-native": "react-native-web"
    },
    extensions: [".web.js", ".js", ".web.jsx", ".jsx"],
    mainFields: ["browser", "main"]
  }
};
```

If you need support for Sass or other CSS pre/post processors, have a look at the documentation for a specific webpack loader, e.g. https://github.com/webpack-contrib/sass-loader

### Step 4: Add index.html and index.web.js

In your project's root folder, add a file called `index.web.js`:

```js
import { AppRegistry } from "react-native";
import App from "./App";

AppRegistry.registerComponent("AwesomeProject", () => App);
AppRegistry.runApplication("AwesomeProject", {
  rootTag: document.getElementById("react-app")
});

if (module.hot) {
  module.hot.accept();
}
```

Also add a file called `index.html` to your project's root folder:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>AwesomeProject</title>
    <meta charset="utf-8" />
    <meta content="initial-scale=1,width=device-width" name="viewport" />
  </head>

  <body>
    <div id="react-app"></div>
    <script type="text/javascript" src="/main.js"></script>
  </body>
</html>
```

### Step 5: Add npm script to start Webpack development server

In your project's `package.json`, add a new command called `web` to `scripts`, so that it looks like this:

```json
"scripts": {
  "web": "NODE_ENV=development webpack-dev-server --mode development"
}
```

### Step 6: Start Webpack development server and open the project in a browser

Run `yarn web` in a terminal window and if there are no warnings, open `http://localhost:8080` in a web browser to see your React Native project running in a browser environment.
