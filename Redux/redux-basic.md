## Getting Started Redux

### Actions
actions은 내가 만든 app 에서 store로 보내는 데이터의 payload 이다. actions는 스토어에 보낼 수 있는 유일한 데이터이다. 우리는 store.dispatch()를 통해서 actions을 보낼 수 있다.

```javascript
	const ADD_TODO = 'ADD_TODO'
	{
	  type: ADD_TODO,
	  text: 'Build my first Redux app'
	}
```

 actions는 자바스크립트 객체이다. action은 무조건 type을 포함해야하며 이것을 통해 어떤 동작을 할지 정해진다. type은 string 상수로 할당되어야 한다.

우리의 app이 커짐에 따라 actions Type이 많아진다면 그것을 다른 파일에 만들어 두고 import 하는 편이 가독성도 좋고 직관적으로 이해하기도 편하다.

type과 함께 다른 데이터도 같이 보낼 수 있다. 위의 예제 처럼 string을 보낼 수도 있고 number, object도 가능하다.

하지만 오고 가는 빈도수가 높을 때 담겨있는 데이터가 크다면 부담이 되고 성능 저하가 올 것이다. 만약 object array 데이터중 하나를 수정하게 된다면 그것을 다 넘기지 않고 그것들을 actions에 넣어두고, type과 함께 index만 넘겨주게 한다면 오고가는 오버헤드가 줄어들 것이다.

### Action Creators
이건 말 그대로 액션을 생성하는 함수이다.

redux에서는 action creator는 간단하게 actions을 return 한다.

```javascript
	function actionCreator(text) {
	  return {
	    type: ADD_TODO,
	    text
	  }// action
	}
```

기본적인 Flux 에서는 action creator는 dispatch를 발생하기도 한다. 그러나 Redux에서는 좀 다르다 Redux에서는 dispatch를 이런식으로 사용한다.

```javascript
	dispatch(addTodo(text)) // addTodo return action
	dispatch(completeTodo(index))
```

아니면 이렇게도 사용 가능하다.

```javascript
	const boundAddTodo = text => dispatch(addTodo(text))
	const boundCompleteTodo = index => dispatch(completeTodo(index))
	// addTodo를 거쳐서 action이 생성되고,
	//dispatch에 type과 기타 데이터가 들어간 js obj가 들어가게 된다.
	boundAddTodo(text)
	boundCompleteTodo(index)
```

dispatch function은 store를 통해서 직접 접근할 수 있지만 react에서는 react-redux의 connect 함수를 통해 접근 가능하다.

bindActionCreators 함수를 사용하여 많은 action creator를 dispatch 함수와 연결 가능하다.

actions.js

```javascript
	/*
	 * action types
	 */

	export const ADD_TODO = 'ADD_TODO'
	export const TOGGLE_TODO = 'TOGGLE_TODO'
	export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

	/*
	 * other constants
	 */

	export const VisibilityFilters = {
	  SHOW_ALL: 'SHOW_ALL',
	  SHOW_COMPLETED: 'SHOW_COMPLETED',
	  SHOW_ACTIVE: 'SHOW_ACTIVE'
	}

	/*
	 * action creators
	 */

	export function addTodo(text) {
	  return { type: ADD_TODO, text }
	}

	export function toggleTodo(index) {
	  return { type: TOGGLE_TODO, index }
	}

	export function setVisibilityFilter(filter) {
	  return { type: SET_VISIBILITY_FILTER, filter }
	}
```

### Reducers
Actions는 뭔가 일어났다 정도만 알려준다. 하지만 app의 state를 어떻게 바꿀 것인지는 모른다. 이 작업을 하는 것이 reducer 이다.

#### Handling Actions
reducer는 previous state와 action을 받는 함수이다. 그리고 new state를 return  한다.

```javascript
	(previousState, action) => newState
```

reducer 내부에서 하지 말아야 할 것들이 있다.
- argument를 복제하면 안된다.
- api call이나 routing transition을 하면 안된다.
- pure function이 아닌 것을 호출하면 안된다 예를들어 Date.now(), Math.random() 같은것들

지금은 일단 reducer는 반드시 pure function이라는 것만 알아두자.

자 이제 reducer를 만들어 보자.

일단 initial state를 만들어야 한다. Redux는 맨 처음에 reducer를 undefined state와 함께 호출 할 것이다. 이때가 우리가 만든 initial state를 적용할 때다.

