---
title: "Organize Redux Selectors With Createstructuredselector"
date: 2020-03-15T22:01:59+05:30
draft: false
toc: false
images:
tags:
  - React
  - Redux
---

Reselect library makes it easy to create memoized selectors that we can use with redux. Today I learned about another function that reselect exports which makes it easy to keep our selectors organized when we pass them to connect component of react-redux.  
Normally we'd so something like this:

```javascript
import SelectorA from "../selectors";
import SelectorB from "../selectors";
import SelectorC from "../selectors";
import SelectorD from "../selectors";

const mapStateToProps = (state) => ({
  prop1: selectorA(state),
  prop2: selectorB(state),
  prop3: selectorC(state),
  props4: selectorD(state),
});
// Now we can pass mapStateToProps to connect
```

createStrcturedSelector helps us avoid this repeating pattern (calling selectors one by one while passing redux state as argument). We can modify the above code like this:

```javascript
import SelectorA from "../selectors";
import SelectorB from "../selectors";
import SelectorC from "../selectors";
import SelectorD from "../selectors";

import { createStructuredSelector } from "reselect";

const mapStateToProps = createStructuredSelector({
  prop1: selectorA,
  prop2: selectorB,
  prop3: selectorC,
  props4: selectorD,
});
// This reduces some unnecessary boilerplate code
```

Now we don't need to call and pass state to each selector one by one.
