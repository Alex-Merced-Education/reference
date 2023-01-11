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

- 