# GRAPHQL

## OVERVIEW

GraphQL enables declarative data fetching where a client can specify exactly what data it needs from an API.

Instead of multiple endpoints that return fixed data structures, a GraphQL server only exposes a single endpoint and responds with precisely the data that a client asked for.

GraphQL minimizes the amount of data that needs to be transferred over the network, thus increasing performance.

We could write our REST APIs in a way appropriate to the screen to return only the info needed, but product iterations would be slower. i.e. every time the design changes, the APIs might need to be re-written/updated to solve those particular needs. 

With graphQL, we have single endpoint. You actually only send a single request to the server and the server would respond with all the information that you need.

The way how this works is by sending a post request to the server where you include a query in the body of that request that describes all the data requirements of the client.

Instrumenting and measuring performance of resolvers provides crucial insights about bottlenecks in your system.

GraphQL offers strong type capabilities, owing to it's schema system.

The schema allows independednt FE and BE development since both are aware of the structure of data to be returned.

## CONCEPTS

#### Queries:

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

#### Mutations: 
Making changes to data.

    mutation {
        createPerson(name: "Bob", age: "20") {
            name
            age
        }
    }

#### Subscriptions: 
Real time updates with subscriptions: when a client subscribes to an event, it will initiate and hold a steady connection to the server.

    subscription {
        newPerson {
            name
            age
        }
    }

In this above example, the client subscribes on the server to get informed about new users being created. Whenever that particular event then actually happens, the server pushes the corresponding data to the client. In this example this is the name and the age of the new user since that's the information that's specified in the subscription payload.

#### Schema definition (SDL = Schema Definition Language):

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
        updatedPerson: Person!
        deletedPerson: Person!
        newPost: Post!
        updatedPost: Post!
        deletedPost: Post!
    }

It's important to note that GraphQL is actually transport-layer agnostic.  
This means it can potentially be used with any available network protocol.  
So, it is definitely possible to implement a GraphQL server based on TCP, WebSockets, or any other transport.  

GraphQL can act as an integration laer between various legacy systems, microservices and third party APIs, or/and it can be it's own server connected to a database.  

#### Resolvers:  
The payload of a GraphQL query or mutation consists of a set of fields that we want the server to return.
In the GraphQL server implementation each of these fields actually corresponds to exactly one function that's called a *resolver*.
The sole purpose of a *resolver* function is to fetch the data for its corresponding field.

    query {
        User(id: 'abcd') {
            name
            friends(first : 5) {
                name
                age
            }
        }
    }

    // Resolvers:
    User(id: String!) User
    name(user: User!): String!
    age(user: User!): Int!
    friends(first: Int, user: User!): [User!]!

## CLIENTS

A major benefit of GraphQL is that it allows you to fetch and update data in a declarative manner. Put differently, we climb up one step higher on the API abstraction ladder and don’t have to deal with low-level networking tasks ourselves anymore.

Rather than using plain HTTP (like fetch in Javascript) to load data from an API, all you need to do with GraphQL is write a query to declare your data requirements and let the system take care of sending the request and handling the response for you. This is precisely what a GraphQL client will do.  

Once the server response is received and handled by the GraphQL client, the requested data somehow needs to end up in your UI.  
Taking React as an example, GraphQL clients use the concept of *higher-order components* to fetch the needed data under the hood and make it available in the props of your components.

