# React Router

## Prior Knowledge

- Understand the parent-child relationship of html elements.
- Understand that React is a Single Page Application (SPA).
- Understand the lifecycle of `useEffect` hooks.
- Understand how html documents are served on different routes using server-side-rendering.

## Learning objectives

- Understand how React can emulate different pages using conditional rendering.
- Understand how the React Router library implements this functionality.
- Understand how to plan React applications with several routes.

## Single page applications vs multi-page applications

React applications are generally **single page applications** (SPAs) - as all the JavaScript is sent (though not necessarily all the data) required to construct the whole website, instead of individual HTML pages. Essentially, it does not matter what the path of the URL is on an SPA, as everything is sent on the home path anyway. There are advantages to this, but there are two major drawbacks to losing this routing behaviour:

- Different areas of our application are not individually addressable (preventing, for example, sharing links)
- Losing use of the browser's history (back and forward navigation)

**React Router** is a library that allows **client-side routing**, as opposed to **server-side routing**:

### Server-side routing

- The server has views for every single route of our app.
- User navigates to `/about`, the browser sends a GET request to `/about` and our server responds with the corresponding view.

### Client-side routing

- In React applications, the server provides a single HTML file (with an empty div) and a bundle of JavaScript. The rendering of the application happens client-side.
- Because all the views of our application are already in the browser, we don't need to make GET requests to get different views.
- We can use a router, a library that catches the changes in the URL and renders different components accordingly.
- HTTP requests still happen in the background, but not for displaying views, just for getting or sending data to servers.
- The page never reloads, components come and go from the DOM, giving the illusion of navigation.

## Using React Router

