---
title: Middleware
nav_order: 4
layout: default
---

# Middleware

Middleware enables consumers to hook into the ShareDB server pipeline. Objects can be asynchronously manipulated as they flow through ShareDB.

This can be particularly useful for [authentication]({% link middleware/authentication.md %}).

## Registering middleware

Middleware is registered on the server with [`backend.use()`]({% link api/backend.md %}#use):

```js
backend.use(action, function(context, next) {
  // Do something with the context

  // Call next when ready. Can optionally pass an error to stop
  // the current action, and return the error to the client
  next(error)
})
```

The `action` should be one of the values listed below.

## Actions

The actions represent different stages of information flow through the server. These hooks are also available on [`backend.MIDDLEWARE_ACTIONS`]({% link api/backend.md %}#middleware_actions--object).

All of the actions will have these `context` properties:

`action` -- string

> The triggered middleware action

`agent` -- [Agent]({% link api/agent.md %})

> The [`Agent`]({% link api/agent.md %}) communicating with the client

`backend` -- [Backend]({% link api/backend.md %})

> The [`Backend`]({% link api/backend.md %}) handling the request

### `'connect'`

A new client connected to the server.

This action has these additional `context` properties:

`stream` -- [Stream](https://nodejs.org/api/stream.html)

> The [`Stream`](https://nodejs.org/api/stream.html) that connected

`req` -- Object

> The `request` argument provided to [`backend.listen()`]({% link api/backend.md %}#listen)

### `'receive'`

The server received a message from a client.

This action has these additional `context` properties:

`data` -- Object

> The received data

### `'reply'`

The server is about to send a non-error reply to a client message.

This action has these additional `context` properties:

`request` -- Object

> The client's received request

`reply` -- Object

> The reply about to be sent
