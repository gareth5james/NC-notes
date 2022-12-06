# Supertest

## Learning Objectives

- Understand how integration testing differs from unit testing
- Understand how to use [supertest](https://www.npmjs.com/package/supertest) to test end-points
- Be able to use `dotenv` to organise environment variables
- Reasons for separate test and development databases
- Connect to different PostgreSQL databases programmatically

## Integration Testing

Up to this point, the vast majority of our tests have been **unit tests** - testing individual functions and methods on a granular level.

Another type of testing is **integration testing**. Unlike unit testing which tests individual functions, integration testing is oriented towards checking that multiple parts of a project work cohesively together to successfully achieve their goal.

This is useful when developing a server, where each endpoint will require the controllers, models and database to work together to respond with the correct data. An **integration test** here could make a request to the server and test the response is correct, meaning that all the distinct moving parts have worked in the intended way.

> Because testing an endpoint involves testing the flow from a user request all the way to the database, it could be argued that there is overlap here with **End-To-End Testing**. Also known as 'E2E Testing', this refers to tests that check an entire user journey.

## Using `supertest`

`supertest` is an `npm` package that allows us to run tests against our server endpoints - allowing us to make test requests and check the response is what we expect.

### `supertest` Setup

> **"Tests must ALWAYS be written first"** - _most good back-end developers, ever_

`supertest` should be installed as a _dev-dependency_ (`npm i -D supertest`), as it is only used for testing, and will not be required for production code - i.e. when the api server is hosted.

`supertest` should be required in at the top of the relevant `test` file as `request`, along with the express `app` that is being tested.

To make a test request to the app, the `request` variable (supertest) is passed the `app` as an argument within the jest `test` block - if the server is not already listening for connections then it is bound to a temporary port, so there is no need to keep track of ports.

The HTTP method is then chained on to `request` and invoked with the relevant `path`. Because the request is asynchronous, we need to make sure to `return` it out of the function passed to `it` - otherwise this test will always pass!
E.g:

```js
describe('GET /api/items', () => {
  it('responds with object containing all items', () => {
    return request(app).get('/api/items');
  });
});
```

Some requests will need to send a `body` to the endpoint, such as POST and PATCH. `supertest` allows this as another chainable method to the request.

```js
// POST request
return request(app).post('/api/items').send(newItem);
```

`supertest` then has some built in assertions than can be chained on to the request to check the response's HTTP `status` code, as well as any header `field`s and `value`s. Note that one of these chainable methods is called `expect` - this is different to the `expect` from `jest` and is an inbuilt feature to `supertest` that allows us to neatly assert the values of some properties on the response object, such as status code and `Content-Type`.

```js
return request(app)
  .get('/api/items')
  .expect(200)
  .expect('Content-Type', 'application/json');
```

To make our own assertions about the aspects of the response, e.g. the data on the `body` property, we can make use of the fact that the above returns a promise.

We can chain a `.then()` to access the response from the server and, within this `.then()` block, can make additional assertions using `jest` as to what we `expect` the body of the response to contain.

```js
return request(app)
  .get('/api/items')
  .expect(200)
  .then((res) => {
    expect(res.body.items.length).toBe(4);
  });
```

## Before and After Tests

### Seeding before tests run

Each test that is run will be interacting with the test database and potentially changing the data that is stored. Consider the consequences of testing a GET /items endpoint and then a POST /items endpoint; because the POST request would actually be adding data to the database, the next time the test suite is run and the GET /items endpoint is tested, there will be more items in the database than were originally seeded.

To reduce this unreliability, we can re-seed the database before we run the test suite. One way to do this is to update the `test` script in the `package.json`. We can specify that, before `jest` is run, the `.sql` file(s) responsible for dropping/recreating the database and inserting the data is run first.

```json
{
  "scripts": {
    "test": "psql -f setup-db.sql && jest"
  }
}
```

The `&&` in the script allows us to chain commands - it will run the second command (`jest`) if the first command succeeded with no errors.

This helps to reduce problems between each run of the full test suite. However, it is important to note that _between each test_ we may have the same problems.

Imagine that one test updates the data and a later test needs to access that data. If the later test does not account for the updates to the data and only relies on how the data looked at point of seeding, then it will fail whenever all of the tests are run. Conversely, if the later test accessing the data _does_ take into account the potential updates from the earlier tests, then it will fail as soon as a `.only` is added.

This is an issue that we should be concious of and will soon learn how to solve.

### Ending connection

Once all of the tests are run, the connection to the pool must be actively closed. This can be done neatly within an `afterAll` block in our test suite - `afterAll` takes a function, within which we can invoke the `.end` method on the connection to the database required in at the top of the file.

```js
const connection = require('../db/connection.js');

afterAll(() => connection.end());
```

## Setting environment variables

`node-postgres` finds which database to connect through from the environment variables. Until now, we have set the name of the database manually in our `package.json` scripts (`PGDATABASE=name-of-database-here`). As is often the case in JavaScript, there is an `npm package` that can help us to do this.

`dotenv` is an [npm package](https://www.npmjs.com/package/dotenv) that handles the configuration of environment variables. Once it is installed, we can create a file called `.env` and store our variables in there. An important benefit of this is that we can `gitignore` this file so that we do not push any sensitive information about our database and/or postgres passwords up to github.

```txt
PGDATABASE=your-database-name-here;
```

Then, in the /db file that we set up our connection to the database we can require `dotenv` into the file and invoke its `config` method. This sets all of the environment variables from the `.env` file to the `process.env`.

```js
// in our db file where we set up our connection to the database
require('dotenv').config();
const { Pool } = require('pg');

module.exports = new Pool();
```

Note that if, for some reason, the PGDATABASE environment variable was not set correctly, `node-postgres` would connect to your default database rather than throwing an error. This could cause lots of issues if not spotted early, and so it is advisable to add a condition that checks the `process.env.PGDATABASE` variable exists and throws an error if missing.

```js
if (!process.env.PGDATABASE) {
  throw new Error('No PGDATABASE configured');
}
```

## Separate `test` and `development` databases

Running tests can cause updates to the data within a database, which can make the state of the data in the database unpredictable across tests. Re-seeding the database before running the test suite allows the data used for the tests to be more predictable and consistent.

However, if the same database is used for user interaction with the application (for example using a browser or Insomnia), the user would experience unexpected changes to the data when the test database was re-seeded, or if the tests made changes to the data. This is why it is beneficial to have a separate database for running test scripts that can be regularly re-seeded, and another for manual use of the application where any changes made by a user can persist.

An added benefit of this is that the test data can be curated by the developers to be a smaller, more manageable subset of the development data. This means that test data can be re-seeded quicker and is easier to update and write tests for.

## Connecting to different PostgreSQL databases

In order to use `node-postgres` to connect to different databases, the `db/index.js` file needs to be configured to programmatically change which database we are connecting to based on the `process.env.NODE_ENV`.

Up until now, we have stored the database name in a `.env` file and used the `dotenv` package to read the this file and set the environment variable of `PGDATABASE` to the `process.env`. Now that we have different databases to connect to depending on the environment ('development' vs 'test'), we will need multiple .env files.

In `.env.test`

```
PGDATABASE=test_database_name
```

In `.env.development`

```
PGDATABASE=development_database_name
```

In the `db/index.js` file itself, we need to configure `dotenv` so that it can find the correct file depending on our node environment (`process.env.NODE_ENV`). If the `process.env.NODE_ENV` is `test`, then `dotenv` needs to get the variables from the `.env.test` file, and if the `NODE_ENV` is `development` then `.env.development`.

As a default, `dotenv` will look for a file simply called `.env`. If the filename is something different, as in our case, we can pass an object (which specifies the path to the correct file) into the `config` method on the required-in `dotenv` module.

```js
const { Pool } = require('pg');

require('dotenv').config({
  path: pathToCorrectEnvFileHere,
});

module.exports = new Pool();
```

To find the path to the correct `.env` file, we have to a) work out the name of the file, and b) create an absolute path to that file to pass into the config object.

To find the name for the file, the naming convention for our `.env.test` and `.env.development` means we can append the `process.env.NODE_ENV` to `.env`.

Creating the absolute path to this file is slightly more work-intensive. This is because there is a disconnect between the relative path from the `db/index` file to the correct `.env` file, versus where the script that runs the project will actually be executed (likely in the root of the project).

The below example uses the globally available [`__dirname`](https://nodejs.org/api/modules.html#modules_dirname) to create an absolute path to the correct `.env` file. This allows the file to be run from anywhere within the project directory structure and maintain an absolute path to the correct `.env` file.

```js
// /Users/username/project-folder/db/index.js
const { Pool } = require('pg');
const ENV = process.env.NODE_ENV || 'development';

const pathToCorrectEnvFile = `${__dirname}/../.env.${ENV}`;

console.log(__dirname); // /Users/username/project-folder/db
console.log(pathToCorrectEnvFile); // /Users/username/project-folder/.env.development

require('dotenv').config({
  path: pathToCorrectEnvFile,
});

module.exports = new Pool();
```

Remember that if the PGDATABASE environment variable is not set, we will be connected to our _default_ database which could cause issues, so it is still advisable to add a condition that checks the `process.env.PGDATABASE` variable exists and throws an error if missing.
