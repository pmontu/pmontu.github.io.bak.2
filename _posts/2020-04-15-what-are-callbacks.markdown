---
layout: post
title: "What are callbacks?"
date: 2020-04-15 20:50:00 +0530
categories: javascript callback node browser html
---

![javascript promise](https://user-images.githubusercontent.com/1643802/79353934-5d28e700-7f59-11ea-94fe-ecead4ce0dd7.png)

## Spot the difference in order of execution of console.log statements in the below program:

**package.json**

```json
{
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^4.17.1",
    "nodemon": "^2.0.3"
  }
}
```

**server.js**

```javascript
const express = require("express");
const cors = require("cors");
const app = express();
const port = 3000;
// this line tells Express to use the public folder as our static folder from which we can serve static files
app.use(express.static("public"));
// Enable All CORS Requests
app.use(cors());
// to support JSON-encoded bodies
app.use(express.json());
/* Routers */
app.get("/", (req, res) => res.sendFile("index.html"));
app.post("/api/sum", (req, res) =>
  res.json(req.body.numbers.reduce((x, i) => x + i, 0))
);
// Start Server
app.listen(port, () =>
  console.log(`Example app listening at http://localhost:${port}`)
);
console.log("I execute at the end");
```

**public/index.html**

```html
<html>
  <head>
    <script>
      console.log("I execute first");
      fetch("http://localhost:3000/api/sum", {
        method: "POST",
        body: JSON.stringify({ numbers: [1, 2, 3] }),
        headers: { "Content-Type": "application/json" },
      })
        .then((res) => res.json())
        .then((data) => console.log("ridu has downloaded the data", data))
        .catch((e) => console.log("ridu has done something wrong"));
      console.log("I execute last");
    </script>
  </head>
  <body>
    <h1>Hello World!!</h1>
  </body>
</html>
```

- Install Dependencies `yarn`

* Run Server `node server.js`

**Output**

```
I execute at the end
Example app listening at http://localhost:3000
```

Open `http://localhost:3000` in Browser

**Console Output**

```
I execute first
I execute last
ridu has downloaded the data 6
```

Notice that both server and client are asynchronous in nature. Functions passed as Callbacks are executed once a background task gets completed or on an event is triggered. Other statements are executed in sequence to completion. Only async behaviour is unpredictable.

**Explanation of above program and output:**

Node server express app listen executes callback console log message after the last console log statement because listen is asynchronous in nature.

Similarly the browser javascript ajax fetch api is asynchronous in nature and takes time to receive a response. The console log statement inside the fetch api is executed after the response is received. Whilst the last console log statement is displayed synchronously before response is received.

Which is why the order of console statements are a bit ordered differently.

Leave a comment if you guessed it right or share something else that is on your mind.

Further read:
[Yarn](https://classic.yarnpkg.com/en/docs/getting-started) | [Download Node](https://nodejs.org/en/download/) | [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) | [Asynchronous Javascript Docs](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous) | [Browser Javascript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/What_is_JavaScript) | [Javascript Programming](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/First_steps/A_first_splash) | [What is Node](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Express_Nodejs) | [First-class functions](https://developer.mozilla.org/en-US/docs/Glossary/First-class_Function) | [Functional Programming](https://www.geeksforgeeks.org/functional-programming-paradigm/)
