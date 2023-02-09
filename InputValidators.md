# Input Validators

src/app.module.ts
```ts
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    FormsModule,
    ReactiveFormsModule
```

src/app/services/users.service.ts
```ts
import { FormControl, Validators } from '@angular/forms';

declare interface UserForm {
  name?: FormControl,
  age: FormControl
}

export class UsersService {
  constructor(private commonService: CommonService) {
    this.userForm.age.valueChanges.subscribe(($event: string) => {
      this.user.age = Number($event);
    });
  }

  userForm: UserForm = {
    age: new FormControl(0, [
      Validators.required,
      Validators.minLength(1),
      Validators.maxLength(3),
      Validators.pattern('^[0-9]+$')
      // Validators.pattern('^[a-zA-Z0-9]+$')
    ])
  };
}
```

src/app/components/contents/users/users.component.html
```html
<input type="text" placeholder="Age"
  [formControl]="usersService.userForm.age"
>
<div>usersService.userForm.age.value: {{usersService.userForm.age.value}}</div>
<div>usersService.user.age: {{usersService.user.age}}</div>
<div
  *ngIf="usersService.userForm.age.invalid && usersService.userForm.age.dirty"
  class="alert-danger"
>
  <div *ngIf="usersService.userForm.age.errors?.required">
    usersService.userForm.age is required.
  </div>
  <div *ngIf="usersService.userForm.age.errors?.minlength">
    usersService.userForm.age must be at least 1 characters long.
  </div>
  <div *ngIf="usersService.userForm.age.errors?.maxlength">
    usersService.userForm.age must be at most 3 characters long.
  </div>
  <div *ngIf="usersService.userForm.age.errors?.pattern">
    usersService.userForm.age can only contain alphanumeric.
  </div>
</div>
<button
  (click)="usersService.usersCreate()"
  [disabled]="!usersService.userForm.age.valid"
>Create</button>
```

src/styles.scss
```scss
input.ng-dirty.ng-invalid {
  border: 2px solid red;
}
input.ng-dirty.ng-valid {
  border: 2px solid green;
}
input:focus {
  outline: none;
}
.alert-danger {
  color: red;
}
```
