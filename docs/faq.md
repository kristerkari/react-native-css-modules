# Frequently Asked Questions

## I added a new CSS file and React Native throws an error for a missing module

You need to restart React Native's packager when you add a new file to your project. This is currently a limitation that will hopefully be fixed in the future.

## How do I use multiple classes for an element?

You can use either the `[styles.class1, styles.class2].join` or the template literal syntax. Have a look at the documentation over here: [babel-plugin-react-native-classname-to-style#multiple-classes](https://github.com/kristerkari/babel-plugin-react-native-classname-to-style#multiple-classes)

## CSS transitions, animations and gradients are missing

React Native does not currently support CSS transitions, animations or gradients. For animations and transitions you can use React Native's [Animated Javascript module](https://facebook.github.io/react-native/docs/animated.html). For linear gradients you can try to use a library like [react-native-linear-gradient](https://github.com/react-native-community/react-native-linear-gradient).

## Should I use React Native to build Web apps?

Absolutely! React Native's `StyleSheet` module implements a subset of CSS. The more complex CSS features are left out, and what you get is a set of CSS features that work well to do styling in both browsers and native apps.

React Native avoids many of the problems of scaling CSS. There is no CSS property inheritance between elements and no CSS selector specificity issues. You can not give a `<View>` font properties that would get inherited, but you can give them directly to a `<Text>` element.

React Native's elements like `<Text>`, `<View>` and `<Image>` are simpler abstractions of the DOM elements that you use in browser. That means that implementing them in other platforms than React Native or the browser (e.g. React VR) is possible. Implementing the browser DOM in React Native would be way too complex.

Have a look at these talks for more info:

- [How Airbnb Is Using React Native](https://www.youtube.com/watch?v=8qCociUB6aQ)
- [Nicolas Gallagher - Twitter Lite, React Native, and Progressive Web Apps](https://www.youtube.com/watch?v=tFFn39lLO-U)
