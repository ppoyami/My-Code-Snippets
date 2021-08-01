# Redux Types

## settings

@types/react-redux

react-redux

redux

redux-thunk

## reducer state

```typescript
interface RepositoriesState {
  loading: boolean;
  error: string | null;
  data: string[];
}

const reducer = (
  state: RepositoriesState = initialState,
  action: any
): RepositoriesState => {... switch(action.type)}
```

## reducer action

```typescript
export enum ActionType {
  SEARCH_REPOSITORIES = 'search_repositories',
  SEARCH_REPOSITORIES_SUCCESS = 'search_repositories_success',
  SEARCH_REPOSITORIES_ERROR = 'search_repositories_error',
}
```

```typescript
import { ActionType } from '../action-types';

interface SearchRepositoriesAction {
  type: ActionType.SEARCH_REPOSITORIES;
}

interface SearchRepositoriesSuccessAction {
  type: ActionType.SEARCH_REPOSITORIES_SUCCESS;
  payload: string[];
}

interface SearchRepositoriesErrorAction {
  type: ActionType.SEARCH_REPOSITORIES_ERROR;
  payload: string;
}

export type Action =
  | SearchRepositoriesAction
  | SearchRepositoriesSuccessAction
  | SearchRepositoriesErrorAction;
```

```typescript
import { ActionType } from '../action-types';
import { Action } from '../actions';

// 생략...

const reducer = (
  state: RepositoriesState = initialState,
  action: Action
): RepositoriesState => {
  switch (action.type) {
    case ActionType.SEARCH_REPOSITORIES:
      return { loading: true, error: null, data: [] };
    case ActionType.SEARCH_REPOSITORIES_SUCCESS:
      return { loading: false, error: null, data: action.payload };
    case ActionType.SEARCH_REPOSITORIES_ERROR:
      return { loading: false, error: action.payload, data: [] };
    default:
      return state;
  }
};

export default reducer;
```

## Define Root State and Dispatch Types

```typescript
import { combineReducers } from 'redux';
import repositoriesReducer from './repositoriesReducer';

const reducers = combineReducers({
  repositories: repositoriesReducer,
}); // reducers는 반환하는 객체의 타입정보를 알고 있다.

export default reducers;

// 각 리듀서들의 타입정보를 가지고 있는 root 리듀서의 모든 타입을 ReturnType<>화 한다.
export type RootState = ReturnType<typeof reducers>;

```

`@reduxjs/toolkit`

```typescript
import { configureStore } from '@reduxjs/toolkit'
// ...

const store = configureStore({
  reducer: {
    posts: postsReducer,
    comments: commentsReducer,
    users: usersReducer,
  },
})

// Infer the `RootState` and `AppDispatch` types from the store itself
export type RootState = ReturnType<typeof store.getState>
// Inferred type: {posts: PostsState, comments: CommentsState, users: UsersState}
export type AppDispatch = typeof store.dispatch
```

## Define Typed Hooks

```typescript
import { useDispatch } from 'react-redux';
import { bindActionCreators } from 'redux';
import { actionCreators } from '../state';

// A: actionCreator 전달을 깔끔하게 하기 위해 사용한다.
export const useActions = () => {
  const dispatch = useDispatch();

  return bindActionCreators(actionCreators, dispatch);
  // *  {searchRepositories: dispatch(searchRepositories)}
};

```

```typescript
import { useSelector, TypedUseSelectorHook } from 'react-redux';
import { RootState } from '../state';
// 미리 타입이 지정된 useSelector를 만든다.
export const useTypedSelector: TypedUseSelectorHook<RootState> = useSelector;
```

`@reduxjs/toolkit`

```typescript
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux'
import type { RootState, AppDispatch } from './store'

// Use throughout your app instead of plain `useDispatch` and `useSelector`
export const useAppDispatch = () => useDispatch<AppDispatch>()
```

## ReturnType, const Assertion

```typescript
const ADD_TODO = 'ADD_TODO'

function addTodo(title: string) {
  // *** const assertion - type 속성을 타입추론 시 활용할 수 있게 하기 위함
  return <const>{
    type: ADD_TODO, // 이 속성을 고정하기 위
    payload: {
      title
    }
  }
}
// *** interface 대신 ReturnType을 활용해 중복 제거
type AddTodoAction = ReturnType<typeof addTodo>

// ...리듀서에서
function reducer(state: any, action: AddTodoAction) {
  switch(action.type) {
    case ADD_TODO:
      return {
        // *** 컴파일 타임에 에러가 잡힙니다. 자동 완성 역시 잘 됩니다.
        todos: [...state.todos, action.title]
      }
  }
}
```

