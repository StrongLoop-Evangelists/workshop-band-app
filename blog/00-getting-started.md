# Let's Build a Band App with LoopBack! (part 1 of many)

In this series, we will work through building an application to support the needs of DIY bands. We'll start out solving some basic problems and move into more complex ground, eventually transforming the application into a platform that others can use and build upon.

There is a [corresponding workshop](https://github.com/StrongLoop-Evangelists/workshop-band-app) as well as a [code repository](https://github.com/StrongLoop-Evangelists/band-app).

🖥 To see the code for this step, here is [commit/diff](https://github.com/StrongLoop-Evangelists/band-app/commit/e488c9cda9966ce3f4dfd6a5a8e67cc813494db0).
<!--more-->
#### Corresponding Video

🎦 [What is LoopBack: a demo and how to get started](https://youtu.be/Llr18lNuYIo?list=PLxGLihicw5Woe3SV9MCFooTdrI9eOmj54)

🎦 [Initializing a LoopBack application](https://www.youtube.com/watch?v=6hFKR5YKrec&list=PLxGLihicw5Woe3SV9MCFooTdrI9eOmj54&index=3)


## Previously on _Let's build a band app!_

We are starting from scratch so there is no previously in this episode :) but we do have some prerequisites, the first being Node.js. We can easily install Node for our platform at the following website: [nodejs.org](https://nodejs.org).

With Node, we get [npm](https://npmjs.org), the Node package manager. We will use npm to install our second prerequisite and that is LoopBack. Once we have Node installed, we can run `npm install -g loopback-cli`. This will install the LoopBack cli globally (`-g`) on our machine so we can initialize a LoopBack application in any directory on our computer.

## In this episode

In this episode, we will install our prerequisites, get an understanding of what LoopBack provides and initialize our LoopBack application.

## What is LoopBack

[LoopBack](http://loopback.io/) is an open-source framework to rapidly build RESTful APIs in Node.js.  It has a command line interface (CLI) tool that you can use to scaffold your application and build out your API layer based on your data (a model-driven approach). It isn't just fast and easy; it's also robust and extensible. It is built on top of Express, so it has widely-used and battle-tested foundation.

**_LoopBack is so awesome, it is almost magical. Let me show you by building an API in 73 seconds:_**

[![Rapid APIs in LoopBack](https://strongloop.com/blog-assets/2017/band-app/rapid-apis-in-lb-video.png)](https://youtu.be/iOMD27DjuO4 "Rapid APIs in LoopBack")

## Let's initialize our LoopBack application

We can initialize a LoopBack application in our terminal by typing `lb` or `lb app` and this will invoke the generator. The generator will go through a series of prompts to scaffold our LoopBack application. Let's get started.

### What's the name of your application?

```
$ lb
? What's the name of your application? (work-folder-name)
```

The first step is to name our application. Let's call it: `band-app`.

### Enter name of the directory to contain the project:

```
? Enter name of the directory to contain the project: (band-app)
```

In this step, we are simply choosing the name of the directory that we are going to build out the application into. By default, it assumes we would like to create a new directory with the same name as the application that we have given in the previous step. So let's hit enter and let the generator do its work.

**Outputs:**

```
? Enter name of the directory to contain the project: band-app
   create band-app/
     info change the working directory to band-app
```

_Note: If we want to name our directory something else, we can do so in this step. If we had already created a directory with the same name as our application---we are working from that directory---LoopBack will detect this and we could simply hit enter._


### Which version of LoopBack would you like to use?

```
? Which version of LoopBack would you like to use? (Use arrow keys)
  2.x (long term support)
❯ 3.x (current)
```

We will choose the current version of LoopBack, 3.x.

_Note: While LoopBack version 2 is still being supported, version 3 is the current version and in general you should use it for new applications._

### What kind of application do you have in mind?

```
? What kind of application do you have in mind? (Use arrow keys)
❯ api-server (A LoopBack API server with local User auth)
  empty-server (An empty LoopBack API, without any configured models or datasources)
  hello-world (A project containing a controller, including a single vanilla Message and a single remote method)
  notes (A project containing a basic working example, including a memory database)
```

We are presented with four options as starting points:

- api-server (A LoopBack API server with local User auth)
- empty-server (An empty LoopBack API, without any configured models or datasources)
- hello-world (A project containing a controller, including a single vanilla Message and
 a single remote method)
- notes (A project containing a basic working example, including a memory database)

We will choose the default option as it is useful to have a built-in User model to extend from when building our model for our users.

_Note: As alluded to above, it is best practice to extend base models such as `User` rather than using them directly. If we have multiple objects extending from a base model and we modify a base model, there may be adverse effects on all the models extending it._

_Protip: in the command line, type `j` or `k` or &uarr; and &darr; to navigate up or down._

### Scaffolding application and running `npm install`

```
? What's the name of your application? band-app
? Enter name of the directory to contain the project: band-app
   create band-app/
     info change the working directory to band-app

? Which version of LoopBack would you like to use? 3.x (current)
? What kind of application do you have in mind? api-server (A LoopBack API server with local User auth)
Generating .yo-rc.json


I'm all done. Running npm install for you to install the required dependencies. If this fails, try running the command yourself.


   create .editorconfig
   create .eslintignore
   create .eslintrc
   create server/boot/root.js
   create server/middleware.development.json
   create server/middleware.json
   create server/server.js
   create README.md
   create server/boot/authentication.js
   create .gitignore
   create client/README.md
```

For an explanation of all the files and directories that the tool creates, see [Project layout reference](http://loopback.io/doc/en/lb3/Project-layout-reference.html) in the LoopBack documentation.

At this point, the tool has scaffolded our application.  Then it runs `npm install` to install all of the application's external dependences. This may take a moment, depending on internet connection. Do not be alarmed by the many modules that are installed: to reduce duplication of effort, Node.js has a large ecosystem of many small modules. The Node package manager, npm, handles these modules in a smart and efficient manner.

Once the tool finishes installation of external modules, we will see a listing of these modules in a tree structure.

### Handy next steps for our convenience

```
Next steps:

  Change directory to your app
    $ cd band-app

  Create a model in your app
    $ lb model

  Run the app
    $ node .
```

At this point, our application is initialized and the terminal outputs some helpful next steps to continue development. We can run our node app with `node .` and it will output, two URLs: one which will take us to the root of our app and one which takes us to our API explorer. There won't be much at the explorer yet, but we will 'explore' that later (sorry for the pun), but if we go to the root of our application, we will see a timestamp letting us know it is working properly. The code that gets the timestamp and outputs it in the browser in our `server/server.js` file. Until next time, you can check out that server file and see that it looks very much like an Express server (remember, Express underpins a LoopBack application.) We may also take a look at the `config.json` file, which contains some of the configuration for our application, such as our API root, the host and port information. Feel free to poke around further.

In the next episode, we will initialize our first endpoint which will be generated based on some questions about our data. It'll be fun, fast and easy! See you then.

