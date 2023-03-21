# GRAPHQL-NOJEJS

Start apollo server:
```typescript
const { ApolloServer } = require('apollo-server');

const typeDefs = 
    `type Query {
        info: String!
    }`;

const resolvers = {
    Query: {
        info: () => `LaconiQL is a ready-to-use minimalistic NodeJS-GraphQL server powered by Apollo-server and Prisma with PostgreSQL.`
    }
};

const server = new ApolloServer({
    typeDefs,
    resolvers,
});

server.listen().then(({ url }) => console.log(`Server is running on ${url}`));
```