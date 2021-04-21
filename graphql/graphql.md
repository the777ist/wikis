# GRAPHQL:

## OVERVIEW:

-GraphQL enables declarative data fetching where a client can specify exactly what data it needs from an API.

-Instead of multiple endpoints that return fixed data structures, a GraphQL server only exposes a single endpoint and responds with precisely the data that a client asked for.

-GraphQL minimizes the amount of data that needs to be transferred over the network, thus increasing performance.

-We could write our REST APIs in a way appropriate to the screen to return only the info needed, but product iterations would be slower. i.e. every time the design changes, the APIs might need to be re-written/updated to solve those particular needs. 

-With graphQL, we have single endpoint. You actually only send a single request to the server and the server would respond with all the information that you need.

-The way how this works is by sending a post request to the server where you include a query in the body of that request that describes all the data requirements of the client.

-Instrumenting and measuring performance of resolvers provides crucial insights about bottlenecks in your system.

-GraphQL offers strong type capabilities, owing to it's schema system.

-The schema allows independednt FE and BE development since both are aware of the structure of data to be returned.

## CONCEPTS:

-Schema definition language (SDL) for defining schemas, eg:

    type Person {
        name: String!
        age: Int
        posts: [Post!]!
    }

    type Post {
        title: String!
        author: Person! 
    }

*! means a compulsory field*  
*[] means multiple*

-Example queries:

    {
        allPersons {
            name
        }
    }

    {
        allPersons(<OPTIONAL_ARGS>) {
            name
            age
        }
    }

    {
        allPersons {
            name
                posts {
                    title
                }
        }
    }









