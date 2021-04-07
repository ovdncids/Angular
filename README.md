# Angular

## Node.js
https://nodejs.org

## NVM (Node Version Manager)
Node.js 버전을 관리하는 프로그램, 어느 버전이든 설치, 변경, 삭제 가능하다.

<details><summary>Mac OS</summary>
https://github.com/nvm-sh/nvm

https://gist.github.com/falsy/8aa42ae311a9adb50e2ca7d8702c9af1
```sh
# 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash

# vi 에디터 실행
vi ~/.bash_profile

# 해당 경로 적용 시키키
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# ~/.bash_profile 재 실행 시키기
source ~/.bash_profile
```
</details>

Windows

https://github.com/coreybutler/nvm-windows/releases

```sh
# 설치 된 node.js 리스트를 본다.
nvm ls

# 해당 버전을 설치 한다.
nvm install 14.15.5

# 해당 버전을 삭제 한다.
nvm uninstall 14.15.5

# 해당 버전을 사용 한다.
nvm use 14.15.5

# 기본 버전 변경 한다.
nvm alias default 14.15.5
```

## Angular CLI
https://angular.io/guide/setup-local

```sh
# Angular CLI 설치
npm install -g @angular/cli

# Angular 프로젝트 생성
ng new angular-study
## Do you want to enforce stricter type checking and stricter bundle budgets in the workspace? N
## Would you like to add Angular routing? Y
## Which stylesheet format would you like to use? SCSS

# VSCode로 해당 디렉토리 열기
## build
ng build --prod
npm install -g serve
serve -s dist/angular-study

## 프로젝트 실행
ng serve --open
```
<!-- ```sh
ng test
``` -->

## GIT
소스 관리를 위해 사용한다. 어느 파일이 언제 어떻게 변경 되었는지 쉽게 볼 수 있다.

**GIT 설치**

**VSCode 확장 Git Graph 설치**

## Markup
src/app/app.component.html
```html
<div>
  <header>
    <h1>Angular study</h1>
  </header>
  <hr />
  <div className="container">
    <nav className="nav">
      <ul>
        <li><h2>Members</h2></li>
        <li><h2>Search</h2></li>
      </ul>
    </nav>
    <hr />
    <section className="contents">
      <div>
        <h3>Members</h3>
        <p>Contents</p>
      </div>
    </section>
    <hr />
  </div>
  <footer>Copyright</footer>
</div>
```

src/styles.scss
```scss
body {
  margin: 0;
}

// common
.pointer {
  cursor: pointer;
}

.relative {
  position: relative;
}

.d-none {
  display: none;
}

.d-block {
  display: block;
}

.flex {
  display: flex;
}

// Markup
hr {
  display: none;
}

h1, footer {
  margin: 0.5rem;
}

app-nav {
  @extend .flex;
}

.container {
  @extend .flex;
  min-height: 300px;
  border-top: 1px solid #ddd;
  border-bottom: 1px solid #ddd;
  .nav {
    min-height: 300px;
    background-color: skyblue;
    ul {
      list-style: none;
      margin: 0.5rem 0 0.5rem 0;
      padding: 0;
      a {
        text-decoration: none;
        &.active {
          color: white;
        }
      }
      h2 {
        margin: 0;
        padding: 0.5rem;
      }
    }
  }
  .contents {
    margin-left: 1rem;
  }
}
```

## Angular Component 만들기
header, nav.vue, footer.vue 이렇게 Component 별로 파일을 생성한다.
```sh
ng generate component header
ng generate component nav
ng generate component footer
```

src/app/app.component.html
```diff
- <header>
-  <h1>Angular study</h1>
- </header>
+ <app-header></app-header>

- <nav class="nav">
-   <ul>
-     <li><h2>Members</h2></li>
-     <li><h2>Search</h2></li>
-   </ul>
- </nav>
+ <app-nav></app-nav>

- <footer>Copyright</footer>
+ <app-footer></app-footer>
```

## hidden, *ngIf, props
src/app/app.component.html
```html
<app-header [hidden]="true"></app-header>

<app-nav *ngIf="false"></app-nav>

<app-footer [title]="'카피라이트'"></app-footer>
```

src/app/footer/footer.component.html
```diff
- <footer>Copyright</footer>
+ <footer>{{title}}</footer>
```

src/app/footer/footer.component.ts
```diff
- import { Component, OnInit } from '@angular/core';
import { Component, Input, OnInit } from '@angular/core';
```
```ts
export class FooterComponent implements OnInit {
  @Input() title = 'Copyright';
```
**props는 부모 Component에서 자식 Component로 값을 전달 한다**

