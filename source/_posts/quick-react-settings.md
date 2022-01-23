---
title: React + Redux + Typescript + Axios 通用配置
date: 2022-01-23 12:38:43
tags: [react]
copyright: true
---
本文代码对应[仓库地址](https://github.com/Flower-F/react-common-setttings)

本文主要包括以下内容配置
- creat-react-app 创建项目
- 配置 redux
- 配置 typescript
- 配置 json-server
- 配置 axios

# 基本配置

首先按照[这篇文章](https://flower-f.github.io/2022/01/12/eslint-prettier-commitlint/)完成项目创建和基本配置，你也可以使用 webpack 进行基础配置。

把 src 文件夹下的 App.css、index.css、logo.svg 删除。

# 配置 styled-components / Sass / Less

```bash
yarn add styled-components
```

如果报错找不到类型文件，就执行以下命令。

```bash
yarn add @types/styled-components -D
```

# 修改默认样式

在根目录新建文件 `globalStyle.ts`，内容如下：

```ts
import { createGlobalStyle } from "styled-components";

export const GlobalStyle = createGlobalStyle`
  html, body {
    // 根据需要配置 background、line-height、font 等
  }

  * {
    margin: 0;
    padding: 0;
  }

  a {
    text-decoration: none;
  }

  ul, li {
    list-style: none;
  }
`;
```

# 配置 react-router

安装 react-router，进行路径配置。（这里以 V6 为准）

```bash
yarn add react-router-dom
```

在 src 文件夹下新建文件夹 route，创建 index.tsx 文件。内容如下：

```tsx
import { Routes, Route } from "react-router-dom";
import { Suspense, lazy } from "react";
import TopBar from "pages/TopBar";
import Home from "pages/Home";

const ReduxExample = lazy(() => import("pages/ReduxExample"));
const RequestExample = lazy(() => import("pages/RequestExample"));

const MyRouter = () => {
  return (
    <Routes>
      <Route path="/" element={<TopBar />}>
        <Route index element={<Home />} />
        <Route path="home" element={<Home />}></Route>
        <Route
          path="redux"
          element={
            <Suspense fallback={<>Loading...</>}>
              <ReduxExample />
            </Suspense>
          }
        ></Route>
        <Route
          path="request"
          element={
            <Suspense fallback={<>Loading...</>}>
              <RequestExample />
            </Suspense>
          }
        ></Route>
        <Route
          path="*"
          element={
            <main style={{ padding: "1rem" }}>
              <p>404 Not Found</p>
            </main>
          }
        />
      </Route>
    </Routes>
  );
};

export default MyRouter;
```

在 src 目录下的 index.tsx 中引入 **BrowserRouter**

```tsx
import { BrowserRouter } from "react-router-dom";
```

代码修改为

```tsx
ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById("root")
);
```

修改 src 目录下的 APP.tsx，引入默认样式以及路由配置。

```tsx
import MyRouter from "routes";
import { GlobalStyle } from "./globalStyle";

function App() {
  return (
    <>
      <GlobalStyle></GlobalStyle>
      <MyRouter />
    </>
  );
}

export default App;
```

在 src 文件夹下新建文件夹 pages，用于存储页面。

文件夹 pages 下新建文件夹 RequestExample、ReduxExample、TopBar、Home，文件内容如下。

```tsx
// ReduxExample/index.tsx
import { memo } from "react";

const ReduxExample = () => {
  return <h1>ReduxExample</h1>;
};

export default memo(ReduxExample);
```

```tsx
// RequestExample/index.tsx
import { memo } from "react";

const RequestExample = () => {
  return <h1>RequestExample</h1>;
};

export default memo(RequestExample);
```

```tsx
// Home/index.tsx
import { memo } from "react";

const Home = () => {
  return <h1>HomePage</h1>;
};

export default memo(Home);
```

```tsx
// TopBar/index.tsx
import { memo, useEffect } from "react";
import { NavLink, Outlet, useLocation, useNavigate } from "react-router-dom";
import { NavBar, TopbarContainer } from "./style";

const TopBar = () => {
  const { pathname } = useLocation();
  const navigate = useNavigate();

  useEffect(() => {
    if (pathname === "/") {
      navigate("/home");
    }
  }, [pathname, navigate]);

  return (
    <TopbarContainer>
      <div>TopBar</div>
      <NavBar>
        <NavLink
          to="/home"
          className={({ isActive }) => (isActive ? "selected" : "unselected")}
        >
          Home Page
        </NavLink>
        <NavLink
          to="/redux"
          className={({ isActive }) => (isActive ? "selected" : "unselected")}
        >
          Redux Example
        </NavLink>
        <NavLink
          to="/request"
          className={({ isActive }) => (isActive ? "selected" : "unselected")}
        >
          Request Example
        </NavLink>
      </NavBar>
      <Outlet />
    </TopbarContainer>
  );
};

export default memo(TopBar);
```

上面这段代码的 `<Outlet />` 是为了能够渲染下一级的路由。因为目前 react-router-dom@6 在 ts 环境下不支持重定向，所以要先用 `useEffect` 强制重定向。

给 TopBar 加上一些简单的样式，方便我们查看。

```ts
// TopBar/style.ts
import styled from "styled-components";

export const TopbarContainer = styled.div`
  .selected {
    color: red;
    text-decoration: underline;
    cursor: pointer;
  }

  .unselected {
    color: black;
    cursor: default;
    &:hover {
      text-decoration: underline;
      cursor: pointer;
    }
  }
`;

export const NavBar = styled.div`
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin: 20px 0;
  border: 1px solid black;
  width: 100px;
`;
```

# 配置 redux

首先安装依赖，这里处理异步使用的是 redux-thunk。

```bash
yarn add redux redux-thunk react-redux immer
```

在 src 文件夹下新建文件夹 store，再在 store 文件夹中新建文件 index.ts 和 reducer.ts。内容如下：

```ts
// reducer.ts
import { combineReducers } from "redux";
import { reducer as reduxExampleReducer } from "../pages/ReduxExample/store/";

export interface RootState {
  reduxExample: reduxExampleReducer.state;
}

export default combineReducers({
  reduxExample: reduxExampleReducer.reducer,
});
```

```ts
// index.ts
import { createStore, compose, applyMiddleware } from "redux";
import thunk from "redux-thunk";
import reducer from "./reducer";

type windowWithReduxExtension = Window &
  typeof globalThis & {
    __REDUX_DEVTOOLS_EXTENSION_COMPOSE__?: Function;
  };

const composeEnhancers =
  (window as windowWithReduxExtension).__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ||
  compose;

const store = createStore(reducer, composeEnhancers(applyMiddleware(thunk)));

export default store;
```

在 App.tsx 中注入 store。

```tsx
import { Provider } from "react-redux";

function App() {
  return (
    <Provider store={store}>
      <GlobalStyle></GlobalStyle>
      <MyRouter />
    </Provider>
  );
}

export default App;
```

为了验证 Redux 配置是否正确，我们使用经典的 Counter 来验证。

在 src 下创建文件夹 components，再在 components 文件夹下新建文件夹 Counter。

```tsx
// Counter/index.tsx
import { memo } from "react";

interface CounterProps {
  count: number;
  increment: () => void;
  decrement: () => void;
  reset: () => void;
  incrementAsync: () => void;
}

const Counter = ({
  count,
  increment,
  decrement,
  reset,
  incrementAsync,
}: CounterProps) => {
  return (
    <>
      <div>Count: {count}</div>
      <button onClick={increment}>+1</button>
      <button onClick={decrement}>-1</button>
      <button onClick={reset}>Reset</button>
      <button onClick={incrementAsync}>+1 (1s delay)</button>
    </>
  );
};

export default memo(Counter);
```

在 ReduxExample 目录下创建文件夹 store，在 store 文件夹下新建文件 index.ts、store.ts、constants.ts、actions.ts。

```ts
// store/constants.ts
export const INCREMENT = "INCREMENT";

export const DECREMENT = "DECREMENT";

export const RESET = "RESET";

export const INCREMENT_ASYNC = "INCREMENT_ASYNC";
```

```ts
// store/actions.ts
import * as actionTypes from "./constants";
import { Dispatch } from "redux";

export const increment = () => ({ type: actionTypes.INCREMENT });

export const decrement = () => ({ type: actionTypes.DECREMENT });

export const reset = () => ({ type: actionTypes.RESET });

export const incrementAsync = () => (dispatch: Dispatch) => {
  setTimeout(() => {
    dispatch({
      type: actionTypes.INCREMENT_ASYNC,
    });
  }, 1000);
};
```

```ts
// store/reducer.ts
import * as actionTypes from "./constants";
import { AnyAction } from "redux";
import { produce } from "immer";

export interface CounterState {
  count: number;
}

const defaultState: CounterState = {
  count: 0,
};

export const reduxExampleReducer = produce(
  (state: CounterState, action: AnyAction) => {
    switch (action.type) {
      case actionTypes.INCREMENT:
      case actionTypes.INCREMENT_ASYNC:
        state.count = state.count + 1;
        break;
      case actionTypes.DECREMENT:
        state.count = state.count - 1;
        break;
      case actionTypes.RESET:
        state.count = 0;
        break;
      default:
        break;
    }
  },
  defaultState
);
```

```ts
// store/index.ts
import * as reducer from "./reducer";
import * as actions from "./actions";
import * as constants from "./constants";

export { reducer, actions, constants };
```

修改 ReduxExample，进行测试。

```tsx
import Counter from "components/Counter";
import { memo } from "react";
import { useDispatch, useSelector } from "react-redux";
import { RootState } from "store/reducer";
import { actions } from "./store";

const ReduxExample = () => {
  const { count } = useSelector((state: RootState) => ({
    count: state.reduxExample.count,
  }));

  const dispatch = useDispatch();

  const increment = () => {
    dispatch(actions.increment());
  };

  const decrement = () => {
    dispatch(actions.decrement());
  };

  const reset = () => {
    dispatch(actions.reset());
  };

  const incrementAsync = () => {
    dispatch(actions.incrementAsync());
  };

  return (
    <>
      <h1>ReduxExample</h1>
      <Counter
        count={count}
        increment={increment}
        decrement={decrement}
        reset={reset}
        incrementAsync={incrementAsync}
      />
    </>
  );
};

export default memo(ReduxExample);
```

# 配置 json-server

json-server 是我认为的一种比较不错的 mock 数据方法。

如果还没有安装的话先全局一下安装 json-server。

```bash
npm install -g json-server
```

在根目录下新建文件夹 `__mock__`，然后在文件夹下新建文件 `db.json`。文件内容为：

```json
{
  "subjects": {
    "list": [
      {
        "id": 1,
        "title": "html"
      },
      {
        "id": 2,
        "title": "css"
      },
      {
        "id": 3,
        "title": "js"
      }
    ],
    "code": 200
  }
}
```

因为 3000 端口已经被我们的页面占用了，所以我们要换一个端口运行命令启动 json-server。为了便捷我们可以在 `package.json` 中加入新的脚本命令。

![](https://cdn.jsdelivr.net/gh/Flower-F/picture@main/img/20220123172231.png)

配置完成后，运行命令

```
yarn server
```

即可在 http://localhost:3001 开启 json-server。

在浏览器输入路径 http://localhost:3001/subjects ，可以看到 `db.json` 中的数据。

# 配置 axios

执行命令安装 axios。

```bash
yarn add axios
```

在 src 目录下新建文件夹 api，并在 api 文件夹下新建文件 config.ts、request.ts。

```ts
// api/config.ts
import axios from "axios";

export const baseUrl = "http://localhost:3001";

const axiosInstance = axios.create({
  baseURL: baseUrl,
});

export { axiosInstance };
```

```ts
import { axiosInstance } from "./config";

export const getSubjectsRequest = <T=any>() => {
  return axiosInstance.get<T>("/subjects");
};
```

编写组件 SubjectList 来验证 axios 配置。

在 components 文件夹下新建文件夹 SubjectList。

```tsx
// SubjectList.tsx
import { memo } from "react";

interface Subject {
  id: number;
  title: string;
}

interface SubjectListProps {
  list: Subject[];
}

const SubJectList = ({ list }: SubjectListProps) => {
  return (
    <ul>
      {list.map((item) => (
        <li key={item.id}>
          {item.id} - {item.title}
        </li>
      ))}
    </ul>
  );
};

export default memo(SubJectList);
```

在 SubjectList 文件夹下新建 store 文件夹。

```ts
// store/constants.ts
export const CHANGE_SUBJECTS = "CHANGE_SUBJECTS";
```

```ts
// store/reducer.ts
import * as actionTypes from "./constants";
import { AnyAction } from "redux";
import produce from "immer";

export interface Subject {
  id: number;
  title: string;
}

export interface RequestExampleState {
  subjectList: Subject[];
}

const defaultState: RequestExampleState = {
  subjectList: [],
};

export const requestExampleReducer = produce(
  (state: RequestExampleState, action: AnyAction) => {
    switch (action.type) {
      case actionTypes.CHANGE_SUBJECTS:
        state.subjectList = action.data;
        break;
      default:
        break;
    }
  },
  defaultState
);
```

```ts
// store/actions.ts
import * as actionTypes from "./constants";
import { getSubjectsRequest } from "api/request";
import { Dispatch } from "redux";
import { RequestExampleState } from "./reducer";

export const changeSubjectList = (data: RequestExampleState) => ({
  type: actionTypes.CHANGE_SUBJECTS,
  data,
});

export const getSubjectList = () => (dispatch: Dispatch) => {
  getSubjectsRequest<{ list: RequestExampleState }>()
    .then(({ data }) => {
      const action = changeSubjectList(data.list);
      dispatch(action);
    })
    .catch(() => {
      console.log("subjects 传输错误");
    });
};
```

```ts
// store/index.ts
import * as reducer from "./reducer";
import * as actions from "./actions";
import * as constants from "./constants";

export { reducer, actions, constants };
```

然后把 reducer 导入全局：

```ts
// src/store/reducer.ts
import { combineReducers } from "redux";
import { reducer as reduxExampleReducer } from "pages/ReduxExample/store/";
import { reducer as requestExampleReducer } from "pages/RequestExample/store";

export interface RootState {
  requestExample: requestExampleReducer.RequestExampleState;
  reduxExample: reduxExampleReducer.CounterState;
}

export default combineReducers({
  reduxExample: reduxExampleReducer.reduxExampleReducer,
  requestExample: requestExampleReducer.requestExampleReducer,
});
```

修改 RequestExample/index.tsx，内容为：

```tsx
import SubjectList from "components/SubjectList";
import { memo, useEffect } from "react";
import { useDispatch, useSelector } from "react-redux";
import { RootState } from "store/reducer";
import { actions } from "./store";

const RequestExample = () => {
  const { list } = useSelector((state: RootState) => ({
    list: state.requestExample.subjectList,
  }));

  const dispatch = useDispatch();

  const getSubjectList = () => {
    dispatch(actions.getSubjectList());
  };

  useEffect(() => {
    if (!list.length) {
      getSubjectList();
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  return (
    <>
      <h1>RequestExample</h1>
      <SubjectList list={list} />
    </>
  );
};

export default memo(RequestExample);
```

完成任务！