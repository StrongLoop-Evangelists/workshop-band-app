# Create our first model-driven API endpoint

LoopBack takes a model-driven approach to building out APIs. Based on our responses to the command line prompts, it will create a JSON file with our model schema and details.

To invoke the generator, we type `lb model` and go through the list of prompts.

_Note: `cd` into your working directory, if you haven't already_

### Enter the name of your model

```
➜  band-aid git:(master) ✗ lb model
? Enter the model name:
```

We'll call our first model while building our marketplace app `Album`.

_Note: models should be singular. The application handles plural naturally, as we'll see in an upcoming step._

### Select the data-source

```
? Enter the model name: album
? Select the data-source to attach album to: (Use arrow keys)
❯ db (memory)
  (no data-source)
```

LoopBack has connectors for many, many data-sources. The most common ones are supported by the StrongLoop team and there is a plethora of community supported connectors for the less common data-sources. Later in the workshop, we will add a conventional data-source, for the time being, let's just use the in-memory data-source.

_Note: an issue with using the in-memory data-source is that every time we shut down our application, we will lose all our data. Again, using the in-memory data-source is only for testing and prototyping. Later in this section, we will look at persisting your in-memory data to a file. And in an upcoming step, we will add a conventional data-source._

### Select model's base class:

```
? Enter the model name: album
? Select the data-source to attach album to: db (memory)
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

The base class for a model is the object that this model will extend from. There are a number of base model classes and each has a specific purpose. We will choose the `Persisted Model` because we will eventually want the persistence functionality: we'll get that "for free" with this choice.

### Expose `<model>` via the REST API?

```
? Enter the model name: album
? Select the data-source to attach album to: db (memory)
? Select model's base class PersistedModel
? Expose album via the REST API? (Y/n)
```

There may be reasons for keeping your endpoint private, but for the sake of this workshop, we will expose our REST API endpoints for public consumption. We will look at securing endpoints later in the workshop.

### Custom plural form

```
? Enter the model name: album
? Select the data-source to attach album to: db (memory)
? Select model's base class PersistedModel
? Expose album via the REST API? Yes
? Custom plural form (used to build REST URL):
```

LoopBack is very smart about handling plural forms of a model. In this case, `album` is easily made plural as `albums`, but it can also handle other common plurals such as `child` to `children` and `mouse` to `mice`. If you have an unconventional model name that isn't easily made plural by adding an `s` then you may need to take advantage of this feature. Most cases, we can let LoopBack do its magic.

### Common model or server only?

```
? Enter the model name: album
? Select the data-source to attach album to: db (memory)
? Select model's base class PersistedModel
? Expose album via the REST API? Yes
? Custom plural form (used to build REST URL):
? Common model or server only? (Use arrow keys)
❯ common
  server
```

We can choose to only have this model available to the server or we can make it available to the client as well. In an effort to be forward thinking, we will choose `common` in case we want to use the Angular SDK or any other client options in the future.

## Let's add some properties now!

Now that we have our model config in place, we will move on to specific model properties. The prompts will guide us, and as you'll see, after your last property is captured, you can hit enter and it will exit the prompt.

### Model property: name

```
Let's add some album properties now.

Enter an empty property name when done.
? Property name:
```

We will begin with the `title` property.

### Model property: type

```
? Property name: title
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

The type of our `title` property will be `string` in this case, but as we can see, there are many options. Having the type set for our properties is important when doing validation, which we will visit later.

### Model property: required

```
? Property name: title
   invoke   loopback:property
? Property type: string
? Required? (y/N)
```

We will make our `title` property required as it is an important piece of information.

### Model property: default value

```
? Property name: title
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:
```

We will leave our default value blank here. If the property were not required, we might put a default value in for when it is not submitted.

### Next model property

As we can see from the terminal output, when we are done, we can simply hit enter when prompted for another property. In our case though, we have a few more properties to add. Here is the output from adding all of our model's properties:

```
Let's add some album properties now.

Enter an empty property name when done.
? Property name: title
   invoke   loopback:property
? Property type: string
? Required? Yes
? Default value[leave blank for none]:

Let's add another album property.
Enter an empty property name when done.
? Property name: year
   invoke   loopback:property
? Property type: number
? Required? No
? Default value[leave blank for none]:

Let's add another album property.
Enter an empty property name when done.
? Property name: record-label
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another album property.
Enter an empty property name when done.
? Property name: artwork
   invoke   loopback:property
? Property type: string
? Required? No
? Default value[leave blank for none]:

Let's add another album property.
Enter an empty property name when done.
? Property name: tracks
   invoke   loopback:property
? Property type: array
? The type of array items: object
? Required? No
? Default value[leave blank for none]:

Let's add another album property.
Enter an empty property name when done.
? Property name:
```

_Note: we need our tracks property to be an array. In doing so, LoopBack asked us what type of items will be in our array._

Once we are done adding our properties, we hit `<enter>` and the prompt will exit.

**Next Step:** [Model files: config and hooks](03-model-files.md)
