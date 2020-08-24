# React

Based on [this guide](https://cravencode.com/post/javascript/react-with-parcel-bundler/).

## Initialization

```sh
npm init
```

## Runtime dependencies

```sh
npm install --save react react-dom
```

## Development dependencies

+ @babel/core: transpiling JavaScript
+ @babel/plugin-proposal-class-properties: optional to allow class properties
+ @babel/preset-env: syntax transforms for less advanced browsers
+ @babel/preset-react: transform JSX to JavaScript

```sh
npm install --save-dev \
    parcel-bundler \
    @babel/core \
    @babel/plugin-proposal-class-properties \
    @babel/preset-env \
    @babel/preset-react
```

## Babel configuration

Create a `.babelrc` at the root of the project:

```json
{
  "presets": ["@babel/preset-env", "@babel/preset-react"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

Also configure `browserlist` to specify to `babel` which browsers are supported.
This is done by creating a `.browserslistrc` at the root of the project (sample):

```text
# Browsers that we support
>0.2%
not dead
not ie < 11
not op_mini all
```

## React

### Create the entry point

Create `src/index.html`:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Your title</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="./index.js"></script>
  </body>
</html>
```

Create `src/index.js`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './components/App';
// import './index.css'

ReactDOM.render(<App />, document.getElementById('root'));
```

### Create the first component

Create `src/components/App.js`:

```javascript
import React, { Component } from 'react';
// import './App.css'

class App extends Component {
    render() {
        return <div>This is your App component</div>;
    }
}

export default App;
```

## Development

To run in development mode, just run:

```sh
npx parcel src/index.html
```

## Production

To package the application and get the output in `dist/`, just run:

**Use `set backupcopy=yes` in `Vim` for hot reload!**

```sh
npx parcel build src/index.html
```

## Socket.IO

Install `socket.io`:

```sh
npm install --save socket.io
```

Add minimal server (without `Express.js`) in `backend/server.js`:

```javascript
var io = require('socket.io');
var server = io.listen(3000);

server.on('connection', function(socket) {
  console.log('user connected');
});
```

Test simply by adding the following in `src/components/App.js`:

```javascript
import io from 'socket.io-client';
var socket = io('http://localhost:3000');
```

## Redux

Based on [this guide](https://itnext.io/building-a-react-based-chat-client-with-redux-816b47cb8c74).

Install `Redux` and `Read-Redux`:

```sh
npm install --save redux react-redux
```

### Test Structure

```text
src/
├── components
│   ├── Client
│   │   └── index.js
│   └── UserInput
│       └── index.js
├── index.html
├── index.js
└── store
    ├── index.js
    └── socket
        ├── actions
        │   └── index.js
        ├── middleware
        │   ├── index.js
        │   └── Socket.js
        └── reducer
              └── index.js
```

`Client` is the top component (application) and `UserInput` is just a test component containing a connect/disconnect button and using the `react-redux` connect functions  to access the store's state and actions.

The store is composed of subdirectories (here only `socket`) containing each a set of actions and a reducer
`socket` is a special type that handles the `Socket.IO` connection through a middleware that catches actions relevant to the socket and acts on them before passing the rest on to `Redux`.

## Material UI

Install the following:

```sh
npm install --save @material-ui/core @material-ui/icons typeface-roboto
```
