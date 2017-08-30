# react-env-config
This is a practice run for setting up a useful development environment for React using webpack, yarn, npm, and babel
## References
[1]: https://codeburst.io/its-easy-setting-up-react-and-webpack-eb9ecaef5094
- [codeburst.io on react webpack setup][1]
[2]: https://yarnpkg.com/lang/en/docs/install/
- [Yarn Install Documentation][2]
[3]:

# Getting started
- Node is necessary to make this work, install it as is appropriate for the OS or distribution being worked on
-  Yarn, which replaces some of the npm functionality for package management of Node, makes a lot of this easier and faster
  - Install Yarn by following their installation [documentation][2]
- Create a project folder for the entirety of the next web-development project being worked on
- Initialize using *yarn* or *npm*
```shell
mkdir my-project-root
yarn init
```
*yarn version*

```shell
mkdir my-project-root
npm init
```

- This command generates a `package.json` file that stores all the info about the project and its files/packages/scripts

## Having Webpack Manage Builds & Apps
- In React it's important to engage in the philosophy of atomic responsibilities
  - The idea that each single purpose has its own component, file or function
  - The browser doesn't understand things like `import` or `include` and so something needs to transpile this into regular Javascript
  - Babel gets used to transpile syntactic sugar of JSX files into understandable javascript
  - Webpack helps manage these various components with one toolchain
- Webpack gets every module the app needs, packages them into one bundle file, and can even run a test server to see the results almost immediately
- Install webpack into the local environment using yarn:
```shell
yarn add webpack --dev
# or with npm
npm install webpack --save-dev
```
> The yarn option `--dev` (npm: `--save-dev`) is there to let the user declare that the dependency is to be saved in `devDependencies` within `package.json`, thus persisting the dependency into the project
> Also **Note** that if this is a git repository, node_modules* is a good thing to include in the `.gitignore` file since it's extraneous to include all modules in the repository. Instead use `yarn install` or `npm isntall` to download and install those packages on a freshly cloned folder

- Webpack uses a JS file to read in the current projects configurations, `webpack.config.js`
  - This file is often not automically included on webpack's install, so add it, and for a good boilerplate set of configurations enter the below into it:

```javascript
var path = require('path');

module.exports = {
  entry: './src/index.js',
    output: {
      filename: 'bundle.js'
      path: path.resolve(__dirname, 'dist')
    }
};
```

- Some notes about the above `webpack.config.js` file:
  - `entry` tells webpack to bundle all assets as specified in `index.js` or whatever is specified in its place together
    - The value next to the `entry` key is a string representing relative path to the entry javascript file
  - `output` is where to specify where properties concerning the output of building the javascript project and where to place the results
    - `filename` is just the name of the file, in this case `bundle.js`
    - `path` what directory path relative to the project root that file should be placed, in this case `/project_root/dist`
    - `path.resolve()` is a function used for correctly determining the absolute path on the local machine to store output

### Adding Useful Scripts to Node Server
- The `package.json` file can be given special scripts, that when evoked through the terminal `npm some_fancy_script` executes that script as it is specified in the `package` file
- When using webpack, it's important to include at least one script in this file that runs webpack whenever the node server is started because there is no global command to starting webpack
  - Even if a global command were specified it wouldn't be the most convenient way of making use of webpack
- Add the following to the `package.json` file

```json
{
  "name": "myproject",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "webpack"
  },
  "license": "MIT",
  "devDependencies": {
    "webpack": "^3.4.0"
  }
}
```

- The `"scripts":` key above should have a dictionary in its value that specified key-value pairs of script names *(ie some_fancy_script like above)* that get referred to after the `npm` command
- The value in the `"scripts"` dictionary is command that gets called whenever they key of that entry in the dictionary is invoked
  - even though `webpack` can't be invoked globally from the shell, it should now be installed as a node module, so node will be able to access it by name

### Input & Output Locations
- It's useful to seperate the source code to React applications and the compiled output
  - `src` is one common way to denote *source* files that go into compilation for a web app
    - All JSX, or other related files should then go in there to be compiled
  - `dist` is a common place to store all the compiled output javascript files (*usually only one bundled file*)
  - These directories have already been specified in `webpack.config.js` file so they need to be in place to make sure the right files and directories are being accessed
  - If different names for directories and files are desired, then change the directory/file names as they are declared in `webpack.config.js` and their actual names so that they are **consistent**
```shell
cd path/to/project/root/
mkdir src
mkdir dist
touch src/index.js
```

### Boilerplate Index HTML
- To run a react app, an `index.html` that frames the app will be needed, even when developing so the results can be served by node
- Here's a decent enough and minimal boilerplate version of `index.html` stored in `my_project/dist/index.html`
```html
<html>
  <head>
    <title>My App</title>
  </head>
  <body>
    <!-- a div with a special, user-defined id for react to render within -->
    <div id="app"></div>
    <!--- the bundled script containing all/most js required for the app -->
    <script src="bundle.js"></script>
  </body>
</html>
```
- On top of everything else that the static part of the html should include, must be a div with a unique `id` that identifies the html element that react should render inside
- And sometime after that a `<script>` tag that sources `bundle.js` or whatever else the bundled output script webpack produces should be called

