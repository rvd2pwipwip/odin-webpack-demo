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
// webpack.config.js
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
1. Two packages are needed to handle CSS:
```
npm install --save-dev style-loader css-loader
```
- `css-loader` will read any CSS files imported in a JavaScript file and store the result in a string.
- `style-loader` then takes that string and actually adds the JavaScript code that will apply those styles to the page.
2. In `webpack.config.js`, add these loaders so Webpack knows what to do. Since these aren’t plugins, they go in a separate section:
```
// webpack.config.js
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
  module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```
You can now import your CSS file into one of your JavaScript files. `src/script.js` makes sense. We don’t need anything from the imported CSS file itself. Since our CSS and style loaders will handle all of that for us, we can just use a side effect import.

```
// script.js
import "./styles.css";
import { greeting } from "./greeting.js";

console.log(greeting);
```
⚠️ **Don't put a link tag to the css in the index.html file!**
Work with multiple smaller CSS files that are imported in the modules they’re needed.

## Loading images
1. **Image files used in our CSS inside** `url()`

`css-loader` already handles this so there’s nothing extra to do for image paths in CSS.

2. **Image files referenced in the HTML template, e.g. as the** `src` **of an** `<img>`

Install `html-loader`, which will detect image file paths in the HTML template and load the right image files. Without this, `./image.png` would just be a bit of text that no longer references the correct file.
```
npm install --save-dev html-loader
```
Then, add the following object to the `modules.rules` array within `webpack.config.js`:
```
{
  test: /\.html$/i,
  loader: "html-loader",
}
```
3. **Images in JavaScript, where files need to be imported**

Since images aren’t JavaScript, we need to tell Webpack that these files will be assets by adding an `asset/resource` rule. No need to install anything here. Just add the following object to the `modules.rules` array within `webpack.config.js`:
```
{
  test: /\.(png|svg|jpg|jpeg|gif)$/i,
  type: "asset/resource",
}
```
Then, import the image in whatever JavaScript module where the image is used.
```
import imageTest from "./imageTest.png";
   
const image = document.createElement("img");
image.src = imageTest;
   
document.body.appendChild(image);
```

## Webpack dev server
```
npm install --save-dev webpack-dev-server
```
It works by bundling your code behind the scenes (as if we ran npx webpack, but without saving the files to dist), and it does this every time you save a file that’s used in the bundle. 

We can also use something called a source map (`eval-source-map` as a `devtool` option) so that any error messages reference files and lines from our development code and not the jumbled mess inside our single bundled .js file!

Once installed, in our `webpack.config.js`, we only need to add a couple more properties somewhere in the configuration object (the order of these properties does not matter):
```
// webpack.config.js
...
  output: {
    filename: "main.js",
    path: path.resolve(__dirname, "dist"),
    clean: true,
  },
  devtool: "eval-source-map",
  devServer: {
    watchFiles: ["./src/index.html"],
  },
  plugins: [
...
```
`webpack-dev-server` will only auto-restart when it detects any changes to files we import into our JavaScript bundle, so our HTML template will be ignored! All we need to do is add it to the dev server’s array of watched files.

Once set up, `npx webpack` serve will host our web page on `http://localhost:8080/` with:
```
npx webpack serve
```
> **Note:** If you change the webpack config file while the dev server is running, it will not reflect those config changes. Use `Ctrl + C` in the terminal to kill it then rerun `npx webpack serve` to apply the new config.