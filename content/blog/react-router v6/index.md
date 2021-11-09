---

     title: react-router v6
     date: 2021-11-09T10:05:42.745Z
     description: react-router v6

---

## breaking change

- [] Replace any `<Route>`s that are not inside a `<Switch> `with `useRouteMatch`, or wrap them in a`<Switch>`

- [x] Replace any elements inside a `<Switch>` that are not plain `<Route>` elements with a regular `<Route>`. This includes any `<PrivateRoute>`-style custom components.

1. `do all your own composition in the <Route element> prop`

```tsx
      <Route path="/public">
        <PublicPage />
      </Route>
      <PrivateRoute path="/protected" redirectTo="/login">
        <ProtectedPage />
      </PrivateRoute>


function PrivateRoute({ path, children, redirectTo }) {
  let isAuthenticated = getAuth();
  return (
    <Route
      path={path}
      render={() => (
        isAuthenticated ? children : <Redirect to={redirectTo} />
      )}
    />
  );
}
```

- [x] 嵌套路由使用 带有`*`的 path 来指定

```tsx
<Route path="users/*" element={<Users />} />
```

### useNavigate

- 代替 `useHistory`
- 更好的兼容 `suspense` : `moving from using the `history`API directly to the`navigate` API is to provide better compatibility with React suspense`

### Navigate

- 代替 `Redirect`

### Routes

- 使用 `Routes` 组件代替 `Switch` 组件

### Route

- 废弃了 component 和 render 属性 ，直接使用 children 属性 或者 element 属性

```tsx
 //v6

 function Home(){
  const params = useParams()
}

 <Route path="/">
        <Home />
 </Route>
 <Route path="/users/:id" children={<User />} />
```

- Route 组件只能被 Routes 或者 Route 组件包裹

1. `[PrivateRoute] is not a <Route> component. All component children of <Routes> must be a <Route> or <React.Fragment>`

```tsx
function PrivateRoute(props) {
  // BAD. This code will never run!
  return <Route {...props} />;
}
```

- 详细属性

```ts
export interface PathRouteProps {
  caseSensitive?: boolean;
  children?: React.ReactNode;
  element?: React.ReactElement | null;
  index?: false;
  path: string;
}
export interface LayoutRouteProps {
  children?: React.ReactNode;
  element?: React.ReactElement | null;
}
export interface IndexRouteProps {
  element?: React.ReactElement | null;
  index: true;
}
```

### Redirect

- [] 不在`Switch`中的`Redirect`,将作为 Navigate 组件

## 参考文献

1. https://reacttraining.com/blog/react-router-v5-1

2. https://v5.reactrouter.com/web/api/Hooks

3. https://gist.github.com/mjackson/d54b40a094277b7afdd6b81f51a0393f
