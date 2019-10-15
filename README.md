<p align="center">
<a href="http://redux-resource.js.org">
<img src="https://user-images.githubusercontent.com/2322305/35489731-63e3bb20-044e-11e8-8211-b7d153722865.png" height="120" alt="Redux Resource Logo" aria-label="redux-resource.js.org" />
</a>
</p>

<p align="center">
  <a href="https://travis-ci.org/jamesplease/redux-resource">
    <img src="http://img.shields.io/travis/jamesplease/redux-resource.svg?style=flat" alt="Redux Resource Travis Builds" />
  </a>
  <a href="https://www.npmjs.com/package/redux-resource">
    <img src="https://img.shields.io/npm/v/redux-resource.svg" alt="Redux Resource NPM Package" />
  </a>
  <a href="https://www.npmjs.com/package/redux-resource">
    <img src="https://img.shields.io/npm/dm/redux-resource.svg" alt="Redux Resource NPM Download Count" />
  </a>
  <a href="https://coveralls.io/github/jamesplease/redux-resource?branch=master">
    <img src="https://coveralls.io/repos/github/jamesplease/redux-resource/badge.svg?branch=master" alt="Redux Resource Code Coverage" />
  </a>
  <a href="https://unpkg.com/redux-resource/dist/redux-resource.min.js">
    <img src="http://img.badgesize.io/https://unpkg.com/redux-resource/dist/redux-resource.min.js?compression=gzip" alt="Redux Resource gzip Size" />
  </a>
</p>

A tiny but powerful system for managing 'resources': data that is persisted to
remote servers.

