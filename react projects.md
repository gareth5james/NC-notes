## NC News Intro

## Prior Knowledge

- Be able to make http requests in React.
- Be able to plan a React application's component tree.
- Be able to use side effects in React components.

## Learning Objectives

- Understand how to enable CORS (Cross Origin Resource sharing) in an express server.
- Be able to use a separate file for functions making API requests.
- Understand how to use the axios params option.
- Understand how to build reusable React components.

## CORS

You will come across a ['cross-origin resource sharing'](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) (CORS) error message when you try to get data from your back-end API. This is because, by default, `express` blocks any requests made to it from another domain (at this point, probably `http://localhost:3000`).

To allow CORS on your back-end API for NC News:

```bash
$ npm install cors
```

In your `app.js` file, require in the package:

```js
const cors = require('cors');
```

In your `app.js` file, have your app use the `cors` middleware before any of your other middleware or routers.

```js
app.use(cors());
```

Once you have made these changes you will need to commit and push them to take effect in your hosted api.

```sh
$ git add app.js
$ git commit -m 'allow cross origin resource sharing'
$ git push origin main
```

More info found on the [express documentation](https://expressjs.com/en/resources/middleware/cors.html).

## Axios Params

Sometimes the request you want to send may have many different combinations of queries based on user inputs. Instead of trying to programmatically build the query ourselves `axios` provides a way of passing in any possible number of queries and will build up the query string for you.

To pass any extra information to an axios get request, like a body or query, axios takes an optional second argument - a config options object.
Inside this object under the key of params is another object. The key value pairs of this params object declares all the queries that will be appended onto the url.

The following will evaluate to a url string of `/data?id=12345&all_data=true`

```js
axios.get('/data', {
  params: {
    id: 12345,
    all_data: true,
  },
});
```

This is especially useful when dealing with optional queries. If the value of a query is `undefined` then it will be omitted from the query altogether.

In the example below the filterTerm is undefined so will be omitted. The resulting query will then just contain the id, like so `/data?id=12345`

```js
const filterTerm = undefined;

axios.get('/data', {
  params: {
    id: 12345,
    filterTerm,
  },
});
```

## Logged-in user

Having a user be logged-in to your site is a common feature of websites. As the site your are making is designed to demonstrate your abilities and act as a portfolio piece it is reasonable to make some concessions that we would not normally do in the real world.

People coming to your website for the first time will not know what the existing usernames are and asking them to create an account just to view your portfolio may be an unnecessary barrier to entry. Consider having an existing user that can be logged in by default to make the site more user-friendly.

Think about what kind of functionality is tied to the current user,

- Posting a comment: should the user be logged in in order to post?
- Deleting a comment: should the user be able to delete other user's comments?
