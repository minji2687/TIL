### Redux with React Router

## 'Router' 와 'Route'

- React 앱에서는 <Router />로 <Route />를 감싼다.
- path에는 URL에서 사용되는 경로를 정의하고 component에는 라우트가 URL과 매치될때 랜더할 단일 컴포넌트를 정의하면 됩니다.

```js
const Root = () => (
  <Router>
    <Route path="/" component={App} />
  </Router>
);
```

## Provider

-React Redux가 제공하는 고차 컴포넌트로 Redux를 React에 바인드

- Router 를 Provider로 감싸서 라우트 핸들러가 store 에 접근 할 수 있게 한다.

```js
const Root = ({ store }) => (
  <Provider store={store}>
    <Router>
      <Route path="/" component={App} />
    </Router>
  </Provider>
);
```

- URL이 '/'에 매치되면 <App /> 컴포넌트가 랜더될겁니다. 그리고 URL에서 파라메터를 읽어올 때 필요한 (:filter) 파라메터를 /에 추가해 주겠습니다.

### browserHistory

- 해시를 제거하고 싶을 때 browserHistory를 사용한다.

- (예: http://localhost:3000/#/?_k=4sbb0i).

```js
<Router history={browserHistory}>
  <Route path="/(:filter)" component={App} />
</Router>
```

### Link

```js
import React from "react";
import { Link } from "react-router";

const FilterLink = ({ filter, children }) => (
  <Link
    to={filter === "all" ? "" : filter}
    activeStyle={{
      textDecoration: "none",
      color: "black"
    }}
  >
    {children}
  </Link>
);

export default FilterLink;
```

```js
import React from "react";
import FilterLink from "../containers/FilterLink";

const Footer = () => (
  <p>
    Show: <FilterLink filter="all">All</FilterLink>
    {", "}
    <FilterLink filter="active">Active</FilterLink>
    {", "}
    <FilterLink filter="completed">Completed</FilterLink>
  </p>
);

export default Footer;
```

- <FilterLink />를 클릭하면 URL이 '/complete', '/active', '/'로 변하는 것

### URL에서 읽어오기

```js
const mapStateToProps = (state, ownProps) => {
  return {
    todos: getVisibleTodos(state.todos, ownProps.filter) // 바꾸기 전에는 getVisibleTodos(state.todos, state.visibilityFilter)
  };
};
```

- params 속성은 URL 내에 지정된 모든 파라메터를 가지는 객체입니다. 예: localhost:3000/completed로 이동하면 params은 { filter: 'completed' }이 됩니다. 이제 우리는 <App />에서 URL을 읽을 수 있습니다.

```js
const App = ({ params }) => {
  return (
    <div>
      <AddTodo />
      <VisibleTodoList filter={params.filter || "all"} />
      <Footer />
    </div>
  );
};
```
