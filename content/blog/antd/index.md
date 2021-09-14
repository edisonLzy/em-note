---

     title: antd
     date: 2021-09-14T15:18:42.597Z
     description: antd

---

## button

- 利用 函数模拟 tuple

```ts
const tuple = <T extends string[]>(...args: T) => args;
const tupleNum = <T extends number[]>(...args: T) => args;
const a = tuple("1", "2"); // a: ['1','2']
```

- tuple 转联合类型

```tsx
type tuple = ["1", 1, false];
type u = tuple[number]; // '1' | 1 | false
```

- 锚点元素类型

```tsx
type A = React.AnchorHTMLAttributes<any>;
const a: A = {
  href: "x",
};
```

- forwardRef 转发类型

1. 通过结果约束函数的类型，并添加静态属性

```tsx
interface ForwardRefRenderFunction<T, P = {}> {
  (
    props: PropsWithChildren<P>,
    ref: ((instance: T | null) => void) | MutableRefObject<T | null> | null
  ): ReactElement | null;
  displayName?: string | undefined;
  // explicit rejected with `never` required due to
  // https://github.com/microsoft/TypeScript/issues/36826
  /**
   * defaultProps are not supported on render functions
   */
  defaultProps?: never | undefined;
  /**
   * propTypes are not supported on render functions
   */
  propTypes?: never | undefined;
}
```

- 获取 children 的数量

```tsx
React.Children.count(children);
```

- 获取 dom 元素的文本内容

1. el.textContent

```tsx
const buttonText = buttonRef.current.textContent;
```

- 类型收窄

1. 如果 loading 是一个对象 和 普通类型,则直接使用 typeof 就可以推断出 loading 是一个对象类型

```tsx
type Loading =
  | number
  | {
      delay: number;
    }
  | {
      d: string;
    };
function isDeley(val: any): val is {
  delay: number;
} {
  return !!val.delay;
}
function loading(l: Loading) {
  if (typeof l === "object") {
    console.log(l); // filter掉 number类型
    if (isDeley(l)) {
      console.log(l.delay);
    }
  }
}
```

- 判断内容是否是两个中文

```ts
const rxTwoCNChar = /^[\u4e00-\u9fa5]{2}$/;
const isTwoCNChar = rxTwoCNChar.test.bind(rxTwoCNChar);
```

- 利用 loading 状态，来避免无效的点击

```tsx
const [innerLoading, setLoading] = React.useState<Loading>(!!loading);
const handleClick = (
  e: React.MouseEvent<HTMLButtonElement | HTMLAnchorElement, MouseEvent>
) => {
  const { onClick } = props;
  if (innerLoading) {
    // loading状态的点击不处理
    return;
  }
  if (onClick) {
    (onClick as React.MouseEventHandler<HTMLButtonElement | HTMLAnchorElement>)(
      e
    );
  }
};
```

- classnames

1. https://www.npmjs.com/package/classnames

```tsx
const classes = classNames(
  prefixCls,
  {
    [`${prefixCls}-${type}`]: type,
    [`${prefixCls}-${shape}`]: shape,
    [`${prefixCls}-${sizeCls}`]: sizeCls,
    [`${prefixCls}-icon-only`]: !children && children !== 0 && iconType,
    [`${prefixCls}-background-ghost`]: ghost && !isUnborderedButtonType(type),
    [`${prefixCls}-loading`]: innerLoading,
    [`${prefixCls}-two-chinese-chars`]: hasTwoCNChar && autoInsertSpace,
    [`${prefixCls}-block`]: block,
    [`${prefixCls}-dangerous`]: !!danger,
    [`${prefixCls}-rtl`]: direction === "rtl",
  },
  className
);
```

- omit.js 删除对象中的某些属性

```ts
omit(obj, [key1, key2]);
```

- React.ForwardRefExoticComponent<ButtonProps & React.RefAttributes<HTMLElement>>

1. 可用于设置 defaultProps 以及 propType 的类型

```ts
interface CompoundedComponent
  extends React.ForwardRefExoticComponent<
    ButtonProps & React.RefAttributes<HTMLElement>
  > {
  Group: typeof Group;
  __ANT_BUTTON: boolean;
}
```

- 如何给函数组件定义静态属性

1. 给 forwardRef 的组件添加静态属性

```tsx
interface CompoundedComponent
  extends React.ForwardRefExoticComponent<
    ButtonProps & React.RefAttributes<HTMLElement>
  > {
  Group: typeof Group;
  __ANT_BUTTON: boolean;
}
const Button = React.forwardRef<unknown, ButtonProps>(
  InternalButton
) as CompoundedComponent;
```

## Wave

- 通过属性选择器 AnimationAPI , RAF ,以及通过插入 Style 标签设置 css 变量来实现不同颜色的 wave 效果\

1. 获取不同的 wave 颜色

```ts
const waveColor =
  getComputedStyle(node).getPropertyValue("border-top-color") || // Firefox Compatible
  getComputedStyle(node).getPropertyValue("border-color") ||
  getComputedStyle(node).getPropertyValue("background-color");
```

2. 动态创建 style 元素结合 属性选择器 动态设置 css 变量的值

```ts
// Not white or transparent or grey
styleForPseudo = styleForPseudo || document.createElement("style");
if (
  waveColor &&
  waveColor !== "#ffffff" &&
  waveColor !== "rgb(255, 255, 255)" &&
  isNotGrey(waveColor) &&
  !/rgba\((?:\d*, ){3}0\)/.test(waveColor) && // any transparent rgba color
  waveColor !== "transparent"
) {
  extraNode.style.borderColor = waveColor;
  styleForPseudo.innerHTML = `
      [${getPrefixCls(
        ""
      )}-click-animating-without-extra-node='true']::after, .${getPrefixCls(
    ""
  )}-click-animating-node {
        --antd-wave-shadow-color: ${waveColor};
      }`;
  if (!document.body.contains(styleForPseudo)) {
    document.body.appendChild(styleForPseudo);
  }
}
```

- node.nodeType: [nodeType](https://www.w3schools.com/jsref/prop_node_nodetype.asp)

1. 1 表示 一个元素节点

- [element.offsetParent](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) : 判断元素是否显示

1. 获取 距离 element 最近的定位元素
2. 当元素的 `style.display` 设置为 "none" 时，`offsetParent` 返回 `null`

```ts
function isHidden(element: HTMLElement) {
  if (process.env.NODE_ENV === "test") {
    return false;
  }
  return !element || element.offsetParent === null;
}
```

- getComputedStyle(el): 获取 el 元素上面的 CSSStyleDeclaration ,可以通过 getPropertyValue 来获取指定属性的值

```tsx
getComputedStyle(el).getPropertyValue("border");
```

- requestAnimationFrame polyfill for node and the browser.

[raf](https://www.npmjs.com/package/raf)

- AnimationEvent 事件的类型

```tsx
onTransitionEnd = (e: AnimationEvent) => {
  if (!e || e.animationName !== "fadeEffect") {
    return;
  }
  this.resetEffect(e.target as HTMLElement);
};
```
