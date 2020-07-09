---
title: "Api Routes With Next Js"
date: 2020-03-13T20:34:12+08:21
draft: false
toc: false
images:
tags:
  - NextJs
  - React
---

Next.Js has made it really easy to work with simple API routes. That means we no longer need to set up any kind of backend server to interact with databases and handle client-side requests. Today we are going to create an API route that can respond to client-side requests.

- First, create a new Next.Js project with create-next-app. Just run `npm init next-app`
- Next, create a new file at `./pages/api/users.js`. Any files created under `pages/api` are treated as API endpoints.
- Navigate to the newly created file and paste the following lines of code:

```javascript
export default (req, res) => {
  if (req.method === "GET") {
    res.status(200).send("GET Request received");
  } else if (req.method === "POST") {
    res.status(201).send("POST Request received");
  } else if (req.method === "DELETE") {
    res.status(202).send("DELETE Request received");
  }
};
```

Now run `npm run dev` to start the development server. If you use a tool like Postman to send requests to http://localhost:5000/api/users you'll find that you have created a new API endpoint that can respond to GET, POST and DELETE Requests.
