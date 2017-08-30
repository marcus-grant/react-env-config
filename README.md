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
> The yarn option `--dev` (npm: `--save-dev`) is there to let the user declare that the dependency is to be saved in `devDependencies` within `package.json`

