# Connect to a data-source

In this step, we are going to instantiate a Cloudant DB and connect it to our application's `album` model. While we will be using a hosted Cloudant data-source instance, most data-sources will be connected in a similar way.

## Create a Cloudant DB service instance

Let's jump into the Bluemix catalog and create a Cloudant DB service, if you don't already have one created. In the catalog section of Bluemix, we can filter by Data & Analytics from the left sidebar and choose the Cloudant DB service:

![Bluemix - Data Apps](assets/bluemix-catalog-data.png)

Once we choose the Cloudant DB service, we are given the option to provide details to initialize our service instance:

![Bluemix - Create Cloudant service](assets/cloudant-details.png)

Clicking create takes us to our Cloudant service instance page:

![Bluemix - Cloudant page](assets/band-aid-cloudant.png)

The next thing we need to do is create the database in our Cloudant service instance. We do so by clicking on the Launch button in (shown in screen above) which will launch the Cloudant dashboard for this service instance:

![Bluemix - Cloudant page](assets/cloudant-create-db.png)

In the dashboard (shown above), we need to click the Databases option in the left sidebar and then near the top right, click Create Database. In the screen above, we have named the database `album`.

![Bluemix - Cloudant page](assets/cloudant-credentials.png)

One more thing we need to do is get the credentials for our Cloudant service to be used in the next step. To do so, we need to go back to our Bluemix dashboard, and click on our Cloudant service instance for this application. From instance overview page, click the 'Service Credentials' tab and click 'View Credentials'. Take note of the information in the resulting view, particularly the `url` key/value pair. We will need that bit of info for the next step where we hook up LoopBack to this data-source.

# Installation

Enter the following in the top-level directory of your LoopBack application:

`$ npm install loopback-connector-cloudant --save`

The --save option adds the dependency to the application’s package.json file.

Then we will run `lb datasource` to setup Cloudant for our app.

```
➜  band-aid git:(master) ✗ lb datasource
? Enter the data-source name: cloudantDB
? Select the connector for cloudantDB: IBM Cloudant DB (supported by StrongLoop)
Connector-specific configuration:
? Connection String url to override other settings (eg: https://username:password@host): https://your-username-here:your-password-here@your-host-here
? database: album
? username:
? password:
? modelIndex:
```

And finally, let's change our `album` model to use the Cloudant DB. We can do so by updating our `model-config.json`. We'll see a section near the bottom for `album` and will update the object key 'dataSource' to 'cloudantDB', which is what we called this data-source when adding it to our application above. It should look like this:

```
  "album": {
    "dataSource": "cloudantDB",
    "public": true
  }
```

*Note: if you are storing your username and password in your code, you will want to be sure to NOT commit them to a public repository.*

In an effort to address this concern, let's dive in a little deeper.

## Hidden secrets

The easiest way to contain sensitive information in your application is to keep it in a file that is in your 'ignore list', in this instance, our `.gitignore`. LoopBack has a way of handling this pretty easily -- the application, on startup, looks for a number of files that aren't there by default, but can be added to handle special circumstances, such as these. The file we will use is `datasources.local.js` and it will expose our secrets file, which we can call `env.js`.

## `env.js`

In `env.js`, we export our data-source configuration object with our secrets.

*Note: we must remember to add this `env.js` file to our `.gitignore` so that it doesn't accidentally end up in our public repository. Please do so now.*

```javascript
'use strict';

module.exports = {
  cloudantDB: {
    "url": "https://your-username:your-password@your-host",
    "database": "album",
    "username": "",
    "password": "",
    "name": "cloudantDB",
    "modelIndex": "",
    "connector": "cloudant"
  }
};
```

## `datasources.local.js`

Our `datasources.json` file is a JSON file that has no JavaScript functionality. So we have to put that logic in our `datasources.local.js`. From this file, we can simply require in the secret `env.js` file (stored in the root of our application) and export it through Node's module system. This will make the `cloudantDB` object we exported in the `env.js` file available to the system. LoopBack looks at this particular file with the expectation that if it exists, it will merge it with the data-source JSON config file, `datasources.json`.

```javascript
'use strict';

module.exports = require('../env.js');
```

## `datasources.json`

This file needs to exist for the application to run, so we can either make it an empty file or simply leave the config object for our in-memory data-source even though we currently aren't using it.

```json
{
  "db": {
    "name": "db",
    "connector": "memory"
  }
}
```

Okay, now that we have our secrets hidden, let's move on.

**Next Step:** [Connect a datasource](06-datasource.md)
