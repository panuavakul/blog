---
title: "Hello Hugo"
date: 2019-07-31T18:00:00+09:00
tags: [react, redux]
categories: [note, stupid_stuff]
---

# `mapDispatchToProps` without `mapStateToProps`

So I have been working with React/Redux recently. I think I understand Redux pretty well thanks to by previous Flutter/Redux project. I also worked with JS in the past, but after couple of years working with Swift and Dart, JS just become weird. Now with trash talking aside, I was trying to map my action creator to props using the magic `mapDispatchToProps` and `compose` function, then I get the 

```
TypeError: dispatch is not a function
```

error, which I spent almost an hour trying to solve. Maybe I should have relized that a simple Google search just told me to put null in the first parameter of the magic `connect()` function.


### From
```js
export default compose(
  connect(
    mapDispatchToProps
  )
)(MyComponent)
```

### To
```js
export default compose(
  connect(
    null,
    mapDispatchToProps
  )
)(MyComponent)
```

And that's it.