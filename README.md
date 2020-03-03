# cra-in-express-boilerplate
A boilerplate for CRA (Create React App) in Express project.

## Table of Contents

1. [Setup](#setup)
    1. [Setup server](#setup-server)
    1. [Setup client](#setup-client)
1. [Build](#build)
    1. [Build client](#build-client)

<a name="setup"></a>
## Setup

This parts tells you how this **master** branch comes. And you can skip this section if you want to try by yourself.

* [Guide to setup server, with express.js](https://expressjs.com/en/starter/generator.html)
* Guide to setup client, with CRA](https://create-react-app.dev/docs/deployment)

<a name="setup-server"></a>
### Setup server

Create backend with `handlebars` and `sass` supported as example.

```bash
$ yarn global add express-generator
$ express --view hbs --css sass --git myapp
$ cd myapp/
$ git init && git add . && git commit -m 'initial commit'
```

Run server

```
$ yarn install
$ DEBUG=myapp:* yarn start
```

Try server: browse http://localhost:3000

Install `nodemon`

```
$  yarn add --dev nodemon
```

Support `nodemon`, by updateing `scripts` section in `package.json`:

```
    "dev": "DEBUG=myapp:* PORT=8080 nodemon ./bin/www",
```

Try nodemon: browse http://localhost:8080, modify some source code, and refresh the browser.

Commit changes

```
$ git add package.json yarn.lock
$ git commit -m 'support nodemon'
```

> Mention the [best practices performance](https://expressjs.com/en/advanced/best-practice-performance.html)!!!

<a name="setup-client"></a>
### Setup client

Create client with CRA (Create React App) scaffold

```
$ yarn global add create-react-app
$ yarn create react-app client
```

Proxy **client** `yarn` command from **server**, with `package.json` in **server** `"scripts"` section.

```
    "client": "cd ./client; yarn",
```

Try **client** commands with `yarn` proxy

```
$ yarn client start
```

> `yarn client start` equals to `(cd client; yarn start)`

Render client assets from server

```js
app.use(express.static(path.join(__dirname, 'public')));
// Step 1: put here loading client/build as server static folder as well.
app.use(express.static(path.join(__dirname, 'client/build')));

app.use('/', indexRouter);  // This router will not work, because of index.html in assets
app.use('/users', usersRouter);

// Step 2: Put after other server routes, we will move this when we need later.
app.get('/*', function(req, res) {
  res.sendFile(path.join(__dirname, 'client/build', 'index.html'));
});
```

Commit changes

```
$ git add package.json
$ git commit -m 'support server side render client assets'
```

Try result:

* browse http://localhost:3000, can see the CRA app by dev server on `3000`.

<a name="build"></a>
## Build

<a name="build-client"></a>
### Build client

```
$ yarn client build
```

Run server side in `production` mode to see builds of client assets

```
PORT=7000 NODE_ENV=production yarn start
```

Try result:

* browse http://localhost:7000, can see the express server on `7000`, uses built client assets btw.
