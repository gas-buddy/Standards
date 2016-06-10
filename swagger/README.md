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

Paging
======
Situations that may return many results should follow a common paging model wherever possible.

In a request:
```
        - name: page_size
          in: query
          description: The maximum number of results to return in one call
          default: (choose a value)
          required: false
          type: integer
          format: int32
        - name: page
          in: query
          description: The 0-based index of the page of results to retrieve (based on pageSize)
          required: false
          type: integer
          format: int32
```

In a response:
```
      page:
        type: integer
        format: int32
        description: The 0-based index of this page of results
      page_size:
        type: integer
        format: int32
        description: The number of results in a single page
      count:
        type: integer
        format: int32
        description: The total number of results (not pages) available, if known      
```

Next and previous page links are nice, but aren't always possible (e.g. in POSTs without extra work).
If you can't provide next and previous links AND can't provide a count, you will need some
boolean or similar to indicate whether there are additional results or not...

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
