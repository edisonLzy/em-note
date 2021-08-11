##

---

     title: react动画
     date: 2021-08-11T13:16:24.476Z
     description:

### Trans

---

- 通常用于组件做一些挂载或卸载时候的动画
- 只维护子元素的过渡状态

- in: 控制 renderProps 的当前处于状态是 true(entered)还是 false(existed)。控制该值的变化可用于过渡效果

- true -> false: entered->exiting-> exited
- false -> true: exited-> entering -> entered

- timeout: 状态过渡到 existed 或者 entered 的时间
- mountOnEnter: 只有当状态是`entered`的时候才渲染子组件。默认情况是 直接渲染一次子组件
- unmounted
- appear: 应用首次状态过渡

### CSSTransition

> - 给`根组件`不同的阶段提供不同的 css 类名实现过度(和 Vue Transition 处理方式类似)
> - classNames: 指定类样式的类名。当值为字符串的时候，表示添加特定的类名前缀(可与 Animate.css 配合使用)

- 进入阶段(in 属性`false->true`): enter -> enter-active-> enter-done
- 退出阶段(in 属性`true->false`): exist -> exist-active-> exist-done
- 结合 Animate.css

```shell
npm i Animate.css
```

### SwitchTransition

> 用于有`秩序`的切换内部组件,需要结合上面两个组件结合使用。
>
> 当`key`发生变化的时候,会将之前的根元素，添加退出样式(exit, exit-active)。退出完成后，将该 DOM 元素移除。最后渲染新的 dom 元素，添加进入的样式(enter ,enter-active,enter-dom)。
>
> 通过 onExited 事件, 改变 key 的值(`timeout` )。

```tsx
<SwitchTransition>
  <CSSTransition key={}></CSSTransition>
</SwitchTransition>
```

- mode: in-out/out-in(默认值)

> in-out: 保留之前的元素,先完成新渲染的元素的进入过渡，然后执行保留元素的退出过渡

### TransitionGroup

> children: 是 `CSSTranstion` 组件或者 `Transition` 组件 。根据 key 值来控制过度状态。
>
> appear: 所有子元素都会应用 appear 样式
>
> component: 控制该元素渲染的 DOM 元素，可以为 null

```tsx
<TransitionGroup>
  <CSSTranstion key={key}></CSSTranstion>
</TransitionGroup>
```

## 自定义动画组件

- [x] 通过 onEnter 和 onEntered 事件 来统一 transition 的过渡时间 和 timeout 的时间

# TODO

- [ ] TransitionGroup 如何进行列表排序
- [ ] 组件插槽的 slot 和 属性 children 的编译结果是否相同

# GET

1. addEventListenrs 的第三个参数的`once` 可以控制事件只触发一次