## Angular router
```sh
ng generate component members
ng generate component search
```

src/app/app.component.html
```diff
  <section class="contents">
-   <div>
-     <h3>Members</h3>
-     <p>Contents</p>
-   </div>
+   <router-outlet></router-outlet>
```

src/app/app-routing.module.ts
```ts
import { MembersComponent } from './members/members.component';
import { SearchComponent } from './search/search.component';
```
```diff
- const routes: Routes = [];
```
```ts
const routes: Routes = [
  { path: 'members', component: MembersComponent },
  { path: 'search', component: SearchComponent },
  { path: '', redirectTo: '/members', pathMatch: 'full' }
];
```

**주소 창에서 router 바꾸어 보기**

src/app/nav/components/nav.component.html
```html
<li><h2><a routerLink="/members" routerLinkActive="active">Members</a></h2></li>
<li><h2><a routerLink="/search" routerLinkActive="active">Search</a></h2></li>
```

**여기 까지가 Markup 개발자 분들이 할일 입니다.**

## Members Service 만들기
```sh
ng generate service services/members
```
src/app/services/members.service.ts
```ts
export class MembersService {
  members = [];
  member = {
    name: '',
    age: ''
  };

  membersCreate() {
    this.members.push({
      name: this.member.name,
      age: this.member.age
    });
    console.log('Done membersCreate', this.members);
  }
```

## Members Compenent Service inject
src/app/members/members.component.ts
```ts
import { MembersService } from '../services/members.service';
```
```diff
- constructor() { }
+ constructor(public membersService: MembersService) { }
```
```ts
ngOnInit(): void {
  this.membersService.member.name = '';
  this.membersService.member.age = '';
```

src/app/members/members.component.html
```html
<div>
  <h3>Members</h3>
  <hr class="d-block" />
  <div>
    <h4>Read</h4>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
          <th>Modify</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>홍길동</td>
          <td>20</td>
          <td>
            <button>Update</button>
            <button>Delete</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
  <hr class="d-block" />
  <div>
    <h4>Create</h4>
    <input type="text" placeholder="Name"
      [ngModel]="membersService.member.name"
      (ngModelChange)="membersService.member.name = $event"
    >
    <input type="text" placeholder="Age"
      [ngModel]="membersService.member.age"
      (ngModelChange)="membersService.member.age = $event"
    >
    <button (click)="membersService.membersCreate()">Create</button>
  </div>
</div>
```

src/app/app.module.ts
```ts
import { FormsModule } from '@angular/forms';
```
```ts
@NgModule({
  imports: [
    FormsModule
```

**debugger 설명**
```js
debugger
```

## Members Service CRUD
### Read
src/app/services/members.service.ts
```ts
membersRead() {
  this.members = [{
    name: '홍길동',
    age: 20
  }, {
    name: '춘향이',
    age: 16
  }];
  console.log('Done membersRead', this.members);
}
```

src/app/members/members.component.ts
```ts
ngOnInit(): void {
  this.membersService.membersRead();
```

src/app/members/members.component.html
```diff
- <tr>
-   <td>홍길동</td>
-   <td>20</td>
```
```html
<tr *ngFor="let member of membersService.members; let index=index">
  <td>{{member.name}}</td>
  <td>{{member.age}}</td>
```

### Update
src/app/services/members.service.ts
```ts
membersUpdate(index, member) {
  this.members[index] = member;
  console.log('Done membersUpdate', this.members);
}
```

src/app/members/members.component.html
```diff
- <td>{member.name}</td>
- <td>{member.age}</td>
```
```html
<td>
  <input type="text" placeholder="Name"
    [ngModel]="member.name"
    (ngModelChange)="member.name = $event"
  >
</td>
<td>
  <input type="text" placeholder="Age"
    [ngModel]="member.age"
    (ngModelChange)="member.age = $event"
  >
</td>
```
```diff
- <button>Update</button>
```
```html
<button (click)="membersService.membersUpdate(index, member)">Update</button>
```

## Delete
src/app/services/members.service.ts
```ts
membersDelete(index) {
  this.members.splice(index, 1);
  console.log('Done membersDelete', this.members);
}
```

src/app/members/members.component.html
```diff
- <button>Delete</button>
```
```html
<button (click)="membersService.membersDelete(index)">Delete</button>
```

