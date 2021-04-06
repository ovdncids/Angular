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
