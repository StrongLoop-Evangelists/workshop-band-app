# Model files: config and additional logic

## Model config

The CLI created the `event.json` file based on the answers we gave to prompts in the previous step. One impressive feature of LoopBack is that not only is the command line generator easy and quick to use, but the generated [Model definition file](http://loopback.io/doc/en/lb3/Model-definition-JSON-file.html) (this `event.json` file) is very easy to understand.

`/common/models/event.json`

```json
{
  "name": "event",
  "base": "PersistedModel",
  "idInjection": true,
  "options": {
    "validateUpsert": true
  },
  "properties": {
    "location": {
      "type": "string"
    },
    "date": {
      "type": "date"
    },
    "cost": {
      "type": "number"
    },
    "lineup": {
      "type": [
        "string"
      ]
    },
    "poster": {
      "type": "string"
    },
    "url": {
      "type": "string"
    },
    "city": {
      "type": "string"
    },
    "state": {
      "type": "string"
    },
    "description": {
      "type": "string"
    },
    "age-restriction": {
      "type": "string"
    }
  },
  "validations": [],
  "relations": {},
  "acls": [],
  "methods": {}
}
```

If we look at the configuration generated in this file, we see that it is all very readable and understandable.

**Metadata and options:** These first few lines contain the name, our base model, whether id injection should be used (based on the type of data-source) and that we want to validate our model on upsert.

**Properties:** The `properties` block is what was generated when we created our model based on the CLI prompts. If we decide later that we want a property to be required and a default value provided, we can just edit this file. It is very easy to get into the configuration and make our needed changes.

**Additional options:** These final object keys have empty values. We can tell by their names what they may pertain to but we'll explore them later in the workshop.

*Note: This file resides in the `models/common` directory; "common" indicates that LoopBack will expose this model to both the server and the client.*

For complete documentation of the file, see [Model definition file](http://loopback.io/doc/en/lb3/Model-definition-JSON-file.html) in the LoopBack documentation.

## Adding application logic

There are three ways to add custom application logic to models:

- [Remote methods](https://loopback.io/doc/en/lb3/Remote-methods.html) - REST endpoints mapped to Node functions.
- [Remote hooks](https://loopback.io/doc/en/lb3/Remote-hooks.html) - Logic that triggers when a remote method is executed (before or after).
- [Operation hooks](https://loopback.io/doc/en/lb3/Operation-hooks.html) - Logic triggered when a model performs create, read, update, and delete operations against a data source.

For the sake of this workshop, we will simply look at adding a remote method, but if we needed other ways to add application logic, the approach is similar and we can get the details from the links above.

#### Remote Method

In LoopBack, a _[remote method](https://loopback.io/doc/en/lb2/Remote-methods.html)_ is a model method that is exposed over a custom REST endpoint.
The generator creates a corresponding JavaScript file for each model (in this case `event.js`) where we can add remote methods to our endpoint:

`model/common/event.js`

```javascript
'use strict';

module.exports = function(Event) {

};
```

As you can see above, we have a simple function waiting for our additional application logic.

#### Example remote method

Below is [an example of a remote method from the LoopBack documentation](https://loopback.io/doc/en/lb3/Extend-your-API.html#add-a-remote-method) that adds an endpoint called `status` that returns whether the coffee shop is open or closed based on the time.

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

#### Example remote hook

Below is an [example of a `beforeRemote` method that adds a timestamp](https://loopback.io/doc/en/lb3/Extend-your-API.html#add-a-remote-method) to a Review object before the create request is fulfilled.

```javascript
module.exports = function(Review) {
  Review.beforeRemote('create', function(context, user, next) {
    context.args.data.date = Date.now();
    context.args.data.publisherId = context.req.accessToken.userId;
    next();
  });
};
```

As we can see, this file allows us to hook into our application to do business logic or manipulate our data according to the needs of our application. Now let's see view our newly created `event` API in our explorer.

**Next Step:** [API Explorer](04-api-explorer.md)

