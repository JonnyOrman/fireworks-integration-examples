# fireworks integration examples

Examples of how to use fireworks applications together.

Contains the following examples:

- `docker` - demonstrates the use of the Docker images.
- `go` - demonstrates the use of the Go packages.

Each example can be run with `docker-compose`.

## How to run

To run an example, run `docker-compose up --build` in its directory.

It will run three services:

- `localhost:3001` - create firestore documents using `firesert`.
- `localhost:3002` - read firestore documents using `firead`.
- `localhost:3003` - delete firestore documents using `firelete`.

### firesert service

The firesert service receives Pub/Sub push messages and inserts their data into a firestore collection.

To trigger it, submit a `POST` to `localhost:3001` with the `Content-Type` header set to `application/json` and the following body:
```
{
    "message": {
        "data": "ewogICAgICAgICJwcm9wMSI6ICJhYmMiLAogICAgICAgICJwcm9wMiI6IDEyMwp9"
    }
}
```

The `data` value must be the document you want to insert into firestore in base64 encoding. The above example `data` will insert the following document:
```
{
    "prop1": "abc",
    "prop2": 123
}
```

The document will be inserted into a collection called "MyCollection". You can view it in the emulator UI at `localhost:4000/firestore/data`.

### firead service

The firead service receives requests for firestore documents and returns them in the response body.

After creating a document, submit a `GET` to `localhost:3002/{documentID}`. Check the emulator UI for the ID of the document you created and substitute it into the url. For example, if you created a document with the ID "kFdcuYv6z8AmjcI2REgC", use the URL `localhost:3002/kFdcuYv6z8AmjcI2REgC`.

### firelete service

The firelete service receives Pub/Sub push messages and deletes the document from firestore with the document ID specified in the message.

To trigger it, submit a `POST` to `localhost:3003` with a `Content-Type` header set to `application/json` and the following body:
```
{
    "message": {
        "data": "ewogICAgICAgICJkb2N1bWVudElEIjogImtGZGN1WXY2ejhBbWpjSTJSRWdDIgp9"
    }
}
```

The `data` value must include a `documentID` value which matches the ID of the document to delete. The above example `data` has the document ID "kFdcuYv6z8AmjcI2REgC":
```
{
    "documentID": "kFdcuYv6z8AmjcI2REgC"
}
```

The document with the specified ID will be deleted from the colleciton. Check that it is gone in the emulator UI at `localhost:4000/firestore/data`.