## Backend Server
* [Download](https://github.com/ovdncids/vue-curriculum/raw/master/download/express-server.zip)
```sh
# BE 서버 실행 방법
npm install
node index.js
# 터미널 종료
Ctrl + c
```

## Axios 서버 연동
https://github.com/axios/axios
```sh
npm install axios
```

### Axios common 에러 처리
```sh
ng generate service services/common
```

src/app/services/common.service.js
```js
export class CommonService {
  axiosError(error) {
    console.error(error.response || error.message || error);
  };
```

src/app/services/members.service.ts
```ts
import axios from 'axios';
import { CommonService } from '../services/common.service';
```
```diff
- constructor() {}
+ constructor(private commonService: CommonService) {}
```
```diff
membersCreate() {
- this.members.push({
-   name: this.member.name,
-   age: this.member.age
- })
- console.log('Done membersCreate', this.members);
```
```ts
axios.post('http://localhost:3100/api/v1/members', this.member).then((response) => {
  console.log('Done membersCreate', response);
  this.membersRead();
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

### Read
src/app/services/members.service.ts
```diff
membersRead() {
- this.members = [{
-   name: '홍길동',
-   age: 20
- }, {
-   name: '춘향이',
-   age: 16
- }];
- console.log('Done membersRead', this.members);
```
```ts
axios.get('http://localhost:3100/api/v1/members').then((response) => {
  console.log('Done membersRead', response);
  this.members = response.data.members;
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

### Update
src/app/services/members.service.ts
```diff
membersUpdate(index, member) {
- this.members[index] = member;
- console.log('Done membersUpdate', this.members);
```
```ts
const memberUpdate = {
  index: index,
  member: member,
}
axios.patch('http://localhost:3100/api/v1/members', memberUpdate).then((response) => {
  console.log('Done membersUpdate', response);
  this.membersRead();
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

### Delete
src/app/services/members.service.ts
```diff
membersDelete(index) {
- this.members.splice(index, 1);
- console.log('Done membersDelete', this.members);
```
```ts
axios.delete('http://localhost:3100/api/v1/members/' + index).then((response) => {
  console.log('Done membersDelete', response);
  this.membersRead();
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

## Search Service 만들기
```sh
ng generate service services/search
```

src/app/services/search.service.ts
```ts
import axios from 'axios';
import { CommonService } from '../services/common.service';
```
```diff
- constructor() { }
```
```ts
constructor(
  private commonService: CommonService,
  private membersService: MembersService
) {}

searchRead(search) {
  const url = `http://localhost:3100/api/v1/search?search=${search}`;
  axios.get(url).then((response) => {
    console.log('Done searchRead', response);
    membersStore.members = response.data.members;
  }).catch((error) => {
    axiosError(error);
  });
}
```

## Search Compenent Service inject
src/app/search/search.component.ts
```ts
import { MembersService } from '../services/members.service';
import { SearchService } from '../services/search.service';
```
```diff
- constructor() { }

- ngOnInit(): void {
- }
```
```ts
constructor(
  public membersService: MembersService,
  public searchService: SearchService
) { }

q = '';

ngOnInit(): void {
  this.searchService.searchRead(this.q);
}
```

src/app/search/search.component.html
```html
<div>
  <h3>Search</h3>
  <hr class="d-block" />
  <div>
    <form (submit)="searchService.searchRead(q)">
      <input type="text" placeholder="Search" name="q"
        [ngModel]="q"
        (ngModelChange)="q = $event"
      />
      <button>Search</button>
    </form>
  </div>
  <hr class="d-block" />
  <div>
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let member of membersService.members; let index=index">
          <td>{{member.name}}</td>
          <td>{{member.age}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
```

## Search Component 쿼리스트링 변경과 새로고침 적용
src/app/search/search.component.ts
```ts
import { ActivatedRoute, Router } from '@angular/router';
```
```diff
- constructor(
-   public membersService: MembersService,
-   public searchService: SearchService
- ) { }
```
```ts
constructor(
  private route: ActivatedRoute,
  private router: Router,
  public membersService: MembersService,
  public searchService: SearchService
) {
  route.queryParams.subscribe(queryParams => {
    this.q = queryParams.q
    this.searchService.searchRead(this.q);
  });
}
```
```diff
- q = '';
+ q = this.route.snapshot.queryParams.q || '';
```
```html
searchRead(q): void {
  this.router.navigate(['/search'], {
    queryParams: {
      q: q
    }
  });
}
```

src/app/search/search.component.html
```diff
- <form (submit)="searchService.searchRead(q)">
+ <form (submit)="searchRead(q)">
```
