## Setup browser compatibility for your project that uses React Native CSS modules

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

Install Webpack dependencies:

```sh
npm install babel-loader babel-core babel-preset-env babel-preset-react webpack webpack-cli css-loader react-hot-loader style-loader webpack-dev-server --save-dev
```

Install React Native for Web dependencies:

```sh
npm install react-art react-dom react-native-web --save
```

### Step 3: Add Webpack config

In your project's root folder, add a file called `webpack.config.js`:

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
        test: /\.js?$/,
        exclude: /node_modules/,
        loader: "babel-loader",
        query: {
          babelrc: false,
          presets: ["babel-preset-env", "react", "react-native"],
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
              localIdentName: "[path][name]__[local]--[hash:base64:5]"
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
    mainFields: ["jsnext:main", "browser", "main"]
  }
};
```

If you need support for Sass or other CSS pre/post processors, have a look at the documentation for a specific webpack loader, e.g. https://github.com/webpack-contrib/sass-loader

### Step 4: Add index files for Web

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

Run `npm run web` in a terminal window and if there are no warnings, open `http://localhost:8080` in a web browser to see your React Native project running in a browser environment.
