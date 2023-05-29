# Overview

- GraphQL is a language for querying and manipulating data.
- It's widely viewed as more powerful, flexible, and efficient than REST

## Benefits provided by GraphQL

- Using GraphQL provides the following benefits

### Predictable results from your queries

- A GraphQL query returns only data the user asks for in their query

### Single request for many results

- A single request made through GraphQL can return any number of resources and their fields by following references between them as defined in the type schema

### Organized data with a typed schema

- A single schema defines how users access data using GraphQL
- These schemas, formatted as JSON object, let users know exactly how to get the data they need

- Example

## Why use GraphQL over REST

- While GraphQL and REST are both specifications for constructing and querying APIs, GraphQL has some significant advantages over REST

### No versioning

- REST APIs typically have multiple versions, such as v1, v2, etc. This is because updating endpoinrts in REST will often impact existing queries

- With GraphQL, there is no need for versioning, since new types and fields can be added to the schema without impacting existing queries

- Removing fields is done through deprecation instead of deleting them from the schema
- If an old query tries to read a deprecated field, GraphQL display a customized warning

- This prevents old queries from throwing confusing errors when trying to read outdated fields, lending to code maintainability

### Faster and more efficient

- REST APIs typically require loading from multiple URLs.
- Imagine a REST API designed to get users and their forum posts.
- `users/id` would return information like `name` and `id` would have to be queried separately to return the user's `comments`

- With GraphQL, these `types` and their `fields` are returned using one query, which saves calls to the API.

# See example in doc