# odin-webpack-demo
odin class at 
https://www.theodinproject.com/lessons/node-path-javascript-webpack

#Install Webpack

1. Once inside the new project directory, install Webpack, which involves two packages.

```
npm install --save-dev webpack webpack-cli
```

The `--save-dev` flag tells npm to record the two packages as development dependencies (package.json).

2. Create a .gitignore file with `node_modules` in the project root.

3. In the project root, create a webpack.config.js file that contains the following:

```
// webpack.config.js
const path = require("path");

module.exports = {
  mode: "development",
  entry: "./src/script.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
};
```

`mode:` For now, we will just leave this in development mode, as it will be more useful to us. We will revisit this and production mode in a later lesson.
`entry:` A file path from the config file to whichever file is our entry point.
`output:` An object containing information about the output bundle.
- `filename:` The name of the output bundle - can be anything.
- `path:` The path to the output directory, in this case, dist (automatically created).
- `clean:` Set to true, empty the output directory first before bundling the files each time we run Webpack to bundle.