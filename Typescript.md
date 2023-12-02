# Typescript

## Any 타입에 타입 넣기
### Meber 타입 생성하기
src/app/services/users.service.ts

```ts
// User 타입 생성
declare interface User {
  name: string,
  age: number | string
}
```

### Mebers 타입 생성하기
```ts
users: Array<User> = [];
// or
users: User[] = [];
```

## User.age 타입 수정하기
### User.age 타입에 number와 ''만 받기
src/app/services/users.service.ts
```diff
declare interface User {
  name: string,
- age: number | string
+ age: number | ''
}
```

### User.age 타입에 number만 받기
src/app/services/users.service.ts
```diff
declare interface User {
  name: string,
- age: number | string
+ age: number
}

user: User = {
  name: '',
- age: ''
+ age: 0
};
```

src/app/components/contents/users/users.component.ts
```diff
ngOnInit(): void {
  this.usersService.user.name = '';
- this.usersService.user.age = '';
+ this.usersService.user.age = 0;
  this.usersService.usersRead();
}
```
* ❕ 하지만 input 박스에서는 문자가 입력 가능 하다.

src/app/components/contents/users/users.component.html
```diff
<input type="text" placeholder="Age"
  [ngModel]="usersService.user.age"
- (ngModelChange)="usersService.user.age = $event"
+ (ngModelChange)="insertUserAge($event)"
>
+ {{usersService.user.age}}
```
* ❕ html 파일에서는 타입을 체크할 수 없다.

src/app/components/contents/users/users.component.ts
```ts
insertUserAge($event: string): void {
  this.usersService.user.age = Number($event);
}
```
* `void` 설명, `void` -> `string`, `void` -> `undefined` 수정해 보기

## Optional
### input 박스에서 number와 undefined 받기
src/app/services/users.service.ts
```diff
declare interface User {
  name: string,
- age: number
+ age?: number
}

user: User = {
  name: '',
- age: 0
+ age: undefined
};
```

src/app/components/contents/users/users.component.ts
```diff
ngOnInit(): void {
  this.usersService.user.name = '';
- this.usersService.user.age = 0;
+ this.usersService.user.age = undefined;
  this.usersService.usersRead();
}

insertUserAge($event: string): void {
- this.usersService.user.age = Number($event);
+ this.usersService.user.age = $event === '' ? undefined : Number($event);
}
```

## Optional chaining
src/app/services/users.service.ts
```ts
declare interface OptionalChaining {
  func1?: Function,
  any1: any
}

export class UsersService {
  optionalChaining: OptionalChaining = {
    func1: function() {
      console.log('func1');
    },
    any1: {
      func2: function() {
        console.log('func2');
      }
    }
  }
}
```
src/app/components/contents/users/users.component.ts
```diff
ngOnInit(): void {
+ this.usersService.optionalChaining.func1?.();
+ this.usersService.optionalChaining.any1?.func2?.();
}
```
* ❔ `func1`을 `undefined`로 변경하기
* ❔ `func2`을 `undefined`로 변경하기
* ❔ `any1`을 `undefined`로 변경하기
* ❔ `func1` 함수에 `return '리턴 func1';` 넣어서 확인해 보기

### Axios error 타입 가져오기
src/app/services/common.service.ts
```ts
import { AxiosError } from 'axios';
// or
import type { AxiosError } from 'axios';
```
```diff
- axiosError(error: any) {
+ axiosError(error: AxiosError) {
```

## Axios error with Redux Toolkit Thunk
```ts
axiosErrorHandler(error, thunkAPI);

export const axiosErrorHandler = (error: unknown, thunkAPI: { dispatch: ThunkDispatch<unknown, unknown, AnyAction> }) => {
  if (axios.isAxiosError(error)) {
    const axiosError: AxiosError = error;
    console.log(axiosError, thunkAPI.dispatch);
  }
  console.error(error);
}
```

## Axios, Fetch with Redux-Saga
```ts
# Axios
import axios, { AxiosResponse } from 'axios';

const response: AxiosResponse<{ result: string }> = yield call(() =>
  axios.get('http://localshot:3100/api/v1/users')
);

# Fetch
import { ServerResponse } from 'http';

declare interface FetchServerResponse extends ServerResponse {
  json: Function;
}

const response: FetchServerResponse = yield call(
  () => fetch('http://localshot:3100/api/v1/users', {
    method: 'GET'
  })
);
const responseJson: string = yield call(() => response.json());
```

## Axios 공용 오류 처리
```ts
const api = axios.create();
api.interceptors.response.use(
  (response: AxiosResponse<any, any>) => {
    return response;
  }, (error) => {
    // 오류 처리 로직
    if (error.code === 'ERR_NETWORK' && error.request.status === 0) {
      window.location.reload();
    }
    throw error;
  }
);
const response = api.get('');
```
* ❕ `throw error`가 없으면 `api.get('')`에서 에러가 발생해도 `response`는 `undefined`를 받는다.

## Redux initialState
```ts
import { createSlice } from '@reduxjs/toolkit';

export interface User {
  name: string,
  age: string | number
}

interface UsersState {
  users: User[],
  user: User
}

export const usersSlice = createSlice({
  name: '$users',
  initialState: {
    users: [],
    user: {
      name: '',
      age: ''
    }
  } as UsersState,
  reducers: {
  }
});

export const usersState = (state: { $users: UsersState }) => state.$users;
export const usersActions = usersSlice.actions;

export default usersSlice.reducer;
```

## Window
```ts
declare global {
  interface Window {
    naver: any,
    N: any,
    MarkerClustering: any
  }
}
```

## Global
```ts
declare global {
  var naver: any
}
```

## 선언되지 않은 속성 읽기
```ts
const a = { a1: 123 };
const { a2 } = a as unknown as { a2: number };
```

## Type이 복수인 경우 어떤 Type인지 확인
```ts
interface A {
  a: number
}

interface B {
  b: number
}

const a: A | B = {a: 123};
const b: A | B = {b: 456};
console.log('a' in a);  // true
console.log('b' in a);  // false
```

## keyof
```ts
interface User {
  name: string,
  age: number
}

interface K {
  k: keyof User  // 'name' | 'age'
}

const k = {
  k: 'name'
} as K;
```

## TSLint 오류들
### object[key] 형식으로 접근할때 (TS:7031 암시적으로 'any' 형식이 있습니다)
```ts
interface User {
  [key: string]: number
}
```
```ts
interface Code {
  a: string,
  [key: string]: string
}

const code: Code = {
  a: '1',
  b: '2'
}

const expression: Code = {
  a: 'AA'
}

Object.entries(code).forEach(([key, value]: [keyof Code, string]) => {
  console.log(expression[key] || key, value)
})
```
<!--
[propName: string]: any;
[propsName: string]: any;
-->