✓ Removes nearly all boilerplate code for remotely-stored data  
✓ Incrementally adoptable  
✓ Encourages best practices like [normalized state](http://redux.js.org/docs/recipes/reducers/NormalizingStateShape.html)  
✓ Works well with APIs that adhere to standardized formats, such as JSON API  
✓ Works well with APIs that don't adhere to standardized formats, too  
✓ Integrates well with your favorite technologies: HTTP, gRPC, normalizr, redux-observable, redux-saga, and more  
✓ Microscopic file size (3kb gzipped!)

### Installation

To install the latest version:

```
npm install --save redux-resource
```

### Documentation

View the documentation at
**[redux-resource.js.org ⇗](https://redux-resource.js.org/)**.

> Looking for the v2.4.1 documentation? **[View it here](https://jamesplease.github.io/redux-resource-2.4.1-docs/)**.

> Migration guides to the latest version can be found
> **[here](https://redux-resource.js.org/docs/other-guides/migration-guides.html)**.

### Quick Start

Follow this guide to get a taste of what it's like to work with Redux
Resource.

First, we set up our store with a "resource reducer," which is a reducer that
manages the state for one type of resource. In this guide, our reducer will
handle the data for our "books" resource.

```js
import { createStore, combineReducers } from 'redux';
import { resourceReducer } from 'redux-resource';

const reducer = combineReducers({
  books: resourceReducer('books')
});

const store = createStore(reducer);
```

Once we have a store, we can start dispatching actions to it. In this example,
we initiate a request to read a book with an ID of 24, then follow it up with an
action representing success. There are two actions, because requests usually
occur over a network, and therefore take time to complete.

```js
import { actionTypes } from 'redux-resource';
import store from './store';

// This action represents beginning the request to read a book with ID of 24. This
// could represent the start of an HTTP request, for instance.
store.dispatch({
  type: actionTypes.READ_RESOURCES_PENDING,
  resourceType: 'books',
  resources: [24]
});

// Later, when the request succeeds, we dispatch the success action.
store.dispatch({
  type: actionTypes.READ_RESOURCES_SUCCEEDED,
  resourceType: 'books',
  // The `resources` list here is usually the response from an API call
  resources: [{
    id: 24,
    title: 'My Name is Red',
    releaseYear: 1998,
    author: 'Orhan Pamuk'
  }]
});
```

Later, in your view layer, you can access information about the status of
this request. When it succeeds, accessing the returned book is straightforward.

```js
import { getStatus } from 'redux-resource';
import store from './store';

const state = store.getState();
// The second argument to this method is a path into the state tree. This method
// protects you from needing to check for undefined values.
const readStatus = getStatus(store, 'books.meta[24].readStatus');

if (readStatus.pending) {
  console.log('The request is in flight.');
}

else if (readStatus.failed) {
  console.log('The request failed.');
}

else if (readStatus.succeeded) {
  const book = state.books.resources[24];

  console.log('The book was retrieved successfully, and here is the data:', book);
}
```

This is just a small sample of what it's like working with Redux Resource.

For a real-life webapp example that uses many more CRUD operations, check out
the **[zero-boilerplate-redux webapp ⇗](https://github.com/jamesplease/zero-boilerplate-redux)**.
This example project uses [React](https://facebook.github.io/react/), although
Redux Resource works well with any view layer.

### Repository Structure

This repository is a [Lerna](https://github.com/lerna/lerna) project. That means
it's a single repository that allows us to control the publishing of a number
of packages. The source for each package can be found in the[
  `./packages`](https://github.com/jamesplease/redux-resource/tree/master/packages)
  directory.

| Package | Version | Description |
| ---- | ---- | ---- |
| `redux-resource` | [![npm version](https://img.shields.io/npm/v/redux-resource.svg)](https://www.npmjs.com/package/redux-resource) | The main library |
| `redux-resource-xhr` | [![npm version](https://img.shields.io/npm/v/redux-resource-xhr.svg)](https://www.npmjs.com/package/redux-resource-xhr) | A library that exports a powerful HTTP CRUD action creator |
| `redux-resource-plugins` | [![npm version](https://img.shields.io/npm/v/redux-resource-plugins.svg)](https://www.npmjs.com/package/redux-resource-plugins) | A collection of common plugins |
| `redux-resource-prop-types` | [![npm version](https://img.shields.io/npm/v/redux-resource-prop-types.svg)](https://www.npmjs.com/package/redux-resource-prop-types) | Handy Prop Types to use with Redux Resource |
| `redux-resource-action-creators` | [![npm version](https://img.shields.io/npm/v/redux-resource-action-creators.svg)](https://www.npmjs.com/package/redux-resource-action-creators) | Unopinionated action creators for Redux Resource (bring your own HTTP request) |

### Contributing

Thanks for your interest in helping out! Check out the
[Contributing Guide](./CONTRIBUTING.md), which covers everything you'll need to
 get up and running.

### Contributors

([Emoji key](https://github.com/kentcdodds/all-contributors#emoji-key))

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore -->
| [<img src="https://avatars3.githubusercontent.com/u/2322305?v=4" width="100px;"/><br /><sub><b>James, please</b></sub>](http://www.jmeas.com)<br />[💻](https://github.com/jamesplease/redux-resource/commits?author=jamesplease "Code") [🔌](#plugin-jamesplease "Plugin/utility libraries") [📖](https://github.com/jamesplease/redux-resource/commits?author=jamesplease "Documentation") [🤔](#ideas-jamesplease "Ideas, Planning, & Feedback") | [<img src="https://avatars3.githubusercontent.com/u/682566?v=4" width="100px;"/><br /><sub><b>Stephen Rivas JR</b></sub>](http://www.stephenrivasjr.com)<br />[💻](https://github.com/jamesplease/redux-resource/commits?author=sprjr "Code") [📖](https://github.com/jamesplease/redux-resource/commits?author=sprjr "Documentation") [🤔](#ideas-sprjr "Ideas, Planning, & Feedback") [🔌](#plugin-sprjr "Plugin/utility libraries") | [<img src="https://avatars0.githubusercontent.com/u/4119765?v=4" width="100px;"/><br /><sub><b>Ian Stewart</b></sub>](https://github.com/ianmstew)<br />[🤔](#ideas-ianmstew "Ideas, Planning, & Feedback") | [<img src="https://avatars3.githubusercontent.com/u/181635?v=4" width="100px;"/><br /><sub><b>Tim Branyen</b></sub>](http://tbranyen.com/)<br />[🤔](#ideas-tbranyen "Ideas, Planning, & Feedback") | [<img src="https://avatars1.githubusercontent.com/u/254562?v=4" width="100px;"/><br /><sub><b>Jason Laster</b></sub>](https://github.com/jasonLaster)<br />[🤔](#ideas-jasonLaster "Ideas, Planning, & Feedback") | [<img src="https://avatars2.githubusercontent.com/u/1104846?v=4" width="100px;"/><br /><sub><b>marlonpp</b></sub>](https://github.com/marlonpp)<br />[🤔](#ideas-marlonpp "Ideas, Planning, & Feedback") | [<img src="https://avatars1.githubusercontent.com/u/4296756?v=4" width="100px;"/><br /><sub><b>Javier Porrero</b></sub>](https://github.com/JPorry)<br />[🤔](#ideas-JPorry "Ideas, Planning, & Feedback") |
| :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [<img src="https://avatars2.githubusercontent.com/u/25591356?v=4" width="100px;"/><br /><sub><b>Smai Fullerton</b></sub>](https://github.com/smaifullerton-wk)<br />[📖](https://github.com/jamesplease/redux-resource/commits?author=smaifullerton-wk "Documentation") | [<img src="https://avatars3.githubusercontent.com/u/276971?v=4" width="100px;"/><br /><sub><b>vinodkl</b></sub>](https://github.com/vinodkl)<br />[🤔](#ideas-vinodkl "Ideas, Planning, & Feedback") | [<img src="https://avatars3.githubusercontent.com/u/828125?v=4" width="100px;"/><br /><sub><b>Eric Valadas</b></sub>](https://github.com/ericvaladas)<br />[📖](https://github.com/jamesplease/redux-resource/commits?author=ericvaladas "Documentation") | [<img src="https://avatars0.githubusercontent.com/u/195580?v=4" width="100px;"/><br /><sub><b>Jeremy Fairbank</b></sub>](http://blog.jeremyfairbank.com)<br />[🚇](#infra-jfairbank "Infrastructure (Hosting, Build-Tools, etc)") | [<img src="https://avatars1.githubusercontent.com/u/4226956?v=4" width="100px;"/><br /><sub><b>Yihang Ho</b></sub>](https://www.yihangho.com)<br />[💻](https://github.com/jamesplease/redux-resource/commits?author=yihangho "Code") | [<img src="https://avatars2.githubusercontent.com/u/1026002?v=4" width="100px;"/><br /><sub><b>Bryce Reynolds</b></sub>](https://github.com/brycereynolds)<br />[💡](#example-brycereynolds "Examples") | [<img src="https://avatars1.githubusercontent.com/u/5614134?v=4" width="100px;"/><br /><sub><b>Ben Creasy</b></sub>](http://bencreasy.com)<br />[📖](https://github.com/jamesplease/redux-resource/commits?author=jcrben "Documentation") |
| [<img src="https://avatars3.githubusercontent.com/u/3513444?v=4" width="100px;"/><br /><sub><b>Guillaume Jasmin</b></sub>](http://www.guillaume-jasmin.fr)<br />[💻](https://github.com/jamesplease/redux-resource/commits?author=GuillaumeJasmin "Code") [🔌](#plugin-GuillaumeJasmin "Plugin/utility libraries") |
<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/kentcdodds/all-contributors)
specification. Contributions of any kind are welcome!
