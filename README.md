# Angular - Standalone(17버전 이후) - 18.2.10
* https://angular.dev
* [데모](https://curriculums-min.web.app)

## Node.js
https://nodejs.org

## NVM (Node Version Manager)
https://github.com/ovdncids/react-curriculum/blob/master/NVM.md

## Angular CLI
https://angular.io/guide/setup-local

```sh
# Angular CLI 설치
npm install -g @angular/cli

# Angular 프로젝트 생성
ng new angular-study
## node 18.19 이상 버전 필요
## Would you like to enable autocompletion? This will set up your terminal so pressing TAB while typing Angular CLI commands will show possible options and autocomplete arguments. (Enabling autocompletion will modify configuration files in your home directory.) Y
### 자동 완성을 .zshrc 설정 파일에 넣는다. Y
## Which stylesheet format would you like to use? Sass (SCSS)
## Do you want to enable Server-Side Rendering (SSR) and Static Site Generation 
(SSG/Prerendering)? N
## Would you like to add Angular routing? Y

cd angular-study
code .

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
소스 관리를 위해 사용한다. 어느 파일이 언제 어떻게 변경되었는지 쉽게 볼 수 있다.

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
  <div class="container">
    <nav class="nav">
      <ul>
        <li><h2>Users</h2></li>
        <li><h2>Search</h2></li>
      </ul>
    </nav>
    <hr />
    <section class="contents">
      <div>
        <h3>Users</h3>
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
* {
  margin: 0;
  font-family: -apple-system,BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
}
a:link, a:visited {
  text-decoration: none;
  color: black;
}
a.active {
  color: white;
}
h1, footer, .nav ul {
  padding: 0.5rem;
}
h4, li {
  margin: 0.5rem 0;
}
hr {
  display: none;
  margin: 1rem 0;
  border: 0;
  border-top: 1px solid #ccc;
}
input[type=text] {
  width: 120px;
}

.d-block {
  display: block;
}
.container {
  display: flex;
  border-top: 1px solid #ccc;
  border-bottom: 1px solid #ccc;
}
.nav {
  min-height: 300px;
  height: 100%;
  background-color: #4285F4;
}
.nav ul {
  list-style: none;
}
.contents {
  flex: 1;
  padding: 1rem;
}

.table-search {
  border: 1px solid rgb(118, 118, 118);
  border-collapse: collapse;
  text-align: center;
}
.table-search th, .table-search td {
  padding: 0.2rem;
}
.table-search td {
  border-top: 1px solid rgb(118, 118, 118);
  min-width: 100px;
}
```

## Angular Component 만들기
header, nav.vue, footer.vue 이렇게 Component 별로 파일을 생성한다.
```sh
ng generate component layout/header
ng generate component layout/nav
ng generate component layout/footer
```

src/app/app.component.html
```diff
- <header>
-  <h1>Angular study</h1>
- </header>
+ <app-header></app-header>

- <nav class="nav">
-   <ul>
-     <li><h2>Users</h2></li>
-     <li><h2>Search</h2></li>
-   </ul>
- </nav>
+ <app-nav></app-nav>

- <footer>Copyright</footer>
+ <app-footer></app-footer>
```

src/app/app.component.ts
```ts
import { HeaderComponent } from "./layout/header/header.component";

@Component({
  imports: [RouterOutlet, HeaderComponent]
})
```
* `Standalone`이 아닌 경우 자동 `import` 된다.

## hidden, *ngIf, props
src/app/app.component.html
```html
<app-header [hidden]="true"></app-header>

<app-nav *ngIf="false"></app-nav>

<app-footer [title]="'카피라이트'"></app-footer>
```

src/app/components/footer/footer.component.html
```diff
- <footer>Copyright</footer>
+ <footer>{{title}}</footer>
```

src/app/components/footer/footer.component.ts
```diff
- import { Component, OnInit } from '@angular/core';
import { Component, Input, OnInit } from '@angular/core';
```
```ts
export class FooterComponent implements OnInit {
  @Input() title = 'Copyright';
```
**props는 부모 Component에서 자식 Component로 값을 전달 한다**

## Angular Router
```sh
ng generate component components/contents/users
ng generate component components/contents/search
```

src/app/app.component.html
```diff
  <section class="contents">
-   <div>
-     <h3>Users</h3>
-     <p>Contents</p>
-   </div>
+   <router-outlet></router-outlet>
```

src/app/app-routing.module.ts
```ts
import { UsersComponent } from './components/contents/users/users.component';
import { SearchComponent } from './components/contents/search/search.component';
```
```diff
- const routes: Routes = [];
```
```ts
const routes: Routes = [
  { path: 'users', component: UsersComponent },
  { path: 'search', component: SearchComponent },
  { path: '', redirectTo: '/users', pathMatch: 'full' }
];
```

**주소 창에서 router 바꾸어 보기**

src/app/components/nav/nav.component.html
```html
<li><h2><a routerLink="/users" routerLinkActive="active">Users</a></h2></li>
<li><h2><a routerLink="/search" routerLinkActive="active">Search</a></h2></li>
```

**여기 까지가 Markup 개발자 분들이 할일 입니다.**

## Users Service 만들기
```sh
ng generate service services/users
```
src/app/services/users.service.ts
```ts
export interface User {
  name: string
  age: string | number
}
```
```ts
export class UsersService {
  constructor() { }

  users: User[] = [];
  user: User = {
    name: '',
    age: ''
  };
}
```

### Users Component Service 주입
src/app/components/contents/users/users.component.ts
```ts
import { UsersService } from '../../../services/users.service';
```
```diff
- constructor() { }
+ constructor(public usersService: UsersService) { }
```
```ts
ngOnInit(): void {
  console.log(this.usersService.user);
  this.usersService.user.name = '';
  this.usersService.user.age = '';
}
```

src/app/components/contents/users/users.component.html
```html
<div>
  <h3>Users</h3>
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
    <input type="text" placeholder="Name" />
    <input type="text" placeholder="Age" />
    <button>Create</button>
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

## Users Service CRUD
### Create
src/app/services/users.service.ts
```ts
usersCreate(user: User) {
  this.users.push({
    name: user.name,
    age: user.age
  });
  console.log('Done usersCreate', this.users);
}
```

src/app/components/contents/users/users.component.ts
```ts
<input type="text" placeholder="Name"
  [ngModel]="usersService.user.name"
  (ngModelChange)="usersService.user.name = $event"
>
<input type="text" placeholder="Age"
  [ngModel]="usersService.user.age"
  (ngModelChange)="usersService.user.age = $event"
>
<button (click)="usersService.usersCreate(usersService.user)">Create</button>
```

### Read
src/app/services/users.service.ts
```ts
usersRead() {
  this.users = [{
    name: '홍길동',
    age: 20
  }, {
    name: '춘향이',
    age: 16
  }];
  console.log('Done usersRead', this.users);
}
```

src/app/components/contents/users/users.component.ts
```ts
ngOnInit(): void {
  this.usersService.usersRead();
```

src/app/components/contents/users/users.component.html
```diff
- <tr>
-   <td>홍길동</td>
-   <td>20</td>
```
```html
<tr *ngFor="let user of usersService.users; let index=index">
  <td>{{user.name}}</td>
  <td>{{user.age}}</td>
```

## Delete
src/app/services/users.service.ts
```ts
usersDelete(index: number) {
  this.users.splice(index, 1);
  console.log('Done usersDelete', this.users);
}
```

src/app/components/contents/users/users.component.html
```diff
- <button>Delete</button>
```
```html
<button (click)="usersService.usersDelete(index)">Delete</button>
```

### Update
src/app/services/users.service.ts
```ts
usersUpdate(index: number, user: User) {
  this.users[index] = user;
  console.log('Done usersUpdate', this.users);
}
```

src/app/components/contents/users/users.component.html
```diff
- <td>{user.name}</td>
- <td>{user.age}</td>
```
```html
<td>
  <input type="text" placeholder="Name"
    [ngModel]="user.name"
    (ngModelChange)="user.name = $event"
  >
</td>
<td>
  <input type="text" placeholder="Age"
    [ngModel]="user.age"
    (ngModelChange)="user.age = $event"
  >
</td>
```
```diff
- <button>Update</button>
```
```html
<button (click)="usersService.usersUpdate(index, user)">Update</button>
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
  axiosError(error: any) {
    console.error(error.response || error.message || error);
  };
```

src/app/services/users.service.ts
```ts
import { CommonService } from './common.service';
import axios from 'axios';
```
```diff
- constructor() {}
+ constructor(private commonService: CommonService) {}
```
```diff
usersCreate(user) {
- this.users.push({
-   name: user.name,
-   age: user.age
- })
- console.log('Done usersCreate', this.users);
```
```ts
axios.post('http://localhost:3100/api/v1/users', user).then((response) => {
  console.log('Done usersCreate', response);
  this.usersRead();
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

### Read
src/app/services/users.service.ts
```diff
usersRead() {
- this.users = [{
-   name: '홍길동',
-   age: 20
- }, {
-   name: '춘향이',
-   age: 16
- }];
- console.log('Done usersRead', this.users);
```
```ts
axios.get('http://localhost:3100/api/v1/users').then((response) => {
  console.log('Done usersRead', response);
  this.users = response.data.users;
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

### Delete
src/app/services/users.service.ts
```diff
usersDelete(index) {
- this.users.splice(index, 1);
- console.log('Done usersDelete', this.users);
```
```ts
axios.delete('http://localhost:3100/api/v1/users/' + index).then((response) => {
  console.log('Done usersDelete', response);
  this.usersRead();
}).catch((error) => {
  this.commonService.axiosError(error);
});
```

### Update
src/app/services/users.service.ts
```diff
usersUpdate(index, user) {
- this.users[index] = user;
- console.log('Done usersUpdate', this.users);
```
```ts
axios.patch('http://localhost:3100/api/v1/users/' + index, user).then((response) => {
  console.log('Done usersUpdate', response);
  this.usersRead();
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
import { CommonService } from './common.service';
import { UsersService } from './users.service';
import axios from 'axios';
```
```diff
- constructor() { }
```
```ts
constructor(
  private commonService: CommonService,
  private usersService: UsersService
) { }

searchRead(q: string) {
  const url = 'http://localhost:3100/api/v1/search?q=' + q;
  axios.get(url).then((response) => {
    console.log('Done searchRead', response);
    this.usersService.users = response.data.users;
  }).catch((error) => {
    this.commonService.axiosError(error);
  });
}
```

## Search Component Service 주입
src/app/components/contents/search/search.component.ts
```ts
import { UsersService } from '../../../services/users.service';
import { SearchService } from '../../../services/search.service';
```
```diff
- constructor() { }

- ngOnInit(): void {
- }
```
```ts
constructor(
  public usersService: UsersService,
  public searchService: SearchService
) { }

ngOnInit(): void {
  this.searchService.searchRead('');
}
```

src/app/components/contents/search/search.component.html
```html
<div>
  <h3>Search</h3>
  <hr class="d-block" />
  <div>
    <form>
      <input type="text" placeholder="Search" />
      <button>Search</button>
    </form>
  </div>
  <hr class="d-block" />
  <div>
    <table class="table-search">
      <thead>
        <tr>
          <th>Name</th>
          <th>Age</th>
        </tr>
      </thead>
      <tbody>
        <tr *ngFor="let user of usersService.users; let index=index">
          <td>{{user.name}}</td>
          <td>{{user.age}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>
```

## Search Component에서만 사용 가능한 state값 적용
src/app/components/contents/search/search.component.ts
```ts
export class SearchComponent implements OnInit {
  q = '';
```

src/app/components/contents/search/search.component.html
```diff
- <form>
-   <input type="text" placeholder="Search" />
-   <button>Search</button>
- </form>
```
```html
<form (submit)="searchService.searchRead(q)">
  <input type="text" placeholder="Search" name="q"
    [ngModel]="q"
    (ngModelChange)="q = $event"
  />
  <button>Search</button>
</form>
```

## Search Component 쿼리스트링 변경
src/app/components/contents/search/search.component.ts
```ts
import { Router } from '@angular/router';
```
```diff
constructor(
  public usersService: UsersService,
  public searchService: SearchService,
+ private router: Router
) { }
```
```ts
searchRead(q: string): void {
  this.router.navigate(['/search'], {
    queryParams: {
      q: q
    }
  });
}
```

src/app/components/contents/search/search.component.html
```diff
- <form (submit)="searchService.searchRead(q)">
+ <form (submit)="searchRead(q)">
```
`검색`, `뒤로가기` 해보기

## Search Component 새로고침 적용
```diff
- import { Router } from '@angular/router';
+ import { ActivatedRoute, Router } from '@angular/router';
```
```diff
- constructor(
-   public usersService: UsersService,
-   public searchService: SearchService,
-   private router: Router
- ) { }
```
```ts
constructor(
  public usersService: UsersService,
  public searchService: SearchService,
  private route: ActivatedRoute,
  private router: Router
) {
  this.route.queryParams.subscribe(queryParams => {
    this.q = queryParams['q'] || ''
    this.searchService.searchRead(this.q);
  });
}
```
```diff
ngOnInit(): void {
- this.searchService.searchRead('');
}
```

## Proxy 설정
proxy.conf.json
```json
{
  "/api/*": {
    "target": "http://localhost:3100"
  }
}
```

package.json
```diff
- "start": "ng serve",
+ "start": "ng serve --open --proxy-config proxy.conf.json",
```

모든 파일 수정
```diff
- http://localhost:3100/api
+ /api
```

당황 하지 말고 다시 실행
```sh
npm start
```

<!-- ## Angular for IE11
tsconfig.json
```diff
- "target": "es2015",
+ "target": "es5",
```

.browserslistrc
```diff
- not IE 11
+ IE 11
```
-->

# 수고 하셨습니다.
