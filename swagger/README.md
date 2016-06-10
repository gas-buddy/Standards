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

Identifiers
===========
Unique ids in all APIs should be treated as strings. Generally, we should not expose raw numeric identifiers to API consumers. This avoids people picking poor data types for storage and limits information disclosure such as the order
of entity creation or total size of the collection.

Paging
======
Situations that may return many results should follow a common paging model.
Not all paging situations are the same, and mainly the issue is around the rate of change in the
underlying data. The general goal is to be able to specify a limited number of results and
an offset into the result set. Since simple "page number/page size" paging can be expressed
with limits and cursors, cursoring is the preferred API representation even if the backend
implements it as simple offsets.

In a request:
```
        - name: limit
          in: query
          description: The maximum number of results to return in one call
          default: (choose a value)
          required: false
          type: integer
          format: int32
        - name: cursor
          in: query
          description: The server-provided cursor for the beginning of the result set. Omit for the "first page"
          required: false
          type: string
```

In a response:
```
      cursor:
        type: object
        properties:
          next:
            type: string
            description: The value that should be passed as cursor to retrieve the previous set of results
          previous:
            type: string
            description: The value that should be passed as cursor to retrieve the next set of results
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
