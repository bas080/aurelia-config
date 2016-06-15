# aurelia-config

Configuration plugin that allows you to expose and centralize your configuration.

[![Gitter](https://img.shields.io/gitter/room/nwjs/nw.js.svg?maxAge=2592000?style=plastic)](https://gitter.im/SpoonX/Dev)

This library is an unofficial plugin for the [Aurelia](http://www.aurelia.io/) platform and simplifies plugin configuration by collecting them in a single place.

> To keep up to date on [Aurelia](http://www.aurelia.io/), please visit and subscribe to [the official blog](http://blog.durandal.io/). If you have questions, we invite you to [join us on Gitter](https://gitter.im/aurelia/discuss). If you would like to have deeper insight into our development process, please install the [ZenHub](https://zenhub.io) Chrome Extension and visit any of our repository's boards. You can get an overview of all Aurelia work by visiting [the framework board](https://github.com/aurelia/framework#boards).

## Used By

This library is used by plugins and applications.

## Installation for applications

Installing this module is fairly simple.

Run `jspm i aurelia-config` from your project root or `npm i aurelia-config --save` when using webpack.

## Usage

### Configuring the plugin

You can specify which plugin configs you want to register and add the data you want to get merged into the configs of the plugins.

```js
export function configure(aurelia) {
  aurelia.use
    .standardConfiguration()
    .developmentLogging()
    .plugin('aurelia-config', {
      plugins: [
        // {module: moduleId, config: configObject, alias: 'an-alias'}
        {moduleId: 'aurelia-api', config: {key:'xy'}}, // default alias is ${moduleId}-config
        {moduleId: 'aurelia-authorization', alias: 'authorization-config', config: {data:'xy'}},
        {moduleId: 'global-config', config: {title:'xy'}}
      ],
      registerAlias: true   // allows injection by alias (default: true)
    })
    /* your other plugins */
);

  aurelia.start().then(() => aurelia.setRoot());
}
```

## Installation for plugin developers

There isn't much to do. Just tell the users to add it to the plugin list for aurelia-config and make sure your plugin exports an class or object named 'Config'. The user settings will get merged into that class or object.

### Usage

Well, above might be enough for you. Configs get registered and merged.

To access the merged configs, you still can inject them by class constructor.

```js
import {Config} from 'aurelia-authorization';

@inject(Config)
export class Foo {
  constructor(config) {
    this.config = config;
  }
}
```

Alternatively, you can inject the ConfigManager from aurelia-config and get the Config by alias.

```js
import {ConfigManager} from 'aurelia-config';

@inject(Config)
export class Foo {
  constructor(configManager) {
    this.config = configManager.configs['authorization-config'];
  }
}
```

Or, if `registerAlias: true` (default), you can inject the Config by alias right away.

```js
@inject('authorization-config')
export class Foo {
  constructor(config) {
    this.config = config;
  }
}
```

There also is a global config. Use it like any other Config. It's alias is 'global-config'.

```js
@inject('global-config')
export class Foo {
  constructor(config) {
    this.config = config;
  }
}
```
