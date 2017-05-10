# Create our first model-driven API endpoint

🖥 To see the code for this step, here is [commit/diff](https://github.com/StrongLoop-Evangelists/band-app/commit/3137aa04dce8caf6589c65032d3cfb8e3e5e1924).

---

LoopBack takes a model-driven approach to building out APIs. Based on our responses to the command line prompts, it will create a JSON file with our model schema and details.  For an introduction to LoopBack models see [LoopBack core concepts -  Models](http://loopback.io/doc/en/lb3/LoopBack-core-concepts.html#models) in the LoopBack documentation.

To invoke the generator, we type `lb model` and go through the list of prompts.

_Note: `cd` into your working directory, if you haven't already._

### Enter the name of your model

```
➜  band-app git:(master) ✗ lb model
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

We can choose to only have this model available to the server or we can make it available to the client as well. In an effort to be forward thinking, we will choose `common` in case we want to use the Angular SDK or any other client options in the future.

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

1. **location**: type: `string`
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
➜  band-app git:(master) lb model
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

**Next Step:** [Model files: config and hooks](03-model-files.md)
