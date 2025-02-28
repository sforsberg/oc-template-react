oc-template-react
=================

react-templates & utilities for the [OpenComponents](https://github.com/opentable/oc) template system

***

Module for handling React templates in OC

[![codecov](https://codecov.io/gh/opencomponents/oc-template-react/branch/master/graph/badge.svg)](https://codecov.io/gh/opencomponents/oc-template-react)
[![Known Vulnerabilities](https://snyk.io/test/github/opencomponents/oc-template-react/badge.svg)](https://snyk.io/test/github/opencomponents/oc-template-react)
[![npm version](https://badge.fury.io/js/oc-template-react.svg)](http://badge.fury.io/js/oc-template-react)

| Node8             | Node9             | Node10            | 
|-------------------|-------------------|-------------------|
| [![Node8][1]][4]  | [![Node9][2]][4]  | [![Node10][3]][4] |

[1]: https://travis-matrix-badges.herokuapp.com/repos/opencomponents/oc-template-react/branches/master/1
[2]: https://travis-matrix-badges.herokuapp.com/repos/opencomponents/oc-template-react/branches/master/2
[3]: https://travis-matrix-badges.herokuapp.com/repos/opencomponents/oc-template-react/branches/master/3
[4]: https://travis-ci.org/opencomponents/oc-template-react

## Packages included in this repo

| Name | Version |
|--------|-------|
| [`oc-template-react`](/packages/oc-template-react) | [![npm version](https://badge.fury.io/js/oc-template-react.svg)](http://badge.fury.io/js/oc-template-react) |
| [`oc-template-react-compiler`](/packages/oc-template-react-compiler) | [![npm version](https://badge.fury.io/js/oc-template-react-compiler.svg)](http://badge.fury.io/js/oc-template-react-compiler) |
| [`oc-react-component-wrapper`](/packages/oc-react-component-wrapper) | [![npm version](https://badge.fury.io/js/oc-react-component-wrapper.svg)](http://badge.fury.io/js/oc-react-component-wrapper) |


## Usage:

Initialize a component with the oc-template-react and follow the CLI instructions

```
$ oc init <your-component-name> oc-template-react
```

## Extra info:
### package.json requirements
- `template.src` - the react App entry point -  should export a react component as `default`
- `template.type` -  `oc-template-react`
- required in `devDependencies` -  `oc-template-react-compiler`
### package.json optional
- `template.externals` - override unpkg.com externals 
```
  "externals": [
    {
      "name": "Object.assign",
      "global": [
        "Object",
        "assign"
      ],
      "url": "https://mydomain.com/es6-object-assign@1.1.0/dist/object-assign-auto.min.js"
    },
    {
      "name": "prop-types",
      "global": "PropTypes",
      "url": "https://mydomain.com/prop-types@15.7.2/prop-types.min.js"
    },
    {
      "name": "react",
      "global": "React",
      "url": "https://mydomain.com/react@16.9.0/umd/react.production.min.js"
    },
    {
      "name": "react-dom",
      "global": "ReactDOM",
      "url": "https://mydomain.com/react-dom@16.9.0/umd/react-dom.production.min.js"
    }
  ]
```

### conventions
- `props = server()` - the viewModel generated by the server is automatically passed to the react application as props
- The oc-client-browser is extended and will now expose all the available react-component at `oc.reactComponents[bundleHash]`
- You can register an event handler within the [oc.events](https://github.com/opentable/oc/wiki/Browser-client#oceventsoneventname-callback) system for the the `oc:componentDidMount` event. The event will be fired immediately after the react app is mounted.
- You can register an event handler within the [oc.events](https://github.com/opentable/oc/wiki/Browser-client#oceventsoneventname-callback) system for the the `oc:cssDidMount` event. The event will be fired immediately after the style tag will be added to the active DOM tree.

### externals
Externals are not bundled when packaging and publishing the component, for better size taking advantage of externals caching. OC will make sure to provide them at Server-Side & Client-Side rendering time for you.
- React
- ReactDOM
- PropTypes
 
### features
- `Server Side Rendering` = server side rendering should work as for any other OpenComponent
- `css-modules` are supported.
- `post-css` is supported with the following plugins:
  - [postcss-import](https://github.com/postcss/postcss-import)
  - [postcss-extend](https://github.com/travco/postcss-extend)
  - [postcss-icss-values](https://github.com/css-modules/postcss-icss-values)
  - [autoprefixer](https://github.com/postcss/autoprefixer)
- `White list dependencies` to be inlcuded in the build process done by the compiler. To whitelist dependencies installed in the node_modules folder, add in the package.json of the component a `buildIncludes` list:
  ```json
    ...
    oc : {
      files: {
        template: {
          ...
          buildIncludes: ['react-components-to-build']
        }
      }
    }
  ```



## Utils

The compiler exposes some utilities that could be used by your React application:

### HOCs

#### withDataProvider

An Higher order component that will make a `getData` function available via props.

##### Usage:

```javascript
import { withDataProvider } from 'oc-template-react-compiler/utils';

const yourApp = props => {
  // you can use props.getData here
};

yourEnhancedApp = withDataProvider(yourApp);
```

`getData` accept two arguments: `(params, callback) => callback(err, result)`. It will perform a post back request to the component endpoint with the specified query perams invoking the callback with the results.

For more details, check the [`example app`](/acceptance-components/react-app/app.js)

#### withSettingProvider

An Higher order component that will make a `getSetting` function available via props.

##### Usage:

```javascript
import { withSettingProvider } from 'oc-template-react-compiler/utils';

const yourApp = props => {
  // you can use props.getSetting here
};

yourEnhancedApp = withSettingProvider(yourApp);
```

`getSetting` accept one argument: `settingName => settingValue`. It will return the value for the requested setting.

Settings available at the moment:
- `getSetting('name')` : return the name of the OC component
- `getSetting('version')` : return the version of the OC component
- `getSetting('baseUrl')` : return the [baseUrl of the oc-registry](https://github.com/opentable/oc/wiki/The-server.js#context-properties)
- `getSetting('staticPath')` : return the path to the [static assets](https://github.com/opentable/oc/wiki/The-server.js#add-static-resource-to-the-component) of the OC component
