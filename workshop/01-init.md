# Let's initialize our LoopBack application


We can initialize a LoopBack application in our terminal by typing `lb` or `lb app` and this will invoke the generator. The generator will go through a series of prompts to scaffold out our LoopBack application. Let's get started.

### What's the name of your application?

```
➜  workshop-band-app git:(master) lb

     _-----_
    |       |    ╭──────────────────────────╮
    |--(o)--|    │  Let's create a LoopBack │
   `---------´   │       application!       │
    ( _´U`_ )    ╰──────────────────────────╯
    /___A___\   /
     |  ~  |
   __'.___.'__
 ´   `  |° ´ Y `

? What's the name of your application? (workshop-band-app)
```

The first step is to name the application. Let's call this application `band-aid`.

### Enter name of the directory to contain the project:

```
? What's the name of your application? band-aid
? Enter name of the directory to contain the project: (band-aid)
```

In this step, we are simply choosing the name of the directory that we are going to build out the application into. By default, it assumes we would like to create a new directory with the same name as the application that we have given in the previous step. So let's hit enter and let the generator do its work.

**Outputs:**

```
? What's the name of your application? band-aid
? Enter name of the directory to contain the project: band-aid
   create band-aid/
     info change the working directory to band-aid
```

_Note: If we want to name our directory something else, we can do so in this step. If we had already created a directory with the same name as our application---we are working from that directory---LoopBack will detect this and we could simply hit enter._


### Which version of LoopBack would you like to use?

```
? What's the name of your application? band-aid
? Enter name of the directory to contain the project: band-aid
   create band-aid/
     info change the working directory to band-aid

? Which version of LoopBack would you like to use? (Use arrow keys)
  2.x (long term support)
❯ 3.x (current)
```

We will choose the current version of LoopBack, 3.x.

_Note: While LoopBack version 2 is still being supported, it is advised to use the current version of LoopBack (`3.x`) which is chosen by default._

### What kind of application do you have in mind?

```
? What's the name of your application? band-aid
? Enter name of the directory to contain the project: band-aid
   create band-aid/
     info change the working directory to band-aid

? Which version of LoopBack would you like to use? 3.x (current)
? What kind of application do you have in mind? (Use arrow keys)
❯ api-server (A LoopBack API server with local User auth)
  empty-server (An empty LoopBack API, without any configured models or datasources)
  hello-world (A project containing a controller, including a single vanilla Message and a single remote method)
  notes (A project containing a basic working example, including a memory database)
```

The user is presented with 4 options as starting points for scaffolding a LoopBack application.

- api-server (A LoopBack API server with local User auth)
- empty-server (An empty LoopBack API, without any configured models or datasources)
- hello-world (A project containing a controller, including a single vanilla Message and
 a single remote method)
- notes (A project containing a basic working example, including a memory database)

We will choose the default option as it is useful to have a built-in User model to extend from when building our model for our users.

_Note: as alluded to above, it is best practice to extend base models such as `User` rather than using them directly. If we have multiple objects extending from a base model and we modify a base model, there may be adverse effects on all the models extending it._

_Protip: in the command line, we can type `j` or `k` to navigate up or down._

### Scaffolding application and running `npm install`

```
? What's the name of your application? band-aid
? Enter name of the directory to contain the project: band-aid
   create band-aid/
     info change the working directory to band-aid

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

At this point, our application has been scaffolded and the generator runs `npm install` to retrieve all of our external dependences. This may take a moment, depending on internet connection. Do not be alarmed by the many modules that are installed: to reduce duplication of effort, Node.js has a large ecosystem of many small modules. NPM, the node package manager, handles these modules in a smart and efficient manner.

Once the installation of external modules is complete, we will see a listing of these modules in a tree structure.

### Handy next steps for our convenience

```
Next steps:

  Change directory to your app
    $ cd band-aid

  Create a model in your app
    $ lb model

  Run the app
    $ node .
```

At this point, our application is initialized and the terminal outputs some helpful next steps to continue development. Let's jump right in and create our first model-driven API endpoint.

**Next Step:** [Create our first model](02-first-model.md)
