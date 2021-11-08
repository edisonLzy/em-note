---

     title: Spin
     date: 2021-11-08T14:24:53.484Z
     description: Spin

---

## 样式文件

- `>` : 直接子代组合器（Child combinator）

```less
&-nested-loading {
  position: relative;
  > div > .@{spin-prefix-cls} {
  }
}
```

## size: 图标大小如何控制

- 通过 `size 属性` ,设置不同的类名.从而设置 `i`标签的大小

1. 默认的 `spin图标`,通过 `i 标签` 来实现,所以自定义的图标没法通过该属性控制大小.

```less
&-lg &-dot {
  font-size: @spin-dot-size-lg;
  i {
    width: 14px;
    height: 14px;
  }
}
```

## delay: 延迟展示图标如何实现

- 利用 lodash 的 debounce 函数来实现延迟执行

1. debouncifyUpdateSpinning: 更新 updateSpinning 函数

```ts
debouncifyUpdateSpinning = (props?: SpinProps) => {
  const { delay } = props || this.props;
  if (delay) {
    this.cancelExistingSpin();
    this.updateSpinning = debounce(this.originalUpdateSpinning, delay);
  }
};
```

2. updateSpinning 相等则说明在 spin 动画`true`中或者没有 loading 动画`false`

```ts
updateSpinning = () => {
  const { spinning } = this.props; // true
  const { spinning: currentSpinning } = this.state; // false
  if (currentSpinning !== spinning) {
    this.setState({ spinning });
  }
};
```

3.  cancelExistingSpin: 调用 debounce 函数的 cancel 方法来取消延迟函数的执行

```ts
cancelExistingSpin() {
    const { updateSpinning } = this;
    // (updateSpinning as any).cancel) 有值说明是 debounce返回的函数
    if (updateSpinning && (updateSpinning as any).cancel) {
      (updateSpinning as any).cancel();
    }
  }
```

## 参考文献

1. https://developer.mozilla.org/zh-CN/docs/Web/CSS/Child_combinator
2. https://www.lodashjs.com/docs/lodash.debounce
3. https://flukeout.github.io/
