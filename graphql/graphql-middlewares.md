# GRAPHQL MIDDLEWARE

As Express offers everything we need to process HTTP requests, and GraphQL.js provides functionality for resolving queries, all we still need is the glue between them.  
This glue is provided by libraries like `express-graphql` and `apollo-server` which are nothing but middleware functions for Express!

## express-graphql

Really, its main responsibility is twofold:  
- Ensure that the GraphQL query (or mutation) contained in the body of an incoming POST request can be executed by GraphQL.js. So, it needs to parse out the query and forward it to the graphql function for execution.
- Attach the result of the execution to the response object so it can be returned to the client.  

Start express-graphql server:

    const express = require('express')
    const graphqlHTTP = require('express-graphql')
    const { GraphQLSchema, GraphQLObjectType, GraphQLString } = require('graphql')

    const app = express()

    const schema = new GraphQLSchema({
        query: new GraphQLObjectType({
            name: 'Query',
            fields: {
                hello: {
                    type: GraphQLString,
                    resolve: (root, args, context, info) => {
                        return 'Hello World'
                    }
                }
            }
        })
    })

    app.use('/graphql', graphqlHTTP({
        schema,
        graphiql: true // enable GraphiQL
    }))

    app.listen(4000)

## apollo-server

Better compatibility outside the Express ecosystem.  
At its essence, apollo-server is very similar to express-graphql, with a few minor differences. The main difference between the two is that apollo-server also allows for integrations with lots of other frameworks, like koa and hapi as well as for FaaS provides like AWS Lambda or Azure Functions. Each integration can be installed by appending the corresponding suffix for the package name, e.g. `apollo-server-express`, `apollo-server-koa` or `apollo-server-lambda`.  

However, at the core it also simply is a middleware bridging the HTTP layer with the GraphQL engine provided by GraphQL.js. Here is what an equivalent implementation of the above `express-graphql`-based example looks like with `apollo-server-express`,  

Start apollo server:

    const express = require('express')
    const bodyParser = require('body-parser')
    const { graphqlExpress, graphiqlExpress } = require('apollo-server-express')
    const { GraphQLSchema, GraphQLObjectType, GraphQLString } = require('graphql')

    const schema = new GraphQLSchema({
        query: new GraphQLObjectType({
            name: 'Query',
            fields: {
                hello: {
                    type: GraphQLString,
                    resolve: (root, args, context, info) => {
                        return 'Hello World'
                    },
                },
            },
        }),
    })

    const app = express()

    app.use('/graphql', bodyParser.json(), graphqlExpress({ schema }))
    app.get('/graphiql', graphiqlExpress({ endpointURL: '/graphql' })) // enable GraphiQL

    app.listen(4000)

## graphql-yoga 

Even when using `express-graphql` or `apollo-server`, there are various points of friction:
- Requires installation of multiple dependencies
- Assumes prior knowledge of Express
- Complicated setup for using GraphQL subscriptions  

This friction is removed by `graphql-yoga`, a simple library for building GraphQL servers. It essentially is a convenience layer on top of Express, apollo-server and a few other libraries to provide a quick way for creating GraphQL servers. (Think of it like `create-react-app` for GraphQL servers.)  

Here is what the same GraphQL server we already saw with `express-graphql` and `apollo-server` looks like.

Start graphql-yoga server:

    const { GraphQLServer } = require('graphql-yoga')

    const typeDefs = 
        `type Query {
            hello: String!
        }`

    const resolvers = {
        Query: {
            hello: (root, args, context, info) => 'Hello  World',
        },
    }

    const server = new GraphQLServer({ typeDefs, resolvers })
    server.start() // defaults to 4000

*Note that a GraphQLServer can either be instantiated using a ready instance of GraphQLSchema or by using the convenience API (based on makeExecutableSchema from graphql-tools) as shown in the snippet above.*