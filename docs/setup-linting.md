## Setup recommended linting (ESLint & stylelint)

Setup [ESLint](https://eslint.org) and [stylelint](https://stylelint.io/) in your project to avoid common problems and to keep CSS consistent.

- ESLint is used to warn for unused CSS.
- stylelint is used to make sure that the CSS is compatible with both Web and React Native, and to warn for duplicate properties, vendor prefixes, incompatible units, etc.

### Step 1: Install ESLint, stylelint and plugins

```sh
yarn add eslint eslint-plugin-css-modules stylelint stylelint-react-native stylelint-config-react-native-css-modules --dev
```

### Step 2: Add configs

Add these configs to your project's `package.json` (or use `.stylelintrc` and `.eslintrc` files).

**ESlint:**

```json
"eslintConfig": {
  "parserOptions": {
    "sourceType": "module",
    "ecmaVersion": 2017,
    "ecmaFeatures": {
      "jsx": true
    }
  },
  "plugins": [
    "css-modules"
  ],
  "extends": [
    "plugin:css-modules/recommended"
  ]
},
```

**stylelint:**

```json
"stylelint": {
  "extends": "stylelint-config-react-native-css-modules",
  "rules": {
    "declaration-block-no-duplicate-properties": true,
    "no-duplicate-selectors": true,
    "no-extra-semicolons": true,
    "no-eol-whitespace": true,
    "no-missing-end-of-source-newline": true
  }
}
```

### Step 3: Add npm script to run linters

In your project's `package.json`, add a new command called `lint` to `scripts`, so that it looks like this:

```json
"scripts": {
  "lint": "eslint . && stylelint '**/*.@(css|scss|sass|less|styl)'"
}
```

_You can remove file extensions that you don't use from the stylelint command._

### Step 4: Run linters

Run `yarn lint` in a terminal window to see if there are any linting errors or warnings.
