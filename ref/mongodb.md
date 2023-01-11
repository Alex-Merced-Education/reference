# MongoDB

## Running Mongo Locally

After installying mongo you need to start up the mongo server which is done by running the command.

```
mongod
```

Output should come out and not stop, that is the server doing its thing. But if it stops, then there is probably an error in the output that would look like this.

```
{"t":{"$date":"2023-01-11T15:58:19.950-05:00"},"s":"E",  "c":"CONTROL",  "id":20557,   "ctx":"initandlisten","msg":"DBException in initAndListen, terminating","attr":{"error":"IllegalOperation: Attempted to create a lock file on a read-only directory: /data/db"}}
{"t":{"$date":"2023-01-11T15:58:19.950-05:00"},"s":"I",  "c":"REPL",     "id":4784900, "ctx":"initandlisten","msg":"Stepping down the ReplicationCoordinator for shutdown","attr":{"waitTimeMillis":15000}}
```

In this output you want to play where's waldo to find the error.

```
{"error":"IllegalOperation: Attempted to create a lock file on a read-only directory: /data/db"}
```

- `Attempted to create a lock file on a read-only directory: /data/db`: this error means it did not have enough privileges to write, you probably need to run `sudo mongod`.

- `SocketException: Already in use` This mean it may already be running or something is already running on port 27017 (default port for mongodb server). First you can try killing the port `sudo kill -9 $(sudo lsof -t -i:27017)` and then running `mongod`. If that doesn't work you run the server on a different port. `mongod --port 27020` any port would work. just keep in mind you'll need to use that port when connecting to mongo shell `mongosh --port 27020` and in your connection string `mongodb://localhost:27020/databasename`.

- `Dbpath in use /data/db/mongod.lock` the lock file is a way that mongo makes sure writes to the file system are safe. Usually this file is deleted when the server shuts down, so if you had to kill a server this file may still exist and prevent it from running again. To fix the problem run `rm /data/db/mongod.lock` which may need `sudo`. If this is still a problem you may want mongod to just store data elsewhere.

**Having Mongod Store Data Elsewhere**

By default mongod will want to store data in `/data/db`, but if that's a problem cause the directory doesn't exist or you don't have privileges to access it you can create some other directory.

```
mkdir ~/mongodata
```

then run mongod to save data to that folder.

```
mongod --dbpath ~/mongodata
```

**Mongo Shell Connection**

Connecting to your local mongo server assuming it's running on port 27017 would just require...

```
mongosh
```

If it's running on another port then.

```
mongosh --port 27020
```

If you want to connect to a remote mongo server like you may have from mongodb.com.

```
mongosh mongodb+srv://<username>:<password>@x.y.mongodb.net/?retryWrites=true&w=majority
```

## Setting Up MongoDB.com

- Signup at mongodb.com

- create a free cluster

- once cluster is up go to network access and white all ips `0.0.0.0`

- Head to database access and create a user with read/write access to all databases

- Click on database, then click on connect and choose "connect your app" and get your connection string. Put your username and password in it and connect use that string to connect with your application or mongosh.