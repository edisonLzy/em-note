---

     title: React 18
     date: 2021-08-11T13:19:00.090Z
     description: React 18

---

- [x] React18 以前所有的渲染都是同步的(异步是可选渲染的)
- [ ] auto-batching update
- [ ] 更新优先级
- [ ] 调度优先级

## 如何使用 react18

1. 安装

```shell
npm i react@alpha react-dom@alpha
```

2. tsconfig.json

```json
compiler:{
  "types":['react/next','react-dom/next']
}
```

3. 新的 API

```tsx
// 启动并发渲染模式
ReactDOM.createRoot(container).render(App);
```

## Suspense

1. 子树优先级低于其他更新优先级

   ### 实现原理

   1. ErrorBoundary 捕获子组件抛出的异常
   2. 维护一个 loading 状态,通过这个状态来控制子树的渲染

   ### React.lazy 实现原理

   1. 第一次渲染抛出异常,会被 Suspense 捕获

## Suspenselist

1. 编排一组 Suspense 的加载顺序

## startTransition

1. 将一个任务变成低优先级任务，让高优任务先执行

## auto-batching

1. 更新优先级相同的更新会进行合并

# GET

1. 小顶堆
