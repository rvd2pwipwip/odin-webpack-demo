# odin-webpack-demo
odin class at 
https://www.theodinproject.com/lessons/node-path-javascript-webpack

## Install Webpack

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

4. Run Webpack: 
```
npx webpack
```
Run main.js with `node dist/main.js` to view greeting logged in the terminal.

## Handling HTML
1. Run the following command in the terminal to install HtmlWebpackPlugin (also as a dev dependency):
```
npm install --save-dev html-webpack-plugin
```
⚠️ **Don't put a script tag in the index.html file!**

HtmlWebpackPlugin will automatically add our output bundle as a script tag. 

2. Update webpack.config.js to handle HTML:
```
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");

module.exports = {
  mode: "development",
  entry: "./src/script.js",
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html",
    }),
  ],
};
```
- The `template` option creates an index.html file in the `dist` directory.
- Webpack automatically injects the appropriate deferred script in the html.
- Any changes to HTML generate fresh dist code with a Webpack rerun.

## Loading CSS
Two packages are needed to handle CSS:
```
npm install --save-dev style-loader css-loader
```
- `css-loader` will read any CSS files we import in a JavaScript file and store the result in a string.
- `style-loader` then takes that string and actually adds the JavaScript code that will apply those styles to the page.