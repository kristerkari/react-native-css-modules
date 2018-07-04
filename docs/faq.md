# Frequently Asked Questions

## What is the difference with regular CSS and React Native's CSS?

React Native's styling works a bit differently compared to regular CSS:

- There is no cascade, CSS properties are not inherited from parent elements.
- No complex CSS selectors. There is only support for simple CSS class selector that maps 1-to-1 with an element.
- Many CSS properties are element specific, e.g. you can not give `Text` properties (`font-family`, etc.) to a `View` and vice versa.
- React Native only implements a subset of CSS. The more complex CSS features are left out, and what you get is a set of CSS features that work well to do styling in both browsers and native apps.
- There are some new styling properties in React Native that do not exist in regular CSS.

Even with the above differences, React Native's CSS implementation is still almost fully compatible with the one in Web browsers. You can think of it as a stricter subset of the CSS that is used in browsers.

The supported styling depends on the element that you want to style. You can have a look at the example apps, or this cheat sheet when writing your styles: https://github.com/vhpoet/react-native-styling-cheat-sheet

If you plan to use the same CSS files for both React Native and Web, then I suggest that you build your app "React Native first". It is much easier to build the app with React Native's styling limitations, and then make it work for web using [React Native for Web](https://github.com/necolas/react-native-web).

You can also use [Progressive Enhancement](https://en.wikipedia.org/wiki/Progressive_enhancement) thinking to build the common parts to be cross platform and adding some Web specific CSS styling.

## What limitations does React Native CSS modules have compared to regular CSS modules?

- No `:global` or `:local` keywords for CSS modules with React Native.
- No support for using [classnames](https://github.com/JedWatson/classnames) module to handle multiple classnames (`classnames` outputs classnames as a string).
- No way to pass options to Sass (`node-sass`) yet (planned).
- No hot loading for Sass files that are imported with `@import` yet.

## I added a new CSS file and React Native throws an error for a missing module

You need to restart React Native's packager when you add a new file to your project. This is currently a limitation that will hopefully be fixed in the future.

## How do I use multiple classes for an element?

You can use either the `[styles.class1, styles.class2].join` or the template literal syntax. Have a look at the documentation over here: [babel-plugin-react-native-classname-to-style#multiple-classes](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style#multiple-classes).

You can also use the `styleName` syntax, which supports multiple classnames: [babel-plugin-react-native-stylename-to-style](https://github.com/kristerkari/babel-plugin-react-native-stylename-to-style)

## CSS transitions, animations and gradients are missing

React Native does not currently support CSS transitions, animations or gradients. For animations and transitions you can use React Native's [Animated Javascript module](https://facebook.github.io/react-native/docs/animated.html). For linear gradients you can try to use a library like [react-native-linear-gradient](https://github.com/react-native-community/react-native-linear-gradient).

## Should I use React Native to build Web apps?

Absolutely! React Native's `StyleSheet` module implements a subset of CSS. The more complex CSS features are left out, and what you get is a set of CSS features that work well to do styling in both browsers and native apps.

React Native avoids many of the problems of scaling CSS. There is no CSS property inheritance between elements and no CSS selector specificity issues. You can not give a `<View>` font properties that would get inherited, but you can give them directly to a `<Text>` element.

React Native's elements like `<Text>`, `<View>` and `<Image>` are simpler abstractions of the DOM elements that you use in browser. That means that implementing them in other platforms than React Native or the browser (e.g. React VR) is possible. Implementing the browser DOM in React Native would be way too complex.

Have a look at these talks for more info:

- [How Airbnb Is Using React Native](https://www.youtube.com/watch?v=8qCociUB6aQ)
- [Nicolas Gallagher - Twitter Lite, React Native, and Progressive Web Apps](https://www.youtube.com/watch?v=tFFn39lLO-U)