```javascript
	const initialState = {
	  visibilityFilter: VisibilityFilters.SHOW_ALL,
	  todos: []
	}
	function todoApp(state = initialState, action) {
	  // For now, don't handle any actions
	  // and just return the state given to us.
	  return state
	}
```

그 다음으로 action type에 따라서 어떤 일을 할 지 결정 해 주자.

```javascript
	function todoApp(state = initialState, action) {
	  switch (action.type) {
	    case SET_VISIBILITY_FILTER:
	      return Object.assign({}, state, {
	        visibilityFilter: action.filter
	      })
	    default:
	      return state
	  }
	}
```

우리는 매개인자로 들어온 state를 복제할 수 없다. 그래서 object.assgn을 통해서 새로운 {} 객체에 원래있던 state의 property들과 new state의 property를 넣어주는 것이다. { ...state, ...newState}도 가능하다.

```javascript
	import {
	  ADD_TODO,
	  TOGGLE_TODO,
	  SET_VISIBILITY_FILTER,
	  VisibilityFilters
	} from './actions'

	...

	function todoApp(state = initialState, action) {
	  switch (action.type) {
	    case SET_VISIBILITY_FILTER:
	      return Object.assign({}, state, {
	        visibilityFilter: action.filter
	      })
	    case ADD_TODO:
	      return Object.assign({}, state, {
	        todos: [
	          ...state.todos,
	          {
	            text: action.text,
	            completed: false
	          }
	        ]
	      })
	    default:
	      return state
	  }
	}
```

### Store
이전에서 같이 살펴봤듯, action은 어떤 이벤트가 발생했는지를 나타내고, reducer는 actions에 따라 state를 업데이트 한다.

Strore은 action과 reducer를 사용한다. 그리고 아래의 것들을 한다.
- hold app state
- allow access to state via getState
- allow state to be updated via dispatch(action)
- register listenders via subscribe(listender)
- 등록되지 않은 listender를 subscribe(listener)가 리턴한 함수를 통해 컨트롤 한다.

store는 reducer만 있다면 쉽게 만들 수 있다.

```javascript
	import { createStore } from 'redux'
	import todoApp from './reducers'
	let store = createStore(todoApp)
```

만약 inital state를 명시하고 싶다면 두번째 매개변수에 넣어줄 수도 있다.

```javascript
	let store = createStore(todoApp, window.STATE_FROM_SERVER)

	import {
	  addTodo,
	  toggleTodo,
	  setVisibilityFilter,
	  VisibilityFilters
	} from './actions'

	// Log the initial state
	console.log(store.getState())

	// Every time the state changes, log it
	// Note that subscribe() returns a function for unregistering the listener
	const unsubscribe = store.subscribe(() =>
	  console.log(store.getState())
	)

	// Dispatch some actions
	store.dispatch(addTodo('Learn about actions'))
	store.dispatch(addTodo('Learn about reducers'))
	store.dispatch(addTodo('Learn about store'))
	store.dispatch(toggleTodo(0))
	store.dispatch(toggleTodo(1))
	store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED))

	// Stop listening to state updates
	unsubscribe()
```

이런식으로 actions를 불러오고 todoApp reducer를 통해 store를 만들고 store.dispatch(action)을 통해 state를 변경 할 수 있다.

### 전체적인 동작
가정 : 어떤 버튼에 이벤트가 달려있고 그것은 state를 변경한다고 하자.

1. actions.js와 reducer.js를 만들어 둔다.
2. createStore(reducer) 를 통해 store를 만든다.
3. app 컴포넌트를 provider로 감싸고 store를 제공한다.
4. app 컴포넌트 상위에 container 컴포넌트를 만든다.
5. container 컴포넌트에서는 mapStateToProps, mapActionsToProps 함수를 구현하여 app컴포넌트가 state와 action을 props로 받을 수 있도록 한다.
6. props로 받은 함수를 버튼에 등록한다.
7. 버튼을 누르면 함수가 실행된다.
8. action중 하나의 함수가 실행되고 action의 return 내용이 reducer로 들어간다.
9. reducer는 현재 state와 action을 받는다.
10. action type에 따라 reducer 함수를 실행시켜서 state를 변경한다.
11. store에 저장되어있는 state가 변경된다.
10. 변경 내용이 컴포넌트에 적용되며 re-rendering 된다.
