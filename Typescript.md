# Typescript

## Any 타입에 타입 넣기
### Meber 타입 생성하기
src/app/services/members.service.ts

```ts
// Member 타입 생성
declare interface Member {
  name: string,
  age: number | string
}
```

### Mebers 타입 생성하기
```ts
members: Array<Member> = [];
// or
members: Member[] = [];
```

### Axios error 타입 가져오기
src/app/services/common.service.ts
```ts
import { AxiosError } from 'axios'
```
```diff
- axiosError(error: any) {
+ axiosError(error: AxiosError) {
```

## Member.age 타입 수정하기
### Member.age 타입에 number와 ''만 받기
src/app/services/members.service.ts
```diff
declare interface Member {
  name: string,
- age: number | string
+ age: number | ''
}
```

### Member.age 타입에 number만 받기
src/app/services/members.service.ts
```diff
declare interface Member {
  name: string,
- age: number | string
+ age: number
}

member: Member = {
  name: '',
- age: ''
+ age: 0
};
```

src/app/components/contents/members/members.component.ts
```diff
ngOnInit(): void {
  this.membersService.member.name = '';
- this.membersService.member.age = '';
+ this.membersService.member.age = 0;
  this.membersService.membersRead();
}
```
* ❕ 하지만 input 박스에서는 문자가 입력 가능 하다.

src/app/components/contents/members/members.component.html
```diff
<input type="text" placeholder="Age"
  [ngModel]="membersService.member.age"
- (ngModelChange)="membersService.member.age = $event"
+ (ngModelChange)="insertMemberAge($event)"
>
+ {{membersService.member.age}}
```
* ❕ html 파일에서는 타입을 체크할 수 없다.

src/app/components/contents/members/members.component.ts
```ts
insertMemberAge($event: string): void {
  this.membersService.member.age = Number($event);
}
```
* `void` 설명, `void` -> `string`, `void` -> `undefined` 수정해 보기

## Optional
### input 박스에서 number와 undefined 받기
src/app/services/members.service.ts
```diff
declare interface Member {
  name: string,
- age: number
+ age?: number
}

member: Member = {
  name: '',
- age: 0
+ age: undefined
};
```

src/app/components/contents/members/members.component.ts
```diff
ngOnInit(): void {
  this.membersService.member.name = '';
- this.membersService.member.age = 0;
+ this.membersService.member.age = undefined;
  this.membersService.membersRead();
}

insertMemberAge($event: string): void {
- this.membersService.member.age = Number($event);
+ this.membersService.member.age = $event === '' ? undefined : Number($event);
}
```

## Optional chaining
src/app/services/members.service.ts
```ts
declare interface OptionalChaining {
  func1?: Function,
  any1: any
}

export class MembersService {
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
src/app/components/contents/members/members.component.ts
```diff
ngOnInit(): void {
+ this.membersService.optionalChaining.func1?.();
+ this.membersService.optionalChaining.any1?.func2?.();
}
```
* ❔ `func1`을 `undefined`로 변경 하기
* ❔ `func2`을 `undefined`로 변경 하기
* ❔ `any1`을 `undefined`로 변경 하기
* ❔ `func1` 함수에 `return '리턴 func1';` 넣어서 확인해 보기
