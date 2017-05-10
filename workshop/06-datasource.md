# Connect to a data-source

🖥 To see the code for this step, here is [commit/diff](https://github.com/StrongLoop-Evangelists/band-app/commit/a2c5667cd21ed92f6e5c1f95e3c5e0da43a12dda).

---

In this step, we are going to instantiate a Cloudant DB and connect it to our application. While we will be using a hosted Cloudant DB, most data-sources can be connected in a similar way.

## Cloudant DB on Bluemix

Let's jump into the [Bluemix catalog](https://console.ng.bluemix.net/catalog/), filter by Data & Analytics, find CloudantDB and create the service:

![Bluemix - Data Apps](assets/bluemix-catalog-data.png)

### Create your service

Once we choose the Cloudant DB service, we are given the option to customize the service and credential names:

![Bluemix - Create Cloudant service](assets/cloudant-details-unbound.png)

Clicking create takes us to our Cloudant service instance page:

![Bluemix - Cloudant page](assets/cloudant-bm-dash.png)

### Create our database

The next thing we need to do is create the database in our Cloudant service instance. We do so by clicking on the Launch button (shown in screen above) which will launch the Cloudant dashboard for this service instance:

![Bluemix - Cloudant page](assets/cloudant-create-db.png)

In the dashboard (shown above), we need to click the Databases option in the left sidebar and then near the top right, click Create Database. We will name the database `band-app`.

Lastly, we need to get the credentials for our Cloudant service -- these credentials are needed in the next step. To do so, we will go back to our Bluemix dashboard and from the Cloudant instance overview page, click the 'Service Credentials' tab (see below) and click 'View Credentials'. Take note of the credentials in the resulting view, particularly the `url` key/value pair. We will need this information for the next step where we connect this data-source to LoopBack.

![Bluemix - Cloudant page](assets/cloudant-credentials.png)

## Use Cloudant in our app

### Installation

Enter the following in the top-level directory of your LoopBack application:

`$ npm install loopback-connector-cloudant --save`

The --save option adds the dependency to the application’s package.json file.

### Add to our LoopBack app

Then we will run `lb datasource` to setup Cloudant for our app.

```
➜  band-app git:(master) ✗ lb datasource
? Enter the data-source name: db
? Select the connector for cloudantDB: IBM Cloudant DB (supported by StrongLoop)
Connector-specific configuration:
? Connection String url to override other settings (eg: https://username:password@host): https://your-username-here:your-password-here@your-host-here
? database: band-app
? username:
? password:
? modelIndex:
```

### Connect our existing model to Cloudant

And finally, let's change our `event` model to use the Cloudant DB. We can do so by updating our `model-config.json`. We'll see a section near the bottom for `event` and will update the object key 'dataSource' to 'cloudantDB', which is what we called this data-source when adding it to our application above. It should look like this:

```json
  "event": {
    "dataSource": "db",
    "public": true
  }
```

*Note: if you are storing your username and password in your code, you will want to be sure to NOT commit them to a public repository.*

## 🚨 SECURITY ISSUE 🚨

We are putting our username and password in our code. If we aren't going to use a public repository, this isn't an issue, but if we do commit this to a public repository, we are making our application vulnerable. There are a number of ways to alleviate this issue. We will look at a preferred method in the next step.

**Next Step:** [Working with credentials](07-credentials.md)
