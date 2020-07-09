---
title: "How to Return Jwt From an Express Server"
date: 2020-03-22T20:05:59+03:55
draft: false
toc: false
images:
tags:
  - Backend
  - Express
  - NodeJs
  - JWT
---

jsonwebtoken package in npm makes it easy to sign and verify jwt tokens. Let's look at an example by creating a basic express server.

```javascript
npm init -y
npm install express
```

Now install jsonwebtoken with the following command `npm i jsonwebtoken`.  
Let's spin up an express server and see how we can sign and return jwt

```javascript
const express = require("express");
const jwt = require("jsonwebtoken");

const app = express();

//enable json body parsing
app.use(express.json());

const PORT = process.env.PORT || 5000;

app.get("/user", async (req, res) => {
  // before reruning jwt, perform all actions needed to verify or register user

  // create or select what data you want to send in jwt token. For example
  const payload = { user };
  // set up any string as secret
  const secret = "abrakadabra";
  jwt.sign(payload, secret, { expiresIn: "30d" }, (err, token) => {
    if (err) {
      throw err;
    }
    res.json({ token });
  });
});

app.listen(PORT, () => console.log(`Server started on port ${PORT}`));
```

expiresIn defines how long our token will be valid.