## More Than Just JavaScript
- React usually involves the use of ES6, and JSX
  - ES6 (EcmaScript 6), a standard of javascript that is much more modern, with modern language features
  - JSX is syntactic sugar to enclude HTML elements that get rendered by `React.createElement`
- For example, JSX code:
  - `<div className="sidebar"></div>`
  - compiles to
  - `React.createElement( 'div', {className: 'sidebar'}, null )
- JSX makes life in React **MUCH EASIER** so it's impportant to include in the webpack build environment so it can be used and compiled to something understood by browsers

## Babel
- Babel is how ES6+ and JSX gets compiled into a form of Javascript understood by all modern browsers
- However, *by feault* babel *doesn't* support these modern features as its split into multiple packages:
  - **bbabel-core** - the main package, contains the core javascript parser, utilities and important helper functions
  - **babel-preset-es2015** - the primary module responsible for compiling ES6 into more compatible ES5
  - **babel-preset-react** - transforms JSX into JS
- To make babel work for this development environment, in terminal run:
```shell
yarn add babel-core babel-preset-es2015 --dev
```

- Just installing babel into node modules isn't enough however, babel itself needs configuration to work
- Configure babel by changing the file `.babelrc`
```json
{
  "presets": ["es2015", "react"]
}
```

- Unless custom presets or configurations of babel are needed, such as configuring arrow functions (ES7 features) that should be all babel needs inorder to compile this project
- Webpack requries the use of loaders to transform the source code of modules
- They allow the use of, for example, statements like `import`
- Install loaders using `yarn add babel-loader --dev` in the terminal
- Now webpack needs to be configured to make use of babel, by adding a dictionary within the main dictionary entry keyed as `module` which itself should have the below configurations to run babel:
```javascript
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            }
        ]
    }
};
```
- Any other loader can be addeed to the `rules` property array above
- Here webpack watches all `.js` and `.jsx` files, imported in the app (except those in `node_modules`) and loads them using babel

## Webpack Dev Server
- It's not terribly convenient to to open `index.html` every time a change is made to the React App in development
- To aid in this, `webpack-dev-server` is used to setup a development server that
  - Monitors file changes that are sourced by webpack's `entry` property for changes
  - Recompiles the project on changes
  - Serves up those changes
  - Delivers compiler messages upon receiving errors from the source code
- Install webpack-dev-server by using:
```shell
yarn add webpack-dev-server --dev
```
- Add a script to `package.json` to run the dev server with the command desired to invoke this behavior
```json
...
"scripts": {
  "dev": "webpack-dev-server"
},
...
```
- This instructs node to start webpack with the newly installed dev server option, whenever the command `npm dev` is run
- `devServer: { ... }` inside `webpack.config.js` should be updated to tell the devServer where to find files
- Change `webpack.config.js` like below to add the correct path for the dev server
```javascript
...
devServer: {
  contentBase: path.resolve(--dirname, "dist")
}
...
```
- Now if `yarn dev` is started, and in a browser the page [localhost:8080](http://localhost:8080) is brought up the initial version of the page is presets
  - Make any change to any of the relevant files within the project, and the page will live-reload

## CSS loader
- The core philosophy of Webpack -- **everything is a module**
- CSS and Images can also be modules
- To add the possibliity to import CSS files as modules in the project, two loaders are required
  - **css-loader** -- takes a CSS file and returns the CSS string with resolved `imports` and `url(...)`
  - **style-loader** - takes CSS string from `css-loader` and actually inserts it into the page
```shell
yarn add css-loader style-loader --dev
```
* requried modules to handle CSS as module in webpack *

- Then the final `webpack.config.js` should look something like this:
```javascript
const path = require('path');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: 'bundle.js'
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                loader: 'babel-loader',
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [ 'style-loader', 'css-loader' ]
            }
        ]
    },
    devServer: {
        contentBase: path.resolve(__dirname, "dist")
    }
};
```

## Finally, React
- Finally, installing React makes sense after all this other environmental complexity that will make developing React applications easier as the project goes on
- `yarn add react react-dom`
  - This installs the react and react-dom modules needed to run React in browsers and to have react manipulate the DOM elements of a webpage
- Now the entry file for webpack can be given a starting boilerplate to actually render a react app
```javascript
import React from 'react';
import ReactDOM from 'react-dom';

class Welcome extends React.Component {
  constructor(props) {
    super(props);
    this.handleChange = this.handleChange.bind(this);
    this.state = {name: 'Medium'};
  }
  handleChange(e) {
    this.setState({name: e.target.value});
  }
  render() {
    return (
      <div style={{textAlign: 'center'}}>
        <h1>Welcome</h1>
        <p>Hello {this.state.name}</p>
        <input onChange={this.handleChange} defaultValue={this.state.name}/>
      </div>
    );
  }
}

ReactDOM.render(<Welcome/>, document.getElementById('app'));
```
- Open the page after saving the changes to `index.js` as above, and now suddenly there should be a welcome message rendered from the entry point js file
- Happy **React**ing, now a decent dev environment for this react project has been setup
