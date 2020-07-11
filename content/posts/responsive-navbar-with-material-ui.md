---
title: "Responsive Navbar With Material Ui"
date: 2020-07-10T22:04:23+05:30
draft: false
toc: true
images:
tags:
  - React
  - Material-UI
---

### Introduction

Material-Ui is a react framework that we can use to design and style react components. It is based on Google's material design system and makes building new web pages a lot faster.  
One caveat is that the docs are confusing if you are new to React. For example, while we can copy paste a Navbar from docs if we are building our website with bootstrap, things are not that easy with Material-UI. Today we are going to look at how I developed a responsive navbar for my latest project.

### Installation

Follow these [instructions](https://material-ui.com/getting-started/installation/) to install material ui and correctly set up font and icons(if you need to) in your application. If you are using NextJs, you can download the starter files from [here](https://github.com/mui-org/material-ui/tree/master/examples/nextjs).  
Now let's take a look at the code.

### The Appbar

- Import Appbar from material-ui as the base components. I grabbed the following code from material-ui documentations and made some minor changes, so there is not need to memorize.

```javascript
import * as React from "react";
import { makeStyles } from "@material-ui/core/styles";
import AppBar from "@material-ui/core/AppBar";
import Toolbar from "@material-ui/core/Toolbar";
import Typography from "@material-ui/core/Typography";
import Button from "@material-ui/core/Button";
import IconButton from "@material-ui/core/IconButton";
import MenuIcon from "@material-ui/icons/Menu";
// Link is a custom anchor component that combines Material-UI Link component and NextJs Link, and comes along with Material-UI starter for NextJs
import Link from "../Link/Link";
import { Box } from "@material-ui/core";

const useStyles = makeStyles((theme) => ({
  menuButton: {
    marginRight: theme.spacing(2),
  },
  title: {
    flexGrow: 1,
  },
}));

const Navbar = () => {
  const classes = useStyles();

  const loggedOutLinks = <Button color="inherit">Login</Button>;
  const loggedInLinks = (
    <>
      <Button underline="none" component={Link} href="/dreams" color="inherit">
        Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/my-dreams"
        color="inherit"
      >
        My Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/new-dream"
        color="inherit"
      >
        New Dream
      </Button>
      <Button underline="none" color="inherit">
        Logout
      </Button>
    </>
  );

  return (
    <AppBar position="static">
      <Toolbar>
        <IconButton
          edge="start"
          className={classes.menuButton}
          color="inherit"
          aria-label="menu"
        >
          <MenuIcon />
        </IconButton>
        <Typography variant="h5" className={classes.title}>
          <Link href="/" color="inherit" underline="none">
            DreamBook
          </Link>
        </Typography>
        <Box>{loggedInLinks}</Box>
      </Toolbar>
    </AppBar>
  );
};
export default Navbar;
```

- Now we can implement showing the menu icon at "md and down" breakpoint. We'll also implement the reverse for Links. For that we'll use Material-UI Hidden component.

```javascript
import Hidden from "@material-ui/core/Hidden";
```

I am setting implementation as css since I am using NextJs (server rendered pages).

```javascript
import * as Reacr from "react";
import { makeStyles } from "@material-ui/core/styles";
import AppBar from "@material-ui/core/AppBar";
import Toolbar from "@material-ui/core/Toolbar";
import Typography from "@material-ui/core/Typography";
import Button from "@material-ui/core/Button";
import IconButton from "@material-ui/core/IconButton";
import MenuIcon from "@material-ui/icons/Menu";
import Link from "../Link/Link";
import { Box } from "@material-ui/core";
import Hidden from "@material-ui/core/Hidden";
import { useTheme } from "@material-ui/core/styles";
import useMediaQuery from "@material-ui/core/useMediaQuery";
const useStyles = makeStyles((theme) => ({
  menuButton: {
    marginRight: theme.spacing(2),
  },
  title: {
    flexGrow: 1,
  },
}));

const Navbar = () => {
  const classes = useStyles();

  // media query helper
  const theme = useTheme();
  const matches = useMediaQuery(theme.breakpoints.down("sm"));
  const center = matches ? "center" : "inherit";

  const loggedOutLinks = <Button color="inherit">Login</Button>;
  const loggedInLinks = (
    <>
      <Button underline="none" component={Link} href="/dreams" color="inherit">
        Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/my-dreams"
        color="inherit"
      >
        My Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/new-dream"
        color="inherit"
      >
        New Dream
      </Button>
      <Button underline="none" color="inherit">
        Logout
      </Button>
    </>
  );

  return (
    <AppBar position="static">
      <Toolbar>
        <Hidden implementation="css" mdUp>
          <IconButton
            edge="start"
            className={classes.menuButton}
            color="inherit"
            aria-label="menu"
          >
            <MenuIcon />
          </IconButton>
        </Hidden>

        <Typography align={center} variant="h5" className={classes.title}>
          <Link href="/" color="inherit" underline="none">
            DreamBook
          </Link>
        </Typography>
        <Hidden implementation="css" smDown>
          <Box>{loggedInLinks}</Box>
        </Hidden>
      </Toolbar>
    </AppBar>
  );
};
export default Navbar;
```

You can see that I have also used `useQuery` hook along with `useTheme` to center navbar title at lower breakpoints.

- Now we can implement Drawer component easily and control it with a local state value in our app component. Here is the code for drawer:

```javascript
import React from "react";

// Please note that loggedInLinks and loggedOutLinks will be toggled later based on auth state of the application
import { makeStyles } from "@material-ui/core/styles";
import Drawer from "@material-ui/core/Drawer";
import Button from "@material-ui/core/Button";
import Link from "../Link/Link";

const useStyles = makeStyles((theme) => ({
  list: {
    width: 150,
    display: "flex",
    flexDirection: "column",
    justifyContent: "space-between",
    alignItems: "center",
    paddingTop: theme.spacing(8),
  },
  paper: {
    backgroundColor: theme.palette.primary.main,
    color: theme.palette.primary.contrastText,
  },
}));

const SideNav = ({ toggleDrawer, isOpen }) => {
  const classes = useStyles();
  const logedOutLinks = (
    <div
      className={classes.list}
      role="presentation"
      onClick={toggleDrawer(false)}
      onKeyDown={toggleDrawer(false)}
    >
      <Button color="inherit">Login</Button>;
    </div>
  );
  const loggedInLinks = (
    <div
      className={classes.list}
      role="presentation"
      onClick={toggleDrawer(false)}
      onKeyDown={toggleDrawer(false)}
    >
      <Button underline="none" component={Link} href="/dreams" color="inherit">
        Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/my-dreams"
        color="inherit"
      >
        My Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/new-dream"
        color="inherit"
      >
        New Dream
      </Button>
      <Button underline="none" color="inherit">
        Logout
      </Button>
    </div>
  );

  return (
    <Drawer
      classes={{ paper: classes.paper }}
      open={isOpen}
      onClose={toggleDrawer(false)}
    >
      {loggedInLinks}
    </Drawer>
  );
};

export default SideNav;
```

- Now we can import it to our Nav component, place it side by side Appbar, pass down required props and we'll be done.

```javascript
import * as React from "react";
import { makeStyles } from "@material-ui/core/styles";
import AppBar from "@material-ui/core/AppBar";
import Toolbar from "@material-ui/core/Toolbar";
import Typography from "@material-ui/core/Typography";
import Button from "@material-ui/core/Button";
import IconButton from "@material-ui/core/IconButton";
import MenuIcon from "@material-ui/icons/Menu";
import Link from "../Link/Link";
import { Box } from "@material-ui/core";
import Hidden from "@material-ui/core/Hidden";
import { useTheme } from "@material-ui/core/styles";
import useMediaQuery from "@material-ui/core/useMediaQuery";
import SideNav from "./SideNav";
const useStyles = makeStyles((theme) => ({
  menuButton: {
    marginRight: theme.spacing(2),
  },
  title: {
    flexGrow: 1,
  },
}));

const Navbar = () => {
  const classes = useStyles();

  // drawer handling
  const [isOpen, setIsOpen] = React.useState(false);
  const toggleDrawer = (value) => (event) => {
    if (
      event.type === "keydown" &&
      (event.key === "Tab" || event.key === "Shift")
    ) {
      return;
    }

    setIsOpen(value);
  };

  // media query helper
  const theme = useTheme();
  const matches = useMediaQuery(theme.breakpoints.down("sm"));
  const center = matches ? "center" : "inherit";

  const loggedOutLinks = <Button color="inherit">Login</Button>;
  const loggedInLinks = (
    <>
      <Button underline="none" component={Link} href="/dreams" color="inherit">
        Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/my-dreams"
        color="inherit"
      >
        My Dreams
      </Button>
      <Button
        underline="none"
        component={Link}
        href="/new-dream"
        color="inherit"
      >
        New Dream
      </Button>
      <Button underline="none" color="inherit">
        Logout
      </Button>
    </>
  );

  return (
    <>
      <SideNav isOpen={isOpen} toggleDrawer={toggleDrawer} />
      <AppBar position="static">
        <Toolbar>
          <Hidden implementation="css" mdUp>
            <IconButton
              onClick={toggleDrawer(true)}
              edge="start"
              className={classes.menuButton}
              color="inherit"
              aria-label="menu"
            >
              <MenuIcon />
            </IconButton>
          </Hidden>

          <Typography align={center} variant="h5" className={classes.title}>
            <Link href="/" color="inherit" underline="none">
              DreamBook
            </Link>
          </Typography>
          <Hidden implementation="css" smDown>
            <Box>{loggedInLinks}</Box>
          </Hidden>
        </Toolbar>
      </AppBar>
    </>
  );
};
export default Navbar;
```

The full source code can be found [here](https://github.com/confusedwdeveloper/dream-book).
