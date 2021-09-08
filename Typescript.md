# Typescript

## Any 타입에 타입 넣기
### Meber 타입 생성하기
src/app/services/members.service.ts

```ts
// Member 타입 생성
declare interface Member {
  name: string
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
### Member.age 타입에 number만 받기
src/app/services/members.service.ts
```diff
declare interface Member {
  name: string
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
* 하지만 input 박스에 문자가 입력 가능 하다.

### input 박스에서 number만 받기
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
* `void` 설명, `void` -> `string` 수정해 보기
