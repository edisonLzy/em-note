---
title: TEST
date: "2020-07-27T07:26:03.284Z"
description: "Just about everything you'll need to style in the theme: headings, paragraphs, blockquotes, tables, code blocks, and more."
categories: [hello world]
comments: true
image:
  feature: https://images.unsplash.com/photo-1440635592348-167b1b30296f?crop=entropy&dpr=2&fit=crop&fm=jpg&h=475&ixjsv=2.1.0&ixlib=rb-0.3.5&q=50&w=1250
  credit: hahah
  creditlink: https://unsplash.com/photos/Ki0dpxd3LGc
---

## Transition

1. 通常用于组件做一些挂载或卸载时候的动画
2. 只维护子元素的过渡状态
3. in: 控制renderProps的当前处于状态是 true(entered)还是false(existed)。控制该值的变化可用于过渡效果

```ts
interface Page{
    name: string
}
```
