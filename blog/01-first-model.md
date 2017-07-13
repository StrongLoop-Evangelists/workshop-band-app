# Let's Build a Band App with LoopBack: creating our first model-driven API endpoint (part 2 of many)

In this series, we are working through building an application to support the needs of DIY bands. We'll start out solving some basic problems and move into more complex ground, eventually transforming the application into a platform that others can use and build upon.

Accompanying this series is a [corresponding workshop](https://github.com/StrongLoop-Evangelists/workshop-band-app) as well as a [code repository](https://github.com/StrongLoop-Evangelists/band-app).

🖥 To see the code for this step, here is the [commit/diff](https://github.com/StrongLoop-Evangelists/band-app/commit/3137aa04dce8caf6589c65032d3cfb8e3e5e1924) in the code repository. This will show you the changes from the last episode to this one.

## Previously on _Let's build a band app!_

In the [previous episode](https://strongloop.com/strongblog/2017-07-06-lets-build-a-band-app-loopback-pt1/), we installed our prerequisites, got an understanding of what LoopBack provides and initialized our LoopBack application.


## In this episode

We'll build out our first API endpoint!

LoopBack takes a model-driven approach to building out APIs. Based on our responses to command line prompts, LoopBack will create a JSON file with our model schema and endpoint details.  For an introduction to LoopBack models see [LoopBack core concepts -  Models](http://loopback.io/doc/en/lb3/LoopBack-core-concepts.html#models) in the LoopBack documentation.

### Let's get into it!

To invoke the generator, we type `lb model` and go through the list of prompts.

_Note: `cd` into your working directory, if you haven't already._

### Enter the name of your model

```
$  band-app git:(master) ✗ lb model
? Enter the model name:
```

We'll call our first model while building our marketplace app `event`.

_Note: models should be singular. The application handles plural naturally, as we'll see in an upcoming step._

### Select the data source

LoopBack generalizes backend services such as databases, REST APIs, SOAP web services, and storage services as data sources.
A _connector_ enables LoopBack applications to use a given data source.  For more information, see [LoopBack core concepts - Data sources and connectors](http://loopback.io/doc/en/lb3/LoopBack-core-concepts.html#data-sources-and-connectors).

```
? Enter the model name: event
? Select the data-source to attach event to: (Use arrow keys)
❯ db (memory)
  (no data-source)
```

LoopBack has connectors for many, many data sources. StrongLoop and IBM support many of the [most popular data sources](http://loopback.io/doc/en/lb3/Connectors-reference.html) and there is also a plethora of [community-supported connectors](http://loopback.io/doc/en/lb3/Community-connectors.html) for others.

Later in the workshop, we will add a conventional data source, for the time being, let's just use the [in-memory data-source](http://loopback.io/doc/en/lb3/Memory-connector.html).

_Note: An issue with using the in-memory data-source is that every time we shut down our application, we will lose all our data. Again, using the in-memory data-source is only for testing and prototyping. Later in this section, we will look at persisting your in-memory data to a file. And in an upcoming step, we will add a conventional data-source._

### Select model's base class:

```
? Enter the model name: event
? Select the data-source to attach event to: db (memory)
? Select model's base class (Use arrow keys)
  Model
❯ PersistedModel
  ACL
  AccessToken
  Application
  Change
  Checkpoint
(Move up and down to reveal more choices)
```

The base class for a model is the object that this model will extend from. There are a number of base model classes and each has a specific purpose. We will choose the `PersistedModel` because we will eventually want the persistence functionality: we'll get that "for free" with this choice.

### Expose `<model>` via the REST API?

```
? Enter the model name: event
? Select the data-source to attach event to: db (memory)
? Select model's base class PersistedModel
? Expose event via the REST API? (Y/n)
```

There may be reasons for keeping an endpoint private, but for the sake of this workshop, we will expose our REST API endpoints for public consumption. We will look at securing endpoints later in the workshop.

### Custom plural form

```
? Enter the model name: event
? Select the data-source to attach event to: db (memory)
? Select model's base class PersistedModel
? Expose event via the REST API? Yes
? Custom plural form (used to build REST URL):
```

LoopBack is very smart about handling plural forms of a model. In this case, `event` is easily made plural as `events`, but it can also handle other common plurals such as `child` to `children` and `mouse` to `mice`. If you have an unconventional model name that isn't easily made plural by adding an `s` then you may need to take advantage of this feature. Most of the time, you can let LoopBack do its magic.

### Common model or server only?

```
? Enter the model name: event
? Select the data-source to attach event to: db (memory)
? Select model's base class PersistedModel
? Expose event via the REST API? Yes
? Custom plural form (used to build REST URL):
? Common model or server only? (Use arrow keys)
❯ common
  server
```

We can choose to only have this model available to the server or we can make it available to the client as well. In an effort to be forward thinking, we will choose `common` in case we want to use one of the client SDKs or any other client options in the future.

## Let's add some properties now!

Now that we have our model configuration in place, we will move on to specific model properties. The prompts will guide us, and as you'll see, after your last property is captured, you can hit enter and it will exit the prompt.

### Model property: name

```
Let's add some event properties now.

Enter an empty property name when done.
? Property name:
```

We will begin with the `venue` property.

### Model property: type

```
? Property name: venue
   invoke   loopback:property
? Property type: (Use arrow keys)
❯ string
  number
  boolean
  object
  array
  date
  buffer
(Move up and down to reveal more choices)
```

The type of our `venue` property will be `string` in this case, but as we can see, there are many options. Having the type set for our properties is important when doing validation, which we will visit later.

### Model property: required

```
? Property name: venue
   invoke   loopback:property
? Property type: string
? Required? (y/N)
```

We will not make our `venue` property required. In fact, for now, we won't make any of properties required, but we will look at how to update any of our properties directly in the configuration.

### Model property: default value

```
? Property name: title
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:
```

We will leave our default value blank here, but adding something like "TBD" would work as well.

### Next model property

```
? Property name: title
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name:
```

As we can see from the terminal output above: when we are done adding properties, we can simply hit `Enter` when prompted for another property. In our case though, we have a few more properties to add, which I've outlined below.

#### Our event properties at a glance:

**All**: not required, no default value.

1. **title**: type: `string`
1. **date**: type: `date`
1. **cost**: type: `number`
1. **lineup**: type: `array` of `string`s
1. **location**: type: `string`
1. **poster**: type: `string`
1. **url**: type: `string`
1. **city**: type: `string`
1. **state**: type: `string`
1. **description**: type: `string`
1. **age-restriction**: type: `string`

*Note: if we aren't so inclined to add so many properties, we can just do 1-4 and leave out 5-11. The first four properties give us a variety of property types and I've included the remaining properties to keep the workshop in sync with the app's codebase.*

Here is the output from adding all of our model's properties:

```
$  band-app git:(master) lb model
? Enter the model name: event
? Select the data-source to attach event to: db (memory)
? Select model's base class PersistedModel
? Expose event via the REST API? Yes
? Custom plural form (used to build REST URL):
? Common model or server only? common
Let's add some event properties now.

Enter an empty property name when done.
? Property name: venue
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: date
   invoke   loopback:property
? Property type: date
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: cost
   invoke   loopback:property
? Property type: number
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: lineup
   invoke   loopback:property
? Property type: array
? The type of array items: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: poster
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: url
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: city
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: state
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: description
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name: age-restriction
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another event property.
Enter an empty property name when done.
? Property name:
```

Now that we are done adding our properties to the `event` model, let's take a look at what LoopBack generated from our command line interactions.

## Model config

The CLI created the `event.json` file based on the answers we gave to prompts in the previous step. One impressive feature of LoopBack is that not only is the command line generator easy and quick to use, but the generated [Model definition file](http://loopback.io/doc/en/lb3/Model-definition-JSON-file.html) (this `event.json` file) is very easy to understand.

`/models/common/event.json`

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

In LoopBack, a [remote method](https://loopback.io/doc/en/lb3/Remote-methods.html) is a model method that is exposed over a custom REST endpoint.
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

At this point, we now have our first API endpoint and we've gotten some understanding about what code LoopBack has generated and how we can extend functionality based on our model. We can start up our Node app by typing `node .` in our terminal. From here, we can make requests to the endpoint either in the browser with a simple form or with some AJAX or perhaps through a tool like Postman or using cURL.

In the next episode, we'll dive deeper into interacting with our API. We'll interact with the Swagger explorer we get for free with LoopBack. We'll look at making requests to our endpoints from various sources and how we can start to build applications making use of our new API.



