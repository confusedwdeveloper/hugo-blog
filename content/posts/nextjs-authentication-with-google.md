---
title: "Next.js Authentication With Google"
date: 2020-07-12T19:27:24+05:30
draft: false
toc: true
images:
tags:
  - NextJs
  - React
  - Authentication
---

### create new Next.js app

Let's start things off by creating a new Next.js app with the following command:

```javascript
npx create-next-app my-app
```

Now install any packages you might need. For authentication we're going to use a package called next-auth. It makes it easy to set up OAuth authentication with your providers of choice. You can find the documentations [here](https://next-auth.js.org/).  
Before we jump into code, you can generate your client ID and client secret [here](https://console.developers.google.com/apis/credentials).

### Create and modify \_app.js

You can pretty much go this example repo [link](https://github.com/iaincollins/next-auth-example) and copy paste the \_app.js file. Using the supplied React `<Provider>` allows instances of useSession() to share the session object across components, by using React Context under the hood. This is not only required if you want to use client methods inside `getServerSideProps` but also improves performance.

### Configure your local environment variables

On the root of your file create a `.env.local` file that looks like this. Note that you may need to install additional packages if you choose to use a database. For example I need to install `mongodb` since I am using MongoDB Atlas as database.

```javascript
NEXTAUTH_URL=http://localhost:3000
GOOGLE_ID=Your google ID
GOOGLE_SECRET=Your google secret
DATABASE_URL=Your database url (not mandatory)
```

### Setting up API

You can pretty much copy paste the `[...nextauth].js` file in pages/api/auth and make the necessary changes as required. For example, I just removed other providers except google.

```javascript
import NextAuth from "next-auth";
import Providers from "next-auth/providers";

const options = {
  providers: [
    Providers.Google({
      clientId: process.env.GOOGLE_ID,
      clientSecret: process.env.GOOGLE_SECRET,
    }),
  ],
  database: process.env.DATABASE_URL,

  // The secret should be set to a reasonably long random string.
  // It is used to sign cookies and to sign and encrypt JSON Web Tokens, unless
  // a seperate secret is defined explicitly for encrypting the JWT.
  secret: process.env.SECRET,

  session: {
    // Use JSON Web Tokens for session instead of database sessions.
    // This option can be used with or without a database for users/accounts.
    // Note: `jwt` is automatically set to `true` if no database is specified.
    jwt: true,

    // Seconds - How long until an idle session expires and is no longer valid.
    // maxAge: 30 * 24 * 60 * 60, // 30 days

    // Seconds - Throttle how frequently to write to database to extend a session.
    // Use it to limit write operations. Set to 0 to always update the database.
    // Note: This option is ignored if using JSON Web Tokens
    // updateAge: 24 * 60 * 60, // 24 hours
  },

  // Enable debug messages in the console if you are having problems
  debug: false,
};

export default (req, res) => NextAuth(req, res, options);
```

### Basic UI for demonstration

Please note that I am using Material-UI for styling. You can use any styling solution you want. The following code is on pages/index.js. I will be using a package called `react-google-button` to render a google sign in button. (It is not necessary, you can style your own button).

```javascript
npm i react-google-button
```

```javascript
import Layout from "../src/components/Layout/Layout";
import { Button, Typography, makeStyles } from "@material-ui/core";
import GoogleButton from "react-google-button";
// functions to manages authentication
import { signin, signout, useSession } from "next-auth/client";
import CircularProgress from "@material-ui/core/CircularProgress";
import Paper from "@material-ui/core/Paper";

const useStyles = makeStyles((theme) => ({
  button: {
    display: "block",
    marginTop: 20,
    marginRight: "auto",
    marginLeft: "auto",
  },
  paper: {
    padding: "3rem 0",
    margin: "auto",
    width: 320,
    marginTop: "10%",
  },
}));

const Home = () => {
  const classes = useStyles();
  // if user is not logged in or has logged out, session will be falsy
  const [session, loading] = useSession();
  if (loading) {
    return <CircularProgress />;
  }
  return (
    <Layout>
      <Paper className={classes.paper} elevation={4}>
        <Typography align="center" color="primary" variant="h3">
          Sign {session ? "Out" : "In"}
        </Typography>
        {session ? (
          <Button
            classes={{ root: classes.button }}
            onClick={signout}
            variant="contained"
            color="primary"
          >
            Sign Out
          </Button>
        ) : (
          <GoogleButton
            className={classes.button}
            onClick={() => signin("google")}
          />
        )}
      </Paper>
    </Layout>
  );
};

export default Home;
```

Full code can be found [here](https://github.com/confusedwdeveloper/dream-book).
