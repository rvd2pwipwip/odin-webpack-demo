# odin-webpack-demo
odin class at 
https://www.theodinproject.com/lessons/node-path-javascript-webpack

1. Once inside your new directory, we can go ahead and install Webpack, which involves two packages.

```
npm install --save-dev webpack webpack-cli
```

The `--save-dev` flag tells npm to record the two packages as development dependencies (package.json).

2. Back in the project root (outside of src), create a webpack.config.js file that contains the following:

```
// webpack.config.js
const path = require("path");

module.exports = {
  mode: "development",
  entry: "./src/index.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
};
```