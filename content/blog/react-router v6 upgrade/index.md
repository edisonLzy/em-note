---

     title: react-router v6 upgrade
     date: 2021-11-09T10:05:42.648Z
     description: react-router v6 upgrade

---

## upgrade

```shell
npm i react-router-dom@latest
npm i @types/react-router-dom@latest -D
```

## useNavigate

- case 1 : `history.push`

```ts
import { useHistory } from "react-router-dom";

function App() {
  const history = useHistory();
  history.push("/home");
}
```

```ts
import { useNavigate } from "react-router-dom";

function App() {
  const navigate = useNavigate();
  navigate("/home");
}
```

- case 2 : `history.replace`

```ts
import { useHistory } from "react-router-dom";
function App() {
  const history = useHistory();
  history.replace("/home");
}
```

```ts
import { useNavigate } from "react-router-dom";
function App() {
  const navigate = useNavigate();
  history.replace("/home", {
    replace: true,
  });
}
```

- case 3 : go goBack goForward

```ts
import { useHistory } from "react-router-dom";
function App() {
  const history = useHistory();
  history.go(-2);
  history.goBack();
  history.goForward();
}
```

```ts
import { useNavigate } from "react-router-dom";
function App() {
  const navigate = useHistory();
  navigate(-2);
  navigate(-1);
  navigate(1);
}
```
