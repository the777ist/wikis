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

-Queries:

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

-Mutations: Making changes to data.

    mutation {
        createPerson(name: "Bob", age: "20") {
            name
            age
        }
    }

-Subscriptions: Real time updates with subscriptions: when a client subscribes to an event, it will initiate and hold a steady connection to the server.

    subscription {
        newPerson {
            name
            age
        }
    }

-In this above example, the client subscribes on the server to get informed about new users being created. Whenever that particular event then actually happens, the server pushes the corresponding data to the client. In this example this is the name and the age of the new user since that's the information that's specified in the subscription payload.

-Schema definition for CRUD:

    type Query {
        allPersons(<OPTIONAL_ARGS>): [Person!]!
        allPosts(<OPTIONAL_ARGS>): [Post!]!
    }

    type Mutation {
        createPerson(name: String!, age: String!): Person!
        updatePerson(id: ID!, name: String!, age: String!): Person!
        deletePerson(id: ID!): Person!

        createPost(title: String!): Post!  
        updatePost(id: ID!, title: String!): Post!
        deletePost(id: ID!): Post!  
    }

    type Subscription {
        newPerson: Person!
    }

    type Person {
        id: ID!
        name: String!
        age: Int
        posts: [Post!]!
    }

    type Post {
        id: ID!
        title: String!
        author: Person! 
    }