[React router](https://reactrouter.com/) can be installed via npm

```bash
npm i react-router-dom
```

### BrowserRouter

React router can be used in the browser or native applications. As we are making web apps we will use the `BrowserRouter` component. This component must be wrapped around our entire App. We can then make use of other components from React router to render our components on specific paths.

```js
import { BrowserRouter } from 'react-router-dom';

const App = () => {
  return (
    <BrowserRouter>
      <div className="App">{/* Rest of App */}</div>
    </BrowserRouter>
  );
};
```

### Routes and Route

A `Routes` component takes a number of `Route` components as children. It will render the `Route` that matches the current path.

A Route component takes a prop of `path` which will be compared to the current url.

```js
import { BrowserRouter, Routes, Route } from 'react-router-dom';

const App = () => {
  return (
    <BrowserRouter>
      <div className="App">
        <h1>My App </h1>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/topics" element={<Topics />} />
          <Route path="/about" element={<About />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
};

// src/components/Home
const Home = () => (
  <div>
    <h2>Home</h2>
  </div>
);

// src/components/About
const About = () => (
  <div>
    <h2>About</h2>
  </div>
);

// src/components/Topics
const Topics = () => (
  <div>
    <h2>Topics</h2>
  </div>
);
```

### Links

Though typing in a new URL will take you to the correct place if you have routing set up, you will still be reloading the entire application, which defeats one of the purposes of React and single page applications. **Anchor tags** (`<a />`) are normally used for linking between pages, but the default behaviour of these is also to load a new page.

React router provides a useful `<Link>` component that removes this behaviour for us. It takes a `to` property that will set the url. It could be useful in a `NavBar` component, for example:

```js
<nav>
  <Link to="/">Home</Link>
  <Link to="/about">About</Link>
  <Link to="/topics">Topics</Link>
</nav>
```

**nb** Just as with `a` tags Links can take more than just text as children. You can wrap a Link component around any number of children.

```js
<ul>
  <li>
    <Link to="/topics/1">
      <h2>Topic Id 1</h2>
      <img src="http://example.com/topic1.jpg" alt="topic1" />
      <p>Topic 1's tag line</p>
    </Link>
  </li>
</ul>
```

### Parametric endpoints - useParams

Just as when writing servers we can declare routes to include parametric endpoints. The syntax for a parametric endpoint is the same as in express, we use a ':' in our path, followed by the parameters name. e.g. `path='/topics/:topic_id'`

React router will parse the current url and extract the values of any params for us. These are accessible on the [match.params] prop from our Route components. Passing these props around can be overly complicated and as we are taking advantage of React hooks we can use the provided [useParams hook](https://reactrouter.com/docs/en/v6/api#useparams) to access the values of our params.

```js
import { BrowserRouter, Routes, Route, useParams } from 'react-router-dom';

const App = () => {
  return (
    <BrowserRouter>
      <div className="App">
        <h1>My App </h1>
        <Routes>
          <Route path="/topics" element={<Topics />} />
          <Route path="/topics/:topic_slug" element={<SingleTopic />} />
        </Routes>
      </div>
    </BrowserRouter>
  );
};

// src/components/Topics
const Topics = () => (
  <div>
    <h2>Topics</h2>
  </div>
);

// src/components/SingleTopic
const SingleTopic = () => {
  const { topic_slug } = useParams(); // useParams returns an object containing the url params - { topic_slug: 'url_value' }
  return (
    <div>
      <h2>Rendering {topic_slug}</h2>
    </div>
  );
};
```

A common pattern is to use the value from the params to fetch data from an api. In the above example we could use the topic_slug to fetch the rest of the data from an api. If the topic_slug is changed, by the user navigating to a new topic for example, the component will be re-rendered with the new topic_slug. (If your interested in how to implement this kind of functionality it is covered in the context api and custom hooks sections of these notes.) We should bear this in mind when writing our side effects and make sure that our dependencies are correct. In this case, if the topic_slug changes, re-run the effect and fetch new data.

```js
import { fetchTopicBySlug } from '../utils/api';

const SingleTopic = () => {
  const [topic, setTopic] = useState({});
  const { topic_slug } = useParams();

  useEffect(() => {
    fetchTopicBySlug(topic_slug).then((topicData) => {
      setTopic(topicData);
    });
  }, [topic_slug]);

  return (
    <div>
      <h2>Rendering {topic_slug}</h2>
    </div>
  );
};

// ../utils/api
import axios from 'axios';

const myApi = axios.create({
  baseURL: 'https://myExampleServer.com/api',
});

export const fetchTopicBySlug = (topic_slug) => {
  return myApi.get(`/topics/${topic_slug}`).then((res) => {
    return res.data.topic;
  });
};
```

The functionality of making an api call is not "component specific" as it isn't tied to any one component and its implementation. It is a good idea to separate our concerns and define the logic of making these calls in their own module (a different file) so that they can be reused, mocked out or refactored easily.

These are some of the core features of React Router and the docs offer much more with excellent examples and guides on how to implement common routing techniques such as nested routing, styling active links, redirects and much more. See the [docs](https://reactrouter.com/) for more info.

### Navigating Programmatically

`react-router-dom` gives access to a [useNavigate](https://reactrouter.com/docs/en/v6/api#usenavigate) hook which returns a function that lets you navigate programmatically, for example after a form is submitted.

The navigate function has two different argument types:

- A string, which represents the "to" value (the same as with `<Link to="/path">`)
- A number, which represents how many steps forwards (positive numbers) or backwards (negative numbers) you'd like to go in the browser's history (e.g. `navigate(-1)` is the same as clicking the back button)

> Note: This is not a replacement for `<Link>` components!

e.g.

```js
import { useNavigate } from 'react-router-dom';

function MyComponent() {
  const navigate = useNavigate();

  function handleSubmit(e) {
    e.preventDefault();
    doSomething()
      .then(() => {
        navigate('/somewhere/else');
      })
      .catch((err) => {
        // handle error
      });
  }

  return (
    <form onSubmit={handleSubmit}>
      {/* labels, inputs and form things... */}
    </form>
  );
}
```
