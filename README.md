# loopback-remote-routing
[![Circle CI](https://circleci.com/gh/Neil-UWA/loopback-remote-routing/tree/master.svg?style=svg)](https://circleci.com/gh/Neil-UWA/loopback-remote-routing/tree/master)

Easily disable remote methods.

##Features

- selectively disable remote methods created by *relations*, defined by code or in definition
- selectively disable remote methods created by *scopes* , defined by code or in definition
- support loopback-component-storage specific methods
- use as mixin

##Installation

```bash
npm install loopback-remote-routing --save
```

##How to use

###use directly

```js

// common/models/color.js
var RemoteRouting = require('loopback-remote-routing');

module.exports = function(Color) {
  // use only to expose specified remote methods
  // symbol @ denotes static method
  // scope methods are static method
  RemoteRouting(Color, {only: [
      '@find',
      '@findById',
      'updateAttributes'
  ]})

  //use except to expose all remote methods except specified ones
  RemoteRouting(Color, {except: [
      '@create',
      '@find'
  ]}

  //disable all remote methods omitting options

  RemoteRouting(Color)

}

```

### use as a mixin

Add the mixins property to your server/model-config.json like the following:

```json
{
  "_meta": {
    "sources": [
      "loopback/common/models",
      "loopback/server/models",
      "../common/models",
      "./models"
    ],
    "mixins": [
      "loopback/common/mixins",
      "../node_modules/loopback-remote-routing",
      "../common/mixins"
    ]
  }
}

```

To use with your Models add the mixins attribute to the definition object of your model config.

```json
  {
    "name": "Color",
    "properties": {
      "name": {
        "type": "string",
      }
    },
    "mixins": {
      "RemoteRouting" : {
        "only": [
          "@find"
        ]
      }
    }
  }
```

##Options

| option | type | description | required |
| ------ | ---- | ----------- | -------- |
|only| [String] | expose specified remote methods | false |
|except| [String] |  expose all remote methods except specified ones | false |

You can only use options.only or options.except, do not use them together.

If you don't know the relation methods name , you can read the doc https://docs.strongloop.com/display/public/LB/Accessing+related+models](https://docs.strongloop.com/display/public/LB/Accessing+related+models
