# GRAPHQL MIDDLEWARE

As Express offers everything we need to process HTTP requests, and GraphQL.js provides functionality for resolving queries, all we still need is the glue between them.  
This glue is provided by libraries like `express-graphql` and `apollo-server` which are nothing but middleware functions for Express!

## express-graphql

Really, its main responsibility is twofold:  
- Ensure that the GraphQL query (or mutation) contained in the body of an incoming POST request can be executed by GraphQL.js. So, it needs to parse out the query and forward it to the graphql function for execution.
- Attach the result of the execution to the response object so it can be returned to the client.