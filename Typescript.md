# Typescript

## Meber 타입 생성하기
src/app/services/members.service.ts

```ts
// Member 타입 생성
interface Member {
  name: string
  age?: number
}

member: Member = {
  name: '',
  age: undefined
};
```

## Mebers 타입 생성하기
```ts
members: Array<Member> = [];
// or
members: Member[] = [];
```
