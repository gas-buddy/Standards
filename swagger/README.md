Swagger
=======

We make heavy use of [Swagger](http://swagger.io/) at GasBuddy for both external and internal service interface definition.
Wherever possible, we should make the verbs and operational model follow [REST](https://en.wikipedia.org/wiki/Representational_state_transfer)
best practices.

URLs
====
URLs should be lower case, and multiple words in a path fragment should be separate with dashes.
The first path component of all APIs should be a version identifer, such as '/1.0'. Entities
described in the API are typically plural when a collection is involved, such as **/accounts/{account_id}**.

Definitions
=======
Data type definitions should be UpperCamelCase and properties inside them should be 
lower snake_case. For example:

```
PointOfInterest:
  type: object
  properties:
    color_of_door:
      type: string
```

Boilerplate
===========
```
swagger: '2.0'
info:
  version: '1.0'
  title: GasBuddy _something_ API
  description: Explain your purpose for existence
basePath: "/1.0"
consumes:
  - application/json
produces:
  - application/json
schemes:
  - http
  - https
paths:
definitions:
```
