# Model files: config and hooks

## Model config

The `album.json` file is generated based on the answers we gave to the CLI in the previous step. One impressive feature of LoopBack from a new user's perspective, is that not only is the command line generator easy and quick to use, but the outputted configuration file (this `album.json` file) is very easy to understand.

`/models/common/album.json`

```json
{
  "name": "album",
  "base": "PersistedModel",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {
    "album": {
      "type": "string",
      "required": true
    },
    "year": {
      "type": "number"
    },
    "record-label": {
      "type": "string"
    },
    "artwork": {
      "type": "string"
    },
    "tracks": {
      "type": [
        "object"
      ]
    }
  },
  "validations": [],
  "relations": {},
  "acls": [],
  "methods": {}
}
```

First thing to notice is that this file resides in the `models/common` directory. LoopBack will expose this model to both the server and the client. If we take a look at this file though, it is very readable, no?

**Meta data and options:** These first few lines contain the name, our base model, whether id injection should be used (based on the type of data-source) and that we want to validate our model on upsert.

**Properties:** The `properties` block is what was generated when we created our model based on the CLI prompts. If we decide later that we want a property to be required and a default value provided, we can just edit this file. It is very easy to get into the configuration and make our needed changes.

**Additional options:** These final object keys have empty values. We can tell by their names what they may pertain to but we'll explore them later in the workshop.

## Model hooks

`/models/common/product.js`

```javascript
'use strict';

module.exports = function(Album) {

};
```

The generator also creates a corresponding JavaScript file to allow you to add remote methods to your endpoint. As you can see above, we have a simple function waiting to add in functionality.

Below is an example of a remote method from the LoopBack documentation that adds an endpoint called `status` that returns whether the coffee shop is open or closed based on the time.

```javascript
module.exports = function(CoffeeShop) {
  CoffeeShop.status = function(cb) {
    const currentDate = new Date();
    const currentHour = currentDate.getHours();
    const response = (currentHour > 6 && currentHour < 20) ?
      'We are open for business.' :
      'Sorry, we are closed. Open daily from 6am to 8pm.'

    cb(null, response);
  };
  CoffeeShop.remoteMethod('status', {
      http: {path: '/status', verb: 'get'},
      returns: {arg: 'status', type: 'string'}
    }
  );
};
```

And below is an example of a `beforeRemote` method that adds a timestamp to a Review object before the create request is fulfilled.

```javascript
module.exports = function(Review) {
  Review.beforeRemote('create', function(context, user, next) {
    context.args.data.date = Date.now();
    context.args.data.publisherId = context.req.accessToken.userId;
    next();
  });
};
```

As we can see, this file allows us to hook into our application to do business logic or manipulate our data according to the needs of our application. Now let's see view our newly created `album` API in our explorer.

**Next Step:** [API Explorer](04-api-explorer.md)

