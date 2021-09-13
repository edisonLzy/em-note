---

     title: antd-源码阅读
     date: 2021-09-13T13:21:10.406Z
     description: antd-源码阅读

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

- TODO 256
