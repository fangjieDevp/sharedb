---
title: Manipulating a document
nav_order: 3
layout: default
has_children: true
---

<!-- TODO: Link to API docs -->

# Manipulating a document

## Document lifecycle

In ShareDB, documents have three states of being:

 - **Uncreated** -- the document has not yet been created, and is how all document start
 <!-- TODO: Link to ops -->
 - **Created** -- the document has been created, and can be manipulated by its type's ops
 - **Deleted** -- the document has been deleted, and acts exactly the same as an uncreated document

## Getting a document

The first step of manipulating any document is to `get` it:

```js
var doc = connection.get('doc-collection', 'doc-id')
```

{: .info }
Getting a document is **not** the same as fetching or subscribing to it. No network requests are made when calling `get`.

## Fetching a document

Once we have an instance of a `Doc`, we can `fetch` its contents from the server:

```js
doc.fetch((error) => {
  if (error) throw error
  if (!doc.type) {
    // If the doc has no type, it hasn't been created yet (see below)
  } else {
    // Otherwise its contents have been loaded into doc.data
    doSomethingWith(doc.data)
  }
})
```

## Subscribing to a document

One of the main benefits of ShareDB is the ability to keep a document in sync with the server. `fetch` will provide a one-off fetch, but will **not** keep the document up-to-date.

If we want a document to stay up-to-date, we should `subscribe`:

```js
doc.subscribe((error) => {
  if (error) throw error
  // doc.data has been fetched, and will be kept up-to-date with the server's version
  doSomethingWith(doc.data)
})
```

### The `op` event

When subscribed, a document will emit `op` events whenever an op (local or remote) is applied to the document. This is the hook to listen to in order to update UI in response to document changes:

```js
doc.on('op', (op, source) => {
  // doc.data has changed
  doSomethingWith(doc.data)
})
```

The `op` handler has the following arguments:

- `op` -- the op that was submitted to the server
<!-- TODO: Link to submitOp -->
- `source` -- the optional `source` provided to `submitOp`, which will always be truthy for a local op (one that was submitted from this document)

<!-- TODO: Link to Quill example -->

{: .info }
The `source` is important, because -- in some cases -- local ops should be handled differently to remote ops. For example, reapplying local ops to Quill leads to an infinite loop.

## Creating a document

A document must first be created before anything else can be done to it.

```js
var payload = {foo: 'bar'}
var type = 'json0'
doc.create(payload, type, (error) => { ... })
```

The above example will create a JSON0 document that looks like: `{"foo": "bar"}`

The `type` argument is optional, and will default to [`json0`](./types/json0.md) if omitted.

{: .info }
If a document has already been created, its `doc.type` will be set, and will contain the document's type (`json0` by default)
