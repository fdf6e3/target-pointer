# 🎯 Target Pointer

This is one of my toy projects made with **Vanilla JavaScript** without any framework. It includes very basic yet important front-end features.

## 🔍 Project description

Users are able to

- target a pointer by moving their mouse 🖱
- check its coordinates 👀

## 🔨 Note

### ❗️ (1)

- For the better performance(to improve CRP),
  `` tag.style.top = `${y}px`; `` better be changed to

```cs
tag.style.transform = `translate(${x}px, ${y}px)`;
```

- Because `transform` does not trigger layout event, whereas `top` or `left` does. (See more on [CSS Triggers](https://csstriggers.com/).)

### ❗️ (2)

- The code block below is an attempt to adjust the target image to be center.
- However, it does not work because `targetHalfWidth` and `targetHalfHeight` are somehow `0`, even though it does not look like it on the browser.

```cs
const target = document.querySelector('.target');

const targetRect = target.getBoundingClientRect();
const targetHalfWidth = targetRect.width / 2;
const targetHalfHeight = targetRect.height / 2;

document.addEventListener('mousemove', e => {
  const x = e.clientX;
  const y = e.clientY;
  ...
  target.style.transform = `translate(${x - targetHalfWidth}px, ${y -
    targetHalfHeight}px)`;
  ...
});
```

- It is because the `<script>` is embedded with `defer`,
- which means as soon as the HTML is ready, the script is excuted before the src(in this case, `<img class="target" src="img/target.png" alt="target" />` arrives,
- therefore, the value of `targetHalfWidth` and `targetHalfHeight` are `0`.

- to solve this issue, the code block should be called with the `load` event. (See more info about [Resource events](https://developer.mozilla.org/en-US/docs/Web/Events).)

```cs
const target = document.querySelector('.target');

addEventListener('load', e => {
  const targetRect = target.getBoundingClientRect();
  const targetHalfWidth = targetRect.width / 2;
  const targetHalfHeight = targetRect.height / 2;

  document.addEventListener('mousemove', e => {
    const x = e.clientX;
    const y = e.clientY;
    ...
    target.style.transform = `translate(${x - targetHalfWidth}px, ${y -
      targetHalfHeight}px)`;
    ...
  });
});
```
