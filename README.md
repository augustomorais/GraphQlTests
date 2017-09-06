# GraphQlTests
Graphql implementation


After clonning the project you must the the following commands.

* npm install 
* npm install graphql --save

Writing Code 
To handle GraphQL queries, we need a schema that defines the Query type, and we need an API root with a function called a “resolver” for each API endpoint. For an API that just returns “Hello world!”, we can put this code in a file named server.js:

var { graphql, buildSchema } = require('graphql');

// Construct a schema, using GraphQL schema language
var schema = buildSchema(`
  type Query {
    hello: String
  }
`);

// The root provides a resolver function for each API endpoint
var root = {
  hello: () => {
    return 'Hello world!';
  },
};

// Run the GraphQL query '{ hello }' and print out the response
graphql(schema, '{ hello }', root).then((response) => {
  console.log(response);
});
If you run this with:

node server.js
You should see the GraphQL response printed out:

{ data: { hello: 'Hello world!' } }
Congratulations - you just executed a GraphQL query!

For practical applications, you'll probably want to run GraphQL queries from an API server, rather than executing GraphQL with a command line tool. To use GraphQL for an API server over HTTP, check out Running an Express GraphQL Server.


Running an Express GraphQL Server
The simplest way to run an GraphQL API server is to use Express, a popular web application framework for Node.js. You will need to install two additional dependencies:

npm install express express-graphql graphql --save
Let's modify our “hello world” example so that it's an API server rather than a script that runs a single query. We can use the 'express' module to run a webserver, and instead of executing a query directly with the graphql function, we can use the express-graphql library to mount a GraphQL API server on the “/graphql” HTTP endpoint:

var express = require('express');
var graphqlHTTP = require('express-graphql');
var { buildSchema } = require('graphql');

// Construct a schema, using GraphQL schema language
var schema = buildSchema(`
  type Query {
    hello: String
  }
`);

// The root provides a resolver function for each API endpoint
var root = {
  hello: () => {
    return 'Hello world!';
  },
};

var app = express();
app.use('/graphql', graphqlHTTP({
  schema: schema,
  rootValue: root,
  graphiql: true,
}));
app.listen(4000);
console.log('Running a GraphQL API server at localhost:4000/graphql');
You can run this GraphQL server with:

node server.js
Since we configured graphqlHTTP with graphiql: true, you can use the GraphiQL tool to manually issue GraphQL queries. If you navigate in a web browser to http://localhost:4000/graphql, you should see an interface that lets you enter queries. It should look like:

hello world graphql example

This screen shot shows the GraphQL query { hello } being issued and giving a result of { data: { hello: 'Hello world!' } }. GraphiQL is a great tool for debugging and inspecting a server, so we recommend running it whenever your application is in development mode.

At this point you have learned how to run a GraphQL server and how to use GraphiQL interface to issue queries. The next step is to learn how to issue GraphQL queries from client code.
