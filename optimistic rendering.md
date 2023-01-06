# Optimistic Rendering

## Prior Knowledge

- Be able to perform http requests in React.
- Be able to catch errors from http requests.
- Be able to update React state accordingly.

## Learning Objectives

- Understand optimistic rendering entails updating the UI before the back end processing has finished.
- Be able to perform state updates optimistically.
- Understand when it is appropriate to perform optimistic rendering.
- Understand how to give appropriate feedback if a request fails.
- Understand how to align users expectations with UI updates.

## Optimistic rendering

Optimistic rendering is the technique of rendering UI updates without confirmation of success from the back end based on the assumption that the request will succeed. There are several requests to a server that won't require any kind of server side validation and will be successful the vast majority of the time. Think of features such as clicking a like button or up-voting a users post. Facebook's servers will process a huge number of requests to like posts with almost all of them being successful.

No matter how fast the servers are however it will take some amount of time to process the request. In order to improve our UX we can assume that the request will succeed and give our user immediate feedback that their request has succeeded whilst it is processed in the background. When the request is eventually successful, we have no need to perform further updates, so can stay quiet.

It should be clear that this approach should be avoided in some circumstances - financial transactions, for examples, should never give the indication that something has happened when it hasn't, or vice versa. But there may be advantages in some cases to adopting the optimistic rendering approach.

## Non-optimistic Approach

Here is a non-optimistically rendered example of a component showing how many messages have been sent:

```js
const MessageStats = () => {
  const [sentMessageCount, setSentMessageCount] = useState(0);

  useEffect(() => {
    api.getSentMessageCount().then((msgCount) => {
      setSentMessageCount(msgCount);
    });
  }, []);

  const handleSendMessageClick = () => {
    api.sendMessage().then((msgCount) => {
      setSentMessageCount(msgCount);
    });
  };

  return (
    // component html/css, including...
    <button onClick={handleSendMessageClick}>Send message</button>
  );
};
```

In this example, the `sentMessageCount` value is updated on two occasions:

- When the component is first rendered via `useEffect`
- When the user sends a message, via the `sendMessage` api function.

When we set the state after the user sends a message, we will be using the latest data from the database. However, if this were a 'real world' application, with many simultaneous users, the latest `sentMessageCount` may have jumped considerably in the time between the component mounting and the messages being sent.

## Optimistic approach

At this point, as developers, we need to decide what we are trying to present here: a factual report of how many messages have been sent at that moment, or the impression that a user has had an impact by pressing the button. If it is the latter, then seeing the number increment by one might be more effective. This is demonstrated below:

```js
const handleSendMessageClick = () => {
  api.sendMessage().then((msgCount) => {
    setSentMessageCount((currCount) => currCount + 1);
  });
};
```

The new `sentMessageCount` here will not be an accurate representation of the state of our database, but may feel more natural to user.

If we are to adopt this approach, then we should consider if we should be waiting for the response from the api in the first place. We are not using any of the data that is being returned, so we can consider an alternative:

```js
const handleSendMessageClick = () => {
  setSentMessageCount((currCount) => currCount + 1);
  api.sendMessage();
};
```

This way we can give our users immediate feedback seeing the message count increase immediately whilst the api call is processed in the background.

### Error handling

If we are going to assume success then we can succeed silently. However there is always a chance of things going wrong, even if it's something like the network connection cutting out. We will need to handle these errors and give our users appropriate feedback. It may be enough to simply undo the change you made so that the user can try again or you may need to keep some err state to give your user more detailed feedback.

```js
const MessageStats = () => {
  const [sentMessageCount, setSentMessageCount] = useState(0);
  const [err, setErr] = useState(null);

  useEffect(() => {
    api.getSentMessageCount().then((msgCount) => {
      setSentMessageCount(msgCount);
    });
  }, []);

  const handleSendMessageClick = () => {
    setSentMessageCount((currCount) => currCount + 1);
    setErr(null);
    api.sendMessage().catch((err) => {
      setSentMessageCount((currCount) => currCount - 1);
      setErr('Something went wrong, please try again.');
    });
  };

  if (err) return <p>{err}</p>;
  return (
    // component html/css, including...
    <button onClick={handleSendMessageClick}>Send message</button>
  );
};
```

To summarise the differences between these approaches:

| **Optimistic rendering**                                   | **Rendering on response**                                        |
| ---------------------------------------------------------- | ---------------------------------------------------------------- |
| Both `setState` and the API request trigger simultaneously | `setState` only happens after the API response                   |
| Cannot use data from the API response                      | Data from the API response is available                          |
| Both actions will always trigger                           | `setState` will only trigger if the API response is not an error |
| Cannot accurately represent the database                   | Can accurately represent the database at the time of response    |

It is of course possible to use elements of both approaches. You may wish to have some effect immediately on triggering the handler - for example, disabling a button to prevent multiple requests from being made. You can also undo state changes should a request fail. If so, you will need to think carefully how to achieve this gracefully and without disorienting a user.
