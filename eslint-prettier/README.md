# [Eslint](https://eslint.org/)

Eslint is a tool that has the ability to scan the javascript code statically (without the need of running it) and give the developer feedbacks about his code 'health'. Eslint can catch bugs in the code before run time, enforces best practices, and keep the overall code base consistent and following the same style.

In order to achieve that Eslint uses a set of built-in rules, which can also be extended via plugins.

Eslint supports sharing configurations with the use of pre-configured set of rules which can be installed via npm.

## Installation

```bash
yarn add --dev eslint
```

after that run:

```bash
yarn eslint --init
```

this command will prompt you with some question about your usage of eslint and after that it will generate a configuration file in the light of your answers.

In the case of `create-react-app` we don't have to do all of that as eslint is already bundled within it.
`create-react-app` provide us with a
[preconfigured rules](https://github.com/facebook/create-react-app/blob/master/packages/eslint-config-react-app/README.md)
that will lint our code in the compile process handled with [webpack](https://webpack.js.org/) via the [eslint-loader](https://github.com/webpack-contrib/eslint-loader) and will show its result in the terminal and in the console of the browser.

In order to take advantage of eslint right in the editor we can create an `.eslintrc` file in the root of our project:

```bash
touch .eslintrc
```

```json
{
  "extends": ["react-app"]
}
```

And don't forget to install the corresponding plugin/extension/package in your editor of choice:

* [Atom :heart: :heart_eyes: :kiss: :stuck_out_tongue_winking_eye:](https://atom.io/packages/linter-eslint)
* [VS Code :+1: :fire:](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

## Add more rules

We can add more rules to customize our experience and enhance our workflow by installing two other eslint plugins: [`eslint-plugin-react`](https://github.com/yannickcr/eslint-plugin-react) and [`eslint-plugin-import`](https://github.com/benmosher/eslint-plugin-import)

```bash
yarn add --dev eslint-plugin-react eslint-plugin-import
```

You can find the full config file [here](code/.eslintrc)

# [Prettier](https://prettier.io/)

Prettier helps us keep our code consistent in order to save us the pain of manually formatting the code and make the Git history much clean.

## Installation

1- first grab prettier from NPM:

```bash
yarn add --dev prettier
```

2- add a config file in the root of the project with the name of `.prettierrc` with the following content:

```json
{
  "printWidth": 120,
  "singleQuote": true,
  "tabWidth": 2,
  "useTabs": false
}
```

3- In `package.json` file add a script to run prettier:

```diff
"scripts": {
  ...
+   "format": "prettier \"src/**/*.js\" --write"
}
```

4- We can prettier to format our code:

```bash
yarn format
```

## Editors Integrations

* [Atom](https://github.com/prettier/prettier-atom)
* [VS Code](https://github.com/prettier/prettier-vscode)
