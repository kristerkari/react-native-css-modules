## Limitations

First of all, React Native has a very limited support for styling. There are some styling properties that do not exist in regular CSS and some properties work differently than in CSS, so don't treat it as something that is 100% compatible with regular CSS.

The supported styling depends on the element that you want to style. The best thing is to have a look at this cheat sheet when ever your are writing your styles: https://github.com/vhpoet/react-native-styling-cheat-sheet

If you plan to use the same CSS files for both React Native and Web, then I suggest that you build your app "React Native first". It is much easier to build the app with React Native's styling limitations, and then make it work for web using [React Native for Web](https://github.com/necolas/react-native-web) and by adding some Web specific CSS.

### Current limitations for CSS modules in React Native

Many of these will be fixed in the near future.

* No hot loading for Sass files that are imported with `@import` yet.
* No `:global` or `:local` keywords for CSS modules with React Native.
* No support for using [classnames](https://github.com/JedWatson/classnames) module for multiple classnames (`classnames` outputs classnames as a string).
* No way to pass options to Sass (`node-sass`) yet (planned).
