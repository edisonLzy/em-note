---

     title: Redux toolkit
     date: 2021-08-11T13:19:00.325Z
     description: Redux toolkit

---

[课件](http://www.zhufengpeixun.com/strong/html/136.toolkit.html)

## 安装

```shell
npm i @reduxjs/toolkit --save
```

## 使用

- configureStore: 创建仓库 , 简化 reducer 配置 , 内置 redux-devTool

> 内置中间件: redux-thunk

```js
const store = configureStore({
  reducer // reducer 可以是一个函数或者 一个对象(内部通过 combineReducer进行合并)
  middleware: [] // 中间件
  preloadedState // 初始状态
})

store.dispatch(function(dispatch){
  setTimeout(()=>{
    dispatch({
       type: 'ADD'
    })
  })
})
```

- createAction: 接受一个 `字符串` 创建一个 type 为 该字符串的`action`创建函数。 接受 一个 `可选的 payload 映射函数` 返回处理之后的 payload

```js
const add = createAction("add");

dispatch(add(payload));
```

- createReducer: 使用 `查找表` 的方式 创建 reducer,内置了 `immer`

> 根据实现,优化 ee 脚手架 不同构建工具打包的逻辑

```js
const reducer = createReducer(初始值, {
  [actionType]: (state, action) => {
    state.name = "lee"; // 可以直接修改应为 这里 state为 immer处理之后的 draft
  },
});
```

- createSlice: 结合 actionCreator 和 reducers(`内部封装了createReducer和createAction`)

> 通过 reducers 的 key 来作为 aciton 的 type。

```js
const {action,reducers} = createSlice({
   name:命名空间
   initialState: 初始化状态
   reducers
})
```

## immer

> - 老状态不可变
> - 尽可能的复用老状态

## reselect

> - 缓存上一次的计算结果
> - 处理 configureStore 的 `reducer`

# TODO

- [ ] SWR
