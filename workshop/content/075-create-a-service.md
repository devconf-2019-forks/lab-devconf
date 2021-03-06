---
Title: Deploying a service
PrevPage: 070-deploying-component-from-source-code
NextPage: 080-linking-components-to-services
---

As we explained before, the *Software Catalog* available in OpenShift provides a list of runtimes to be used for your components, but also offers you the possibility to deploy COTS (Common Off the Shelf) software, like databases. We can see a list of these by querying the catalog:

```execute-1
odo catalog list services
```

Or you can search the catalog by keywords. As we need a *mongodb* database, let's see what the catalog provides:

```execute-1
odo catalog search service mongodb
```

Typically, the creation of this services can be complex as they might require a huge amount of parameterization that might not be known to the user. For this, `odo` provides an interactive mode that is really handy. Let's create a mongodb database and see it in action:

```execute-1
odo service create
```

When you don't specify any parameter to the creation of a service, you'll be interactively asked for answers. For this specific component, you will need to provide the following:

- *Which kind of service do you wish to create* Select `database` with the arrows and hit enter.
- *Which database service class should we use* As you type `mongo` the list will be filtered. From the two available options, select `mongodb-ephemeral`
- For the following options, just accept the defaults.

You will see something similar to this:

```bash
 odo service create
? Which kind of service do you wish to create? database
? Which database service class should we use? mongodb-ephemeral
? Enter a value for string property DATABASE_SERVICE_NAME (Database Service Name): mongodb
? Enter a value for string property MEMORY_LIMIT (Memory Limit): 512Mi
? Enter a value for string property MONGODB_DATABASE (MongoDB Database Name): sampledb
? Enter a value for string property MONGODB_VERSION (Version of MongoDB Image): 3.2
? Provide values for non-required properties No
? How should we name your service  mongodb-ephemeral
 ✓  Service 'mongodb-ephemeral' was created
Progress of the provisioning will not be reported and might take a long time.
You can see the current status by executing 'odo service list'
```

Now we can wait until the service is provisioned:

```execute-1
odo service list
```

Keep re-running this command, and once provisioned, we should see similar output to this:

```bash
NAME                  TYPE                  STATUS
mongodb-ephemeral     mongodb-ephemeral     ProvisionedAndBound
```

Instead of interactive mode, you could instead have provided all these configuration via command line flags, but for *services* that have many parameters this can be inconvenient. A separate example using PostgreSQL, and not MongoDB as we want here, where command line flags were used is:

```bash
odo service create dh-postgresql-apb my-postgresql-db --plan dev -p postgresql_user=luke -p postgresql_password=secret
```

We have our MongoDB database already running though, so let's move on.
