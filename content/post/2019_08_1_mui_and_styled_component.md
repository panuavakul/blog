---
title: "Using styled-components to customize Material UI Component"
date: 2019-08-01T13:00:00+09:00
tags: [react, redux]
categories: [note, stupid_stuff]
---

My current project use `styled-components` to style the component instead of the default material ui theme stuffs. So I was looking for a correct way to override some part of the material ui style like for example, the `CircularProgress` 's color.

So what you can do is see the material ui document for the component that you want to override
https://material-ui.com/api/circular-progress/

![1](/static/images/2019_08_1_mui_and_styled_component_1.png)

Here, you can see that it say "Style sheet name" is `MuiCircularProgress` and below that are the keys that you can override.
So in this case I want to override the `colorPrimary` and `colorSecondary` keys so what I have to do is just have to set the class

```css
&.<SheetName>-<Key>{
  ...
  ...
  ...
}
```

for example

```js
const StyledCircularProgress = styled(CircularProgress)`
  && {
    &.MuiCircularProgress-colorPrimary {
      color: ${Color.white};
    }
    &.MuiCircularProgress-colorSecondary {
      color: ${Color.gray.main};
    }
  }
`
```

and then use it as normal component

```js
<StyledCircularProgress />
```