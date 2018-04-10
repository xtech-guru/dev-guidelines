## Installation

To add bootstrap with `scss` support to a `React` project:

1- Add bootstrap, reactstrap(optional), node-sass compiler:

```bash
yarn add bootstrap reactstrap@5.0.0-beta node-sass-chokidar
```

2- In `package.json` file we must add those two scripts:

```diff
"scripts": {
+  "build-css": "node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/",
+  "watch-css": "npm run build-css && node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/ --watch --recursive",
  "start": "react-scripts start",
  "build": "react-scripts build",
```

3- To run `"watch-css"` script wiht `yarn start`:

```bash
yarn add npm-run-all
```

then

```diff
"scripts": {
  "build-css": "node-sass-chokidar src/ -o src/",
  "watch-css": "npm run build-css && node-sass-chokidar src/ -o src/ --watch --recursive",
-    "start": "react-scripts start",
-    "build": "react-scripts build",
+    "start-js": "react-scripts start",
+    "start": "npm-run-all -p watch-css start-js",
+    "build-js": "react-scripts build",
+    "build": "npm-run-all build-css build-js",
  "test": "react-scripts test --env=jsdom",
  "eject": "react-scripts eject"
}
```

the final version of the scripts section in `package.json` will be:

```json
"scripts": {
    "build-css": "node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/",
    "watch-css":
      "npm run build-css && node-sass-chokidar --include-path ./src --include-path ./node_modules src/ -o src/ --watch --recursive",
    "start-js": "react-scripts start",
    "start": "npm-run-all -p watch-css start-js",
    "build-js": "react-scripts build",
    "build": "npm-run-all build-css build-js",
    // the rest of the scripts after...
}
```

[further reading on adding sass/scss to a project bootstrapped with create-react-app](https://github.com/facebook/create-react-app/blob/master/packages/react-scripts/template/README.md#adding-a-css-preprocessor-sass-less-etc)

4- rename `index.css` (and all css files) to `index.scss`

5- import bootstrap scss in `index.scss`:

```scss
@import 'bootstrap/scss/bootstrap';
```

## Best Practices (todo)
