## Action && Reducer

### Action

- 작업에 대한 정보를 지니고 있는 객체

- 리덕스에서 상태트리를 변경하는 유일한 방법

- 액션은 <b style="text-decoration:underline">객체</b>입니다.

- 이름은 대문자와 \_ (언더스코어) 를 사용한다.

```js
const ADD_TODO = 'ADD_TODO'

{
  type: ADD_TODO,
    //액션객체는 필수로 type을 가지고 있어야 한다. type 은 액션이 어떤 종류인지 알려준다.

  text: 'Build my first Redux app'
     //추가적인 정보가 필요할 경우, 그 아래 필드를 추가해서 추가적인 정보를 적어준다.
}

```

- 타입의 종류에 따라서 나중에 리듀서가 할 일을 하게 됨

* 하지만 이 액션 객체를 매번 새로 만들기는 귀찮다.
  그래서 액션생성자를 사용한다.

### 액션 생산자

- calmelCase 로 사용한다.

- 액션 생성자는 <b>함수</b>이고, 리턴하는것은 객체이다.

- 액션이 많을경우 종류별로 파일을 분리해서 관리해도 된다.

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text //파라미터가 있으면 적어주면 된다. text: text와 동일하다
  };
}
```

액션을 보내려면 액션 생상자의 결과값을 <b style="text-decoration:underline">dispatch()</b> 함수에 넘깁니다

```js
dispatch(addTodo(text));
dispatch(completeTodo(index));
```

아니면 자동으로 액션을 보내주는 바인드된 액션 생산자를 만듭니다

```js
const boundAddTodo = text => dispatch(addTodo(text));
const boundCompleteTodo = index => dispatch(completeTodo(index));
```

이들은 바로 호출할 수 있습니다

```js
boundAddTodo(text);
boundCompleteTodo(index);
```

### Reducer

- 리듀서는 변화를 일이키는 함수 입니다.

- action 객체를 처리합니다.

- <b>순수</b>해야함

- 절대로 리듀서 내에서 하지 말아야 할 것들은

  - 인수들을 변경(mutate)하기

  - API 호출이나 라우팅 전환같은 사이드이펙트를 일으키기

  - Date.now()나 Math.random() 같이 순수하지 않은 함수를 호출하기.

- <b>이전 상태</b>와 <b>액션</b>을 받아서 다음상태를 반환한다.
  (previousState,action) => newState

- 예전 상태는 변경하지 않고 새로운 복사본을 만든 후 거기에다 모든 변화를 준 다음에 반환

#### 상태 설계

```js
import { VisibilityFilters } from "./actions";

const initialState = {
  visibilityFilter: VisibilityFilters.SHOW_ALL,
  todos: []
};

function todoApp(state = initialState, action) {
  //이 리듀스 함수가 처음실행 될 때는 state 가  undifined 이다
  //그럴떄는 initialState를 반환하도록 한다.

  //   if (typeof state === 'undefined') {
  //     return initialState
  //   }

  return state;
}
```

### 더 많은 액션 다루기

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      });
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      });
    default:
      return state;
  }
}
```

##### Object.assign에 관하여

state를 변경하지 않았습니다.<br>
Object.assign()을 통해 복사본을 만들었죠.<br>
반드시 첫번째 인수로 빈 객체를 전달해야 합니다. <br>
ES7로 제안된 object spread syntax을 써서 { ...state, ...newState }로 작성할수도 있습니다.

```js
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1); // { a: 1, b: 2, c: 3 }, target object itself is changed.
```

- 리듀서 쪼개기

지금까지의 코드

```js
function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      });
    case ADD_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos,
          {
            text: action.text,
            completed: false
          }
        ]
      });
    case COMPLETE_TODO:
      return Object.assign({}, state, {
        todos: [
          ...state.todos.slice(0, action.index),
          Object.assign({}, state.todos[action.index], {
            completed: true
          }),
          ...state.todos.slice(action.index + 1)
        ]
      });
    default:
      return state;
  }
}
```

todos 분리하기

- todoApp은 관리할 상태의 조각만을 넘기고, todos는 그 조각을 어떻게 수정할지 알고 있습니다.

```js
function todos(state = [], action) {
  //todos가 state도 받지만 이건 그냥 배열이
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ];
    case COMPLETE_TODO:
      return [
        ...state.slice(0, action.index),
        Object.assign({}, state[action.index], {
          completed: true
        }),
        ...state.slice(action.index + 1)
      ];
    default:
      return state;
  }
}

function todoApp(state = initialState, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return Object.assign({}, state, {
        visibilityFilter: action.filter
      });
    case ADD_TODO:
    case COMPLETE_TODO:
      return Object.assign({}, state, {
        todos: todos(state.todos, action)
      });
    default:
      return state;
  }
}
```

```js
function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ];
    case COMPLETE_TODO:
      return [
        ...state.slice(0, action.index),
        Object.assign({}, state[action.index], {
          completed: true
        }),
        ...state.slice(action.index + 1)
      ];
    default:
      return state;
  }
}

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter;
    default:
      return state;
  }
}

function todoApp(state = {}, action) {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  };
}
```

- 리듀서 합치기

```js
import { combineReducers } from "redux";

const todoApp = combineReducers({
  visibilityFilter,
  todos
});

export default todoApp;
```

#### Source Code

```js
import { combineReducers } from "redux";
import {
  ADD_TODO,
  COMPLETE_TODO,
  SET_VISIBILITY_FILTER,
  VisibilityFilters
} from "./actions";
const { SHOW_ALL } = VisibilityFilters;

function visibilityFilter(state = SHOW_ALL, action) {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter;
    default:
      return state;
  }
}

function todos(state = [], action) {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ];
    case COMPLETE_TODO:
      return [
        ...state.slice(0, action.index),
        Object.assign({}, state[action.index], {
          completed: true
        }),
        ...state.slice(action.index + 1)
      ];
    default:
      return state;
  }
}

const todoApp = combineReducers({
  visibilityFilter,
  todos
});

export default todoApp;
```

reducx 특징과 흐름

1. 뷰가 액션을 요청하면, 액션 생성자가 액션을 만든다.

2. 뷰가 스토어에게 액션을 준다.

3. 스토어가 현제 상태트리를 루트 리듀서에게 준다.

4. 루트 리듀서가 현재 상태를 나눠서 담당 서브 리듀서에게 보내준다.

5. 서브리듀서는 상태의 사본을 만들어서 변경해야할 것을 변경하고 사본을 돌려준다.

6. 루트리듀서는 변경된 상태를 한곳에 모아서 스토어에게 돌려준다.
7. 스토어는 뷰 레이어 바인딩에게 상태가 변경되었다는 것을 알려주면 뷰레이어가 새로운 상태를 달라고 한다

8. 뷰레이어 바인딩이 새로운 데이터를 받고, 뷰에게 화면의 특정 부분을 업데이트 하라고 시킨다.

```

```

- slice

```js
var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
var citrus = fruits.slice(1, 3);

// fruits contains ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango']
// citrus contains ['Orange','Lemon']
```
