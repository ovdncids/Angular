# Typescript

## Meber 타입 생성하기
src/app/services/members.service.ts

```ts
// Member 타입 생성
interface Member {
  name: string
  age: number | string
}
```

## Mebers 타입 생성하기
```ts
members: Array<Member> = [];
// or
members: Member[] = [];
```

## Axios error 타입 가져오기
src/app/services/common.service.ts
```ts
import { AxiosError } from 'axios'
```
```diff
- axiosError(error: any) {
+ axiosError(error: AxiosError) {
```
