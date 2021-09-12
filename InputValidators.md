# Input Validators

src/app.module.ts
```ts
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  imports: [
    FormsModule,
    ReactiveFormsModule
```

src/app/services/members.service.ts
```ts
import { FormControl, Validators } from '@angular/forms';

declare interface MemberForm {
  name?: FormControl,
  age: FormControl
}

export class MembersService {
  constructor(private commonService: CommonService) {
    this.memberForm.age.valueChanges.subscribe(($event: string) => {
      this.member.age = Number($event);
    });
  }

  memberForm: MemberForm = {
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

src/app/components/contents/members/members.component.html
```html
<input type="text" placeholder="Age"
  [formControl]="membersService.memberForm.age"
>
<div>membersService.memberForm.age.value: {{membersService.memberForm.age.value}}</div>
<div>membersService.member.age: {{membersService.member.age}}</div>
<div
  *ngIf="membersService.memberForm.age.invalid && membersService.memberForm.age.dirty"
  class="alert-danger"
>
  <div *ngIf="membersService.memberForm.age.errors?.required">
    membersService.memberForm.age is required.
  </div>
  <div *ngIf="membersService.memberForm.age.errors?.minlength">
    membersService.memberForm.age must be at least 1 characters long.
  </div>
  <div *ngIf="membersService.memberForm.age.errors?.maxlength">
    membersService.memberForm.age must be at most 3 characters long.
  </div>
  <div *ngIf="membersService.memberForm.age.errors?.pattern">
    membersService.memberForm.age can only contain alphanumeric.
  </div>
</div>
<button
  (click)="membersService.membersCreate()"
  [disabled]="!membersService.memberForm.age.valid"
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
