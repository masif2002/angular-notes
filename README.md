# Angular
Personalized Notes for [Angular](https://angular.io/)

## Commands
```
npm i -g @angular/cli
```
* Installs angular
```
ng new appname
```
* Creates a new project
```
ng serve
```
* Opens app in a local server
```
ng generate component name 
```
* Generates a new component
## VScode Extensions
```
Angular Language Service  |  for syntax highlighting
Angular Console | GUI for Angular CLI
```
## Resources
* [Angular CLI](https://angular.io/cli)
* [Angular CDK](https://material.angular.io/cdk/categories)
* [Angular Material](https://material.angular.io/) - Material Design components for Angular
* [Angular Color Pallette Generator](http://mcg.mbitson.com/#!?mcgpalette0=%233f51b5)
* [Angular Pipes](https://fknop.gitbook.io/angular-pipes/#angular-pipes)
* [Angular Material Schematics](https://material.angular.io/guide/schematics) - Allows you to generate components styled (by default) with Material
* [Angular Icons](https://www.angularjswiki.com/angular/angular-material-icons-list-mat-icon-list/)
* [AngularFire](https://github.com/angular/angularfire) - Firebase for Angular
* [Angular Universal](https://angular.io/guide/universal) - SSR with Angular
* [Prerendering in Angular](https://angular.io/guide/prerendering)
* [Observables in Angular](https://angular.io/guide/observables)


### Troubleshoot
* [ng add fails due to peer dependency issue](https://github.com/ngrx/platform/issues/2830)
* [Budget limit increased](https://stackoverflow.com/questions/53995948/warning-in-budgets-maximum-exceeded-for-initial)



## Notes
### Angular Material
```
ng add @angular/material
```
* When adding packages in Angular prefer to use `ng add` instead of `npm install` because `ng` automaticaly modifies imports and other stuff for you
* When you add a Material dark theme, it only applies to Material components 

### File Structure
* `tsconfig` files tell how to compile TS code to vanillaJS that can be used by your browser 
* `angular.json` is responsible for the behaviour of the CLI
* `main.ts` is where the application starts. It's like the `index.js` file in React
* For a component:
    * `*.component.ts` contains the logic for the component that can be used in `*.component.html` file

### Binding 
* You can bind variables written in `.component.ts` file to `.component.html` file 
    ```ts
    export class AppComponent {
    title = 'kanban';
    }
    ```
    ```html
    <button	>{{ title }}</button>
    ```
* You can also acess these variables to be used in attributes
    ```ts
    export class AppComponent {
    clicked = true;
    }
    ```
    ```html
    <button	[disabled]="clicked">Submit</button>
    <!-- This "disabled" is a proprty given by HTML not Angular -->
    ```
* Similarly, you can also bind html elements to events
    ```ts
    export class AppComponent {
        clicked = false;

        handleClick() {
            this.clicked = true;
        }
    }
    ```
    ```html
    <button	[disabled]="clicked" (click)="handleClick()">Submit</button>
    ```

### Structural Directives
* Any directive that starts with a `*` is a structural directive and it controls how html is rendered in the DOM. 
#### ngIf
* The element is rendered only if the value in the right is `true`
```html
<div *ngIf="clicked">
    <p>Lorem ipsum dolor</p>
</div>
```
##### ngFor
* Used to loop over an array
```ts
export class AppComponent {

  products = [
    {
      name: 'Iphone',
      price: '$999' 
    },
    {
      name: 'IMac',
      price: '$1499' 
    }
  ]
  
}
```
```html
<div *ngFor="let product of products">
    <h1>{{ product.name }}</h1>
    <p>{{ product.price }}</p>
</div>
```

#### ngClass
```html
<div *ngFor="let product of products">
    <h1>{{ product.name }}</h1>
    <p 
    [ngClass]="{
      'green': product.price < 1000,
      'red': product.price > 1000
      }"
    >
        {{ product.price }}
    </p>
</div>
```
* A specific class is applied to the element if RHS is true 

#### ng-content
* This directive allows you to insert (or project) content inside a component
```html
<!-- example.component.html -->
<h2>Single-slot content projection</h2>
<ng-content></ng-content>
```
```html
<!-- app.component.html  -->
<example>
  Hi <!-- Inserted content inside component -->
</example>
```
> https://angular.io/guide/content-projection
### Custom Directives
* You can also create your own custom directives and use them in your components
```
ng generate directive [name]
```

### Pipes
* Pipes are some kind of JS functions that can be directly used inside your html files
> Pipes can be used for interpolated values in your html (interpolated values are values in double curly braces. Ex: {{ name }})
* You can create your own pipe but also use pipes created by others in the community. You can find a list of pipes [here](https://fknop.gitbook.io/angular-pipes/#angular-pipes)
* Create your own pipes using the command `ng generate pipe [name]`
* Pipes are generally a function that take in an argument, manupulate it and return a value
  ```ts
  // Custom Pipe

  export class MyPipe implements PipeTransform {

    transform(value: number): string {
      return value.toString().substr(2, 2)
    }

  } 
  ```

### Component Lifeycle
* Initially, the first thing that runs when the component is called is the `constructor()` method. We eould usually have (or inject) dependencies? in this constructor
* Just like in React, we have some lifecycle methods in angular as well. Like `componentDidMount`, `componentDidUpdate` and `componentWillUnmount()`, we have:
```ts
export class HomeComponent implements OnInit, OnDestroy, AfterViewInit, DoCheck {

  constructor() {}

  ngOnInit() {
    // Runs when the component is mounted
    // Initial setup code of the component goes here
  }

  ngDoCheck() {
    // Runs when any event is detected (like onMouseEnter, onClick ... )
  } // like virtual DOM

  ngAfterViewInit() {
    // Runs after Child Components are loaded
  }

  ngOnDestroy() {
    // Runs when the component is unmounted
  }
}
``` 
* For the `DoCheck` lifecycle method, Angular uses `zone.js` behind the scenes which checks for event and asynchronous activites which then re renders the component if any changes take place

### Smart Component vs Dumb Component
* A **Dumb Component** is the component only deals with UI and not the logic of the component. Usually these are child components that does this UI job. Parent components that take care of the actual logic are called **Smart Components**

### Service
* A service is something that you create that contains functions or data that you would like to use across your entire application. 
* You, then use the service to inject the data and methods into different components
* Create a service using `ng generate service [name]` 

### Modules
* Angular offers the option to modularize your code with `modules`
* When you create a new component, you can namespace it under a module. So, now the newly created component will only be available in that particular module
* If you need to share the component from one module to another module, you need to export it from the current module and import it in the module you want to use the component
```ts
@NgModule({
  // Components defined in this module
  declarations: [
    FooComponent
  ],
  // Components used in this module
  imports: [
    CommonModule
  ],
  // Components that can be imported by other modules
  exports: [
    FooComponent
  ],
})
```
* Create a new module using `ng generate module [name]`

### Template Variables
```html
<mat-sidenav #drawer class="sidenav" fixedInViewport>
  <mat-toolbar>Menu</mat-toolbar>
  <mat-nav-list>
    
    <a mat-list-item routerLink="/" (click)="drawer.close()">Home</a>
    <a mat-list-item routerLink="/login" (click)="drawer.close()">Login</a>
    <a mat-list-item routerLink="/kanban" (click)="drawer.close()">Kanban Demo</a>
    <a mat-list-item routerLink="/customers" (click)="drawer.close()">SSR Demo</a>
  </mat-nav-list>
</mat-sidenav>
```
* Here `#drawer` is the template variable that is referenced later to close the navbar

> https://angular.io/guide/template-reference-variables

### Routing Setup
* You need to set up the routes which should point to a component. This must be done in the `app-routing.module.ts` file
```ts
const routes: Routes = [
  {path: '', component: HomePageComponent}
];
```
* Here `HomePageComponent` gets displayed on the `/` route

#### routerLinkActive directive
```html
<a routerLink="/" routerLinkActive="some-css-class">Home page</a>
```
* Here, routerLink is just like a normal `href` in an `a` tag. However we can wrap it in square brackets to provide a dynamic route. Now, it expects an array of URL segments as shown below
  ```html
  <a [routerLink]="['/product', id]">Home page</a>
  ```

### Lazy Loading
```ts
// app-routing.module.ts
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HomePageComponent } from './home-page/home-page.component'

const routes: Routes = [
  {path: '', component: HomePageComponent},
  {
    // loadChildren does lazy loading
    path: 'login', loadChildren: () =>  import('./user/user.module').then(m => m.UserModule)
  }
];
```
* Here, we are not explicitly importing the user module like we are importing the homepage component. The Module is lazy loaded only when the user goes to the `/login` path 

### Reactive Forms
* Reactive forms are reactive to the input of your forms. You can easily react to the changes happening inside your forms
```ts
import { FormBuilder, FormGroup, Validators, } from '@angular/forms';


export class EmailLoginComponent implements OnInit {
  form: FormGroup;
  
  constructor(private fb: FormBuilder) {}
  
  ngOnInit() {
    this.form = this.fb.group({
      email: [[''], [Validators.required, Validators.email]],
      passwoord: [[''], [Validators.required, Validators.minLength(8)]],
      passwordConfirm: [[''], []]
    })
  }

  async onSubmit() {
    // Logic to submit form
  }

}
```
* The first parameter for the form inputs are the default values. Here for all the three inputs, the default value is en empty string (`['']`)
* The second parameter is the validators. So, in this form, the email and password fields are required inputs 

```html
<form [formGroup]="form" (ngSubmit)="onSubmit()">
  <input
    formControlName="email"
    type="email"
    autocomplete="off"
    appearance="outline"
  />

  <input
    matInput
    formControlName="passwoord"
    type="password"
    autocomplete="off"
  />

  <input
    matInput
    formControlName="passwordConfirm"
    type="password"
    autocomplete="off"
  />

  <button
    *ngIf="!isPasswordReset"
    type="submit"
  >
    Submit
  </button>
</form>
```
* Here, the `[formGroup]` refers to the `form` variable we created in the TypeScript file and the `formControlName` refers to the name of the input fields we defined in the TS file

### Auth Guard
* Gaurds offered by Angular allows you to guard the routes that requires authentication
* Steps to guard your routes
  1. generate a gaurd by running `ng generate guard [name]`
  2. Add logic to your guard. Check if the user exists. Else, do something (like a popup message or a snackbar)
  3. Go to your `routing.ts` file where you need to guard your routes and apply the guards to your routes (`canActivate: [AuthGuard]`)

### Server Side Rendering
* In Angular, we have **Angular Universal** for SSR which is like Next.js for React
* And, to use render files on a NodeJS server, we can use `Express` or `NestJS`
```
ng add @nestjs/ng-universal

npm run build:ssr
npm run serve:ssr
```
* After running `npm run build:ssr`, two modules, one for _server_ and one for _browser_ are created in the `dist` folder (build folder)
#### Prerendering 
* Prerendering is rendering all the routes well in advance to serve to the users. We can also render dynamic routes (not to be confused with dynamic pages)
```
ng run [name-of-the-app]:prerender 
```
* `--routes /route1 /route2` flag can be added in order to specify dynamic routes  
* `--routes-file routes.txt` flag can be used to render list of routes in a text file
* Prerendering also creates a new build folder from scratch. So the app can be served using `npm run serve:ssr` 

### Miscellaneous
* You add custom theme in `styles.scss` for Angular Material
* Using `public` keyword in the constructor (`.ts` file) allows you to access the variable inside html template 