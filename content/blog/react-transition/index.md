---
title: React-transition
date: "2020-07-27T07:26:03.284Z"
description: "React transition组件的用法"
comments: true
---

## Transition

### usage

1. 通常用于组件做一些挂载或卸载时候的动画

2. 只维护子元素的过渡状态

### props

1. in: 控制renderProps的当前处于状态是 true(entered)还是false(existed)。控制该值的变化可用于过渡效果

* true -> false: entered->exiting-> exited 
* false -> true: exited-> entering -> entered

2. timeout: 状态过渡到 existed或者 entered的时间

3. mountOnEnter: 只有当状态是`entered`的时候才渲染子组件。默认情况是 直接渲染一次子组件

4. unmounted

5. appear: 应用首次状态过渡

## CSSTransition

### usage

1. 给`根组件`不同的阶段提供不同的css类名实现过度(和 Vue Transition处理方式类似)

2. 只维护子元素的过渡状态

### props

1. in: 控制renderProps的当前处于状态是 true(entered)还是false(existed)。该值的变化可用于不同的类名

* true -> false: enter -> enter-active-> enter-done
* false -> true: exist -> exist-active-> exist-done

2. classNames: 指定类样式的类名。当值为字符串的时候，表示添加特定的类名前缀(可与Animate.css配合使用) 