[Caching graphQl query results for consumption](https://www.apollographql.com/blog/the-concepts-of-graphql-bc68bd819be3/)  

Since the schema contains all information about what a client can potentially do with a GraphQL API, there is a great opportunity to validate and potentially optimize the queries that a client wants to send at build-time.

GraphQL allows you to have UI code and data requirements side-by-side. The tight coupling of views and their data dependencies greatly improves the developer experience. The mental overhead of thinking about how the right data ends up in the right parts of the UI is eliminated.

## MORE CONCEPTS

#### Fragments:
Fragments are a handy feature that improves the structure and reusability of your GraphQL code. A fragment is a collection of fields on a specific type.

For example, let’s assume we have the following type:

    type User {
        name: String!
        age: Int!
        email: String!
        street: String!
        zipcode: String!
        city: String!
    }

Here, we could represent all the information that relates to the user’s physical address into a fragment:

    fragment addressDetails on User {
        name
        street
        zipcode
        city
    }

Now, when writing a query to access the address information of a user, we can use the following syntax to refer to the fragment and save the work to actually spell out the four fields:

    {
        allUsers {
            ... addressDetails
        }
    }

This query is equivalent to writing:

    {
        allUsers {
            name
            street
            zipcode
            city
        }
    }

Fragments are great when you need to reuse different fields on a query.

#### Parameters:
In GraphQL type definitions, each field can take zero or more arguments. Similar to arguments that are passed into functions in typed programming languages, each argument needs to have a name and a type. In GraphQL, it’s also possible to specify default values for arguments.

As an example:

    type Query {
        allUsers: [User!]!
    }

    type User {
        name: String!
        age: Int!
    }

We could now add an argument to the allUsers field that allows us to pass an argument to filter users and include only those above a certain age. We also specify a default value so that by default all users will be returned:

    type Query {
        allUsers(olderThan: Int = -1): [User!]!
    }

This olderThan argument can now be passed into the query using the following syntax:

    {
        allUsers(olderThan: 30) {
            name
            age
        }
    }

#### Aliases:

One of GraphQL’s major strengths is that it lets you send multiple queries in a single request. However, since the response data is shaped after the structure of the fields being requested, you might run into naming issues when you’re sending multiple queries asking for the same fields:

    {
        User(id: "1") {
            name
    }
        User(id: "2") {
            name
        }
    }

In fact, this will produce an error with a GraphQL server, since it’s the same field but different arguments. The only way to send a query like that would be to use aliases, i.e. specifying names for the query results:

    {
        first: User(id: "1") {
            name
        }
        second: User(id: "2") {
            name
        }
    }

In the result, the server would now name each User object according to the specified alias:

    {
        "first": {
            "name": "Alice"
        },
        "second": {
            "name": "Sarah"
        }
    }

#### More on SDL:

**Object and Scalar Types:**  
Scalar Types:  
They represent concrete units of data. The GraphQL spec has five predefined scalars as `String, Int, Float, Boolean, and ID`.  

Object Types:  
They have fields that express the properties of that type and are composable. 
You can define your own scalar and object types.  

**Enumerations (Enums) Types:**  
GraphQL allows you to define enumerations types (short enums), a language feature to express the semantics of a type that has a fixed set of values. We could thus define a type called Weekday to represent all the days of a week:

    enum Weekday {
        MONDAY
        TUESDAY
        WEDNESDAY
        THURSDAY
        FRIDAY
        SATURDAY
        SUNDAY
    }

**Interface Type:**  
An interface can be used to describe a type in an abstract way. It allows you to specify a set of fields that any concrete types which implement this interface need to have.

    interface Node {
        id: ID!
    }

    type User implements Node {
        id: ID!
        name: String!
        age: Int!
    }

**Union Types:** 
Union types can be used to express that a type should be either of a collection of other types. Let’s consider the following types:

    type Adult {
        name: String!
        work: String!
    }

    type Child {
        name: String!
        school: String!
    }

Now, we could define a Person type to be the union of Adult and Child:

    union Person = Adult | Child

This brings up a different problem. In a GraphQL query where we ask to retrieve information about a Child but only have a Person type to work with, how do we know whether we can actually access this field?  
The answer to this is called conditional fragments:

    {
        allPersons {
            name 
            ... on Child {
                school
            }
            ... on Adult {
                work
            }
        }
    }

## TOOLING

### Introspection:

Allows client to get schema information. We can ask GraphQL for this information by querying the __schema meta-field, which is always available on the root type of a query per the spec.  

Say we have the following SDL:

    type Query {
        author(id: ID!): Author
    }

    type Author {
        posts: [Post!]!
    }

    type Post {
        title: String!
    }

This query:

    query {
        __schema {
            types {
                name
            }
        }
    }

Will return:

    {
        "data": {
            "__schema": {
                "types": [
                    {
                        "name": "Query"
                    },
                    {
                        "name": "Author"
                    },
                    {
                        "name": "Post"
                    },
                    {
                        "name": "ID"
                    },
                    {
                        "name": "String"
                    },
                    {
                        "name": "__Schema"
                    },
                    {
                        "name": "__Type"
                    },
                    {
                        "name": "__TypeKind"
                    },
                    {
                        "name": "__Field"
                    },
                    {
                        "name": "__InputValue"
                    },
                    {
                        "name": "__EnumValue"
                    },
                    {
                        "name": "__Directive"
                    },
                    {
                        "name": "__DirectiveLocation"
                    }
                ]
            }
        }
    }

Another example:

    {
        __type(name: "Author") {
            name
            description
        }
    }

In this example, we query a single type using the __type meta-field and we ask for its name and description. Here’s the result for this query:

    {
        "data": {
            "__type": {
                "name": "Author",
                "description": "The author of a post.",
            }
        }
    }

## SECURITY

- Authenticated introspection
- Timeout
- Max query depth
- Max query complexity
- Throttling: stop clients from requesting resources too often.
    - based on server-time-per-second value that a client has access to
    - based on query complexity value that a client has access to
