# Input Validators

src/app.module.ts
```ts
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    FormsModule,
    ReactiveFormsModule
```

src/app.component.ts
```ts
import { FormControl, Validators } from '@angular/forms';

export class AppComponent {
  username = new FormControl('', [
    Validators.required,
    Validators.minLength(4),
    Validators.maxLength(20),
    Validators.pattern('^[a-zA-Z0-9]+$'),
  ]);
```

src/app.component.html
```html
<input type="text" [formControl]="username">
<div *ngIf="username.invalid && username.dirty" class="alert-danger">
  <div *ngIf="username.errors.required">
    username is required.
  </div>
  <div *ngIf="username.errors.minlength">
    username must be at least 4 characters long.
  </div>
  <div *ngIf="username.errors.maxlength">
    username must be at most 20 characters long.
  </div>
  <div *ngIf="username.errors.pattern">
    username can only contain alphanumeric characters.
  </div>
</div>
```

app.component.scss
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
