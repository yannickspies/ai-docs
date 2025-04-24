# Angular v19

This document provides comprehensive information about Angular v19 features, components, and development concepts for future reference by LLMs.

## Signals

Signals are a system for reactively tracking values that are accessed during rendering and automatically updating the view when they change.

### Core Concepts

- **Signal**: A wrapper around a value that notifies consumers when that value changes
- **Signal producer**: A function that creates or derives signals
- **Signal consumer**: A function that reads signals and reacts to changes

### Types of Signals

- **Writable signals**: Created with `signal()` and can be directly updated
- **Computed signals**: Created with `computed()` and derive their value from other signals
- **Effects**: Created with `effect()` and execute side effects when signals change

### Creating and Using Signals

```typescript
import { signal, computed, effect } from '@angular/core';

// Create a writable signal
const count = signal(0);

// Read the value
console.log(count()); // 0

// Update the value
count.set(1);
// Or update based on the previous value
count.update(value => value + 1);

// Create a computed signal
const doubledCount = computed(() => count() * 2);

// Create an effect
effect(() => {
  console.log(`Count is now: ${count()}`);
  console.log(`Doubled count is now: ${doubledCount()}`);
});
```

### Linked Signals

Linked signals allow you to chain and compose signals together, creating relationships between different parts of your application state.

```typescript
import { signal, linked } from '@angular/core';

// Create signals
const firstName = signal('John');
const lastName = signal('Doe');

// Create a linked signal
const fullName = linked((read, write) => {
  // Reading
  if (read) {
    return `${firstName()} ${lastName()}`;
  }
  
  // Writing
  if (write) {
    const [first, last] = write.split(' ');
    firstName.set(first);
    lastName.set(last);
  }
});

// Using the linked signal
console.log(fullName()); // "John Doe"
fullName.set('Jane Smith');
console.log(firstName()); // "Jane"
console.log(lastName()); // "Smith"
```

### Resources

Resource signals help manage asynchronous data fetching and loading states. They simplify working with async data by handling loading, error, and success states.

```typescript
import { resource } from '@angular/core';

interface User {
  id: number;
  name: string;
}

const userResource = resource<User>(async () => {
  const response = await fetch('/api/user/1');
  return response.json();
});

// In a component template
@Component({
  template: `
    @switch (userResource.state) {
      @case ('loading') {
        <p>Loading...</p>
      }
      @case ('error') {
        <p>Error: {{ userResource.error }}</p>
      }
      @case ('success') {
        <p>User: {{ userResource().name }}</p>
      }
    }
  `
})
```

## Components

Components are the main building blocks of Angular applications. They control a portion of the screen called a view.

### Basic Component Structure

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-hello-world',
  template: '<h1>Hello, {{name}}!</h1>',
  styles: ['h1 { font-weight: normal; }']
})
export class HelloWorldComponent {
  name = 'World';
}
```

### Component Selectors

Selectors define how the component is used in templates. Angular supports three selector types:

1. **Element selectors**: `selector: 'app-component'` → `<app-component></app-component>`
2. **Attribute selectors**: `selector: '[appAttribute]'` → `<div appAttribute></div>`
3. **Class selectors**: `selector: '.app-class'` → `<div class="app-class"></div>`

### Component Styling

Angular provides several ways to add styles to components:

1. **Inline styles**:
```typescript
@Component({
  styles: ['h1 { color: blue; }']
})
```

2. **External stylesheets**:
```typescript
@Component({
  styleUrls: ['./my-component.css']
})
```

3. **Template inline styles**:
```html
<style>
  h1 { color: blue; }
</style>
```

4. **Style encapsulation modes**:
```typescript
@Component({
  encapsulation: ViewEncapsulation.Emulated // Default
  // Other options: ViewEncapsulation.None, ViewEncapsulation.ShadowDom
})
```

### Inputs

Inputs allow data to be passed from a parent component to a child component.

```typescript
@Component({
  selector: 'app-child',
  template: '<p>{{ name }}</p>'
})
export class ChildComponent {
  @Input() name: string;
  @Input('aliasName') originalName: string; // With alias
  
  // With transform function (Angular v16+)
  @Input({ transform: (value: string) => value.toUpperCase() })
  capitalizedName: string;
  
  // Required input (Angular v14+)
  @Input({ required: true })
  requiredValue: string;
}
```

### Outputs

Outputs allow child components to send data to their parent components via events.

```typescript
@Component({
  selector: 'app-child',
  template: '<button (click)="onClick()">Click me</button>'
})
export class ChildComponent {
  @Output() buttonClicked = new EventEmitter<string>();
  
  onClick() {
    this.buttonClicked.emit('Button was clicked!');
  }
}

// In parent template
<app-child (buttonClicked)="handleClick($event)"></app-child>
```

### Content Projection

Content projection allows you to insert content from a parent component into a child component.

**Single-slot projection**:
```html
<!-- Child component template -->
<div class="card">
  <div class="card-header">Header</div>
  <div class="card-content">
    <ng-content></ng-content>
  </div>
</div>

<!-- Parent usage -->
<app-card>
  <p>This content will be projected.</p>
</app-card>
```

**Multi-slot projection**:
```html
<!-- Child component template -->
<div class="card">
  <div class="card-header">
    <ng-content select="[header]"></ng-content>
  </div>
  <div class="card-content">
    <ng-content></ng-content>
  </div>
  <div class="card-footer">
    <ng-content select="[footer]"></ng-content>
  </div>
</div>

<!-- Parent usage -->
<app-card>
  <h2 header>Card Title</h2>
  <p>Main content</p>
  <button footer>Action</button>
</app-card>
```

### Host Elements

Host elements allow you to manipulate the element that hosts your component.

```typescript
@Component({
  selector: 'app-button',
  template: '<ng-content></ng-content>',
  host: {
    '[class.active]': 'isActive',
    '(click)': 'onClick()',
    'role': 'button'
  }
})
export class ButtonComponent {
  @Input() isActive = false;
  
  onClick() {
    console.log('Button clicked');
  }
}

// Alternative using HostBinding and HostListener
@Component({
  selector: 'app-button',
  template: '<ng-content></ng-content>'
})
export class ButtonComponent {
  @HostBinding('class.active') isActive = false;
  @HostBinding('attr.role') role = 'button';
  
  @HostListener('click')
  onClick() {
    console.log('Button clicked');
  }
}
```

### Component Lifecycle

Angular components have a lifecycle that begins when Angular instantiates the component class and renders the component view.

**Lifecycle Hooks**:

1. **ngOnChanges**: Called when input properties change
2. **ngOnInit**: Called once after the first ngOnChanges
3. **ngDoCheck**: Called during every change detection run
4. **ngAfterContentInit**: Called after content projection
5. **ngAfterContentChecked**: Called after every check of projected content
6. **ngAfterViewInit**: Called after component's view is initialized
7. **ngAfterViewChecked**: Called after every check of the component's view
8. **ngOnDestroy**: Called just before Angular destroys the component

```typescript
@Component(...)
export class MyComponent implements OnInit, OnDestroy {
  ngOnInit() {
    // Initialize the component
    this.subscription = someService.subscribe();
  }
  
  ngOnDestroy() {
    // Clean up resources
    this.subscription.unsubscribe();
  }
}
```

### View Queries

Queries allow a component to access child elements or directives in its template.

```typescript
@Component({
  selector: 'app-my-component',
  template: `
    <div #divRef>Hello</div>
    <child-component></child-component>
  `
})
export class MyComponent {
  // Query a DOM element
  @ViewChild('divRef') divElement: ElementRef;
  
  // Query a component
  @ViewChild(ChildComponent) childComponent: ChildComponent;
  
  // Query multiple elements
  @ViewChildren(ChildComponent) childComponents: QueryList<ChildComponent>;
  
  ngAfterViewInit() {
    // Access elements after view is initialized
    console.log(this.divElement.nativeElement.textContent);
    this.childComponent.someMethod();
  }
}
```

### DOM APIs

Angular provides safe ways to interact with the DOM:

```typescript
@Component(...)
export class MyComponent {
  constructor(
    private elementRef: ElementRef,
    private renderer: Renderer2
  ) {}
  
  ngOnInit() {
    // Unsafe direct DOM access (avoid when possible)
    this.elementRef.nativeElement.style.backgroundColor = 'red';
    
    // Preferred approach using Renderer2
    this.renderer.setStyle(
      this.elementRef.nativeElement,
      'backgroundColor',
      'blue'
    );
  }
}
```

### Component Inheritance

Components can inherit from other components to reuse functionality:

```typescript
// Base component
@Component({
  selector: 'app-base',
  template: '<div>{{message}}</div>'
})
export class BaseComponent {
  @Input() message: string;
  
  log() {
    console.log('Base component method');
  }
}

// Child component
@Component({
  selector: 'app-child',
  template: '<div>{{message}} - Extended</div>'
})
export class ChildComponent extends BaseComponent {
  ngOnInit() {
    this.log(); // Inherited method
  }
}
```

### Programmatic Rendering

Angular allows you to create dynamic views programmatically:

```typescript
@Component({
  selector: 'app-dynamic',
  template: '<ng-container #container></ng-container>'
})
export class DynamicComponent {
  @ViewChild('container', { read: ViewContainerRef }) container: ViewContainerRef;
  
  constructor(private componentFactoryResolver: ComponentFactoryResolver) {}
  
  createComponent() {
    this.container.clear();
    const factory = this.componentFactoryResolver.resolveComponentFactory(DynamicChildComponent);
    const componentRef = this.container.createComponent(factory);
    componentRef.instance.data = 'Dynamic Data';
  }
}
```

## Angular Elements

Angular Elements lets you package Angular components as custom elements (web components) that can be used in any HTML page.

```typescript
// Define the component
@Component({
  selector: 'app-hello',
  template: '<h1>Hello, {{name}}!</h1>'
})
export class HelloComponent {
  @Input() name: string = 'World';
}

// Convert to a custom element
import { createCustomElement } from '@angular/elements';

@NgModule({
  declarations: [HelloComponent],
  imports: [...],
})
export class AppModule {
  constructor(private injector: Injector) {
    // Create custom element
    const customElement = createCustomElement(HelloComponent, { injector });
    // Register the custom element
    customElements.define('hello-element', customElement);
  }
}
```

Usage in any HTML page:
```html
<hello-element name="Angular"></hello-element>
```

## Templates

Angular templates combine HTML with Angular markup that can modify HTML elements before they are displayed.

### Template Syntax

```html
<!-- Interpolation -->
<p>{{ expression }}</p>

<!-- Property binding -->
<img [src]="imageUrl">

<!-- Event binding -->
<button (click)="onClick()">Click me</button>

<!-- Two-way binding -->
<input [(ngModel)]="name">

<!-- Attribute binding -->
<button [attr.aria-label]="buttonLabel">...</button>

<!-- Class binding -->
<div [class.active]="isActive">...</div>

<!-- Style binding -->
<div [style.color]="textColor">...</div>
```

### Structural Directives

```html
<!-- Conditionals -->
<div *ngIf="condition">Shown if condition is true</div>

<!-- Loops -->
<div *ngFor="let item of items; let i = index">
  Item {{ i }}: {{ item.name }}
</div>

<!-- Switch cases -->
<div [ngSwitch]="condition">
  <div *ngSwitchCase="'case1'">Case 1</div>
  <div *ngSwitchCase="'case2'">Case 2</div>
  <div *ngSwitchDefault>Default case</div>
</div>
```

### Control Flow (New in Angular v17+)

```html
<!-- If/else blocks -->
@if (condition) {
  <p>Condition is true</p>
} @else {
  <p>Condition is false</p>
}

<!-- For loops -->
@for (item of items; track item.id) {
  <div>{{ item.name }}</div>
} @empty {
  <div>No items found</div>
}

<!-- Switch statements -->
@switch (status) {
  @case ('loading') {
    <spinner></spinner>
  }
  @case ('error') {
    <error-message></error-message>
  }
  @default {
    <content></content>
  }
}

<!-- Deferred content loading -->
@defer {
  <heavy-component></heavy-component>
} @loading {
  <spinner></spinner>
} @error {
  <error-message></error-message>
}
```

### Template Reference Variables

```html
<input #nameInput type="text">
<button (click)="greet(nameInput.value)">Greet</button>
```

### Template Expressions and Statements

Expressions:
```html
<p>{{ 1 + 1 }}</p>
<div [style.width.px]="100 + 50"></div>
```

Statements:
```html
<button (click)="count = count + 1">Add</button>
```

### Pipes

```html
<!-- Built-in pipes -->
<p>{{ date | date:'shortDate' }}</p>
<p>{{ price | currency:'USD' }}</p>
<p>{{ object | json }}</p>

<!-- Pipe chaining -->
<p>{{ text | lowercase | titlecase }}</p>

<!-- Pipes with parameters -->
<p>{{ value | slice:1:5 }}</p>
```

## Directives

Directives are classes that add additional behavior to elements in your Angular applications.

### Types of Directives

1. **Component Directives**: Directives with a template
2. **Structural Directives**: Change the DOM layout by adding and removing elements
3. **Attribute Directives**: Change the appearance or behavior of an existing element

### Creating an Attribute Directive

```typescript
import { Directive, ElementRef, Input, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input('appHighlight') highlightColor: string;
  @Input() defaultColor: string;
  
  constructor(private el: ElementRef) {}
  
  @HostListener('mouseenter')
  onMouseEnter() {
    this.highlight(this.highlightColor || this.defaultColor || 'yellow');
  }
  
  @HostListener('mouseleave')
  onMouseLeave() {
    this.highlight(null);
  }
  
  private highlight(color: string | null) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

Usage:
```html
<p [appHighlight]="'orange'" [defaultColor]="'lightblue'">
  Highlighted text
</p>
```

### Creating a Structural Directive

```typescript
import { Directive, Input, TemplateRef, ViewContainerRef } from '@angular/core';

@Directive({
  selector: '[appUnless]'
})
export class UnlessDirective {
  private hasView = false;
  
  constructor(
    private templateRef: TemplateRef<any>,
    private viewContainer: ViewContainerRef
  ) {}
  
  @Input() set appUnless(condition: boolean) {
    if (!condition && !this.hasView) {
      this.viewContainer.createEmbeddedView(this.templateRef);
      this.hasView = true;
    } else if (condition && this.hasView) {
      this.viewContainer.clear();
      this.hasView = false;
    }
  }
}
```

Usage:
```html
<div *appUnless="condition">
  This content is displayed unless the condition is true.
</div>
```

## Forms

Angular provides two different approaches to handling user input through forms: reactive forms and template-driven forms.

### Reactive Forms

Reactive forms use an explicit and immutable approach to managing form state.

```typescript
import { Component } from '@angular/core';
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

@Component({
  selector: 'app-profile-editor',
  template: `
    <form [formGroup]="profileForm" (ngSubmit)="onSubmit()">
      <div>
        <label for="firstName">First Name:</label>
        <input id="firstName" formControlName="firstName">
        <div *ngIf="firstName.invalid && firstName.touched">
          First name is required
        </div>
      </div>
      
      <div>
        <label for="lastName">Last Name:</label>
        <input id="lastName" formControlName="lastName">
      </div>
      
      <div formGroupName="address">
        <div>
          <label for="street">Street:</label>
          <input id="street" formControlName="street">
        </div>
        <div>
          <label for="city">City:</label>
          <input id="city" formControlName="city">
        </div>
      </div>
      
      <button type="submit" [disabled]="profileForm.invalid">Submit</button>
    </form>
  `
})
export class ProfileEditorComponent {
  profileForm: FormGroup;
  
  get firstName() { return this.profileForm.get('firstName'); }
  
  constructor(private fb: FormBuilder) {
    this.createForm();
  }
  
  createForm() {
    this.profileForm = this.fb.group({
      firstName: ['', Validators.required],
      lastName: [''],
      address: this.fb.group({
        street: [''],
        city: ['']
      })
    });
  }
  
  onSubmit() {
    console.log(this.profileForm.value);
  }
}
```

### Template-Driven Forms

Template-driven forms rely on directives in the template to create and manipulate the form model.

```typescript
import { Component } from '@angular/core';

interface User {
  name: string;
  email: string;
}

@Component({
  selector: 'app-user-form',
  template: `
    <form #userForm="ngForm" (ngSubmit)="onSubmit(userForm)">
      <div>
        <label for="name">Name:</label>
        <input id="name" name="name" [(ngModel)]="user.name" required #name="ngModel">
        <div *ngIf="name.invalid && name.touched">
          Name is required
        </div>
      </div>
      
      <div>
        <label for="email">Email:</label>
        <input id="email" name="email" [(ngModel)]="user.email" 
               required email #email="ngModel">
        <div *ngIf="email.invalid && email.touched">
          <div *ngIf="email.errors?.['required']">Email is required</div>
          <div *ngIf="email.errors?.['email']">Email is invalid</div>
        </div>
      </div>
      
      <button type="submit" [disabled]="userForm.invalid">Submit</button>
    </form>
  `
})
export class UserFormComponent {
  user: User = { name: '', email: '' };
  
  onSubmit(form) {
    console.log('Form submitted', form.value);
  }
}
```

## Dependency Injection

Dependency Injection (DI) is a design pattern where a class requests dependencies from external sources rather than creating them.

### Providing Services

```typescript
// Service definition
@Injectable({
  providedIn: 'root' // Makes the service available application-wide
})
export class UserService {
  getUsers() {
    return [{ id: 1, name: 'John' }, { id: 2, name: 'Jane' }];
  }
}

// Alternative providers
@NgModule({
  providers: [
    UserService, // Standard provider
    { provide: API_URL, useValue: 'https://api.example.com' }, // Value provider
    { provide: Logger, useClass: ProductionLogger }, // Class provider
    { provide: UserAPI, useExisting: UserService }, // Existing provider
    { provide: UserFactory, useFactory: userFactoryFn, deps: [Config] } // Factory provider
  ]
})
```

### Injecting Dependencies

```typescript
@Component({
  selector: 'app-user-list',
  template: `
    <ul>
      <li *ngFor="let user of users">{{ user.name }}</li>
    </ul>
  `
})
export class UserListComponent {
  users: any[] = [];
  
  constructor(
    private userService: UserService,
    @Inject(API_URL) private apiUrl: string
  ) {
    this.users = this.userService.getUsers();
  }
}
```

### Hierarchical Injection

```typescript
@Component({
  selector: 'app-parent',
  providers: [{ provide: UserService, useClass: ParentUserService }],
  template: `
    <h2>Parent Component</h2>
    <app-child></app-child>
  `
})
export class ParentComponent { }

@Component({
  selector: 'app-child',
  providers: [{ provide: UserService, useClass: ChildUserService }],
  template: `<h3>Child Component</h3>`
})
export class ChildComponent {
  // This will use ChildUserService, not ParentUserService
  constructor(private userService: UserService) { }
}
```

## Routing

Angular's Router enables navigation from one view to another as users perform tasks in your app.

### Basic Setup

```typescript
// Routes configuration
const routes: Routes = [
  { path: '', redirectTo: '/home', pathMatch: 'full' },
  { path: 'home', component: HomeComponent },
  { path: 'users', component: UserListComponent },
  { path: 'users/:id', component: UserDetailComponent },
  { path: '**', component: PageNotFoundComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

### Router Outlets and Links

```html
<!-- Router outlet where activated components are displayed -->
<router-outlet></router-outlet>

<!-- Router links -->
<a routerLink="/home">Home</a>
<a [routerLink]="['/users', user.id]">View User</a>

<!-- Active route styling -->
<a routerLink="/home" routerLinkActive="active">Home</a>
```

### Route Parameters

```typescript
@Component(...)
export class UserDetailComponent implements OnInit {
  userId: string;
  
  constructor(private route: ActivatedRoute) {}
  
  ngOnInit() {
    // Snapshot approach
    this.userId = this.route.snapshot.paramMap.get('id');
    
    // Observable approach for handling parameter changes without reloading
    this.route.paramMap.subscribe(params => {
      this.userId = params.get('id');
    });
  }
}
```

### Child Routes

```typescript
const routes: Routes = [
  {
    path: 'users',
    component: UsersComponent,
    children: [
      { path: '', component: UserListComponent },
      { path: ':id', component: UserDetailComponent }
    ]
  }
];
```

### Route Guards

```typescript
@Injectable({ providedIn: 'root' })
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}
  
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): boolean | UrlTree {
    if (this.authService.isLoggedIn()) {
      return true;
    }
    
    // Redirect to login page
    return this.router.parseUrl('/login');
  }
}

// Using the guard
const routes: Routes = [
  {
    path: 'admin',
    component: AdminComponent,
    canActivate: [AuthGuard]
  }
];
```

### Lazy Loading

```typescript
// Main routes
const routes: Routes = [
  { path: 'home', component: HomeComponent },
  { path: 'users', loadChildren: () => import('./users/users.module').then(m => m.UsersModule) }
];

// Users module routes
const userRoutes: Routes = [
  { path: '', component: UserListComponent },
  { path: ':id', component: UserDetailComponent }
];

@NgModule({
  imports: [RouterModule.forChild(userRoutes)],
  exports: [RouterModule]
})
export class UsersRoutingModule { }
```

## HTTP Client

Angular's HTTP client provides a simplified API for making HTTP requests.

### Basic Usage

```typescript
import { HttpClient } from '@angular/common/http';
import { Injectable } from '@angular/core';
import { Observable } from 'rxjs';

interface User {
  id: number;
  name: string;
  email: string;
}

@Injectable({ providedIn: 'root' })
export class UserService {
  private apiUrl = 'https://api.example.com/users';
  
  constructor(private http: HttpClient) {}
  
  getUsers(): Observable<User[]> {
    return this.http.get<User[]>(this.apiUrl);
  }
  
  getUser(id: number): Observable<User> {
    return this.http.get<User>(`${this.apiUrl}/${id}`);
  }
  
  createUser(user: User): Observable<User> {
    return this.http.post<User>(this.apiUrl, user);
  }
  
  updateUser(user: User): Observable<User> {
    return this.http.put<User>(`${this.apiUrl}/${user.id}`, user);
  }
  
  deleteUser(id: number): Observable<any> {
    return this.http.delete(`${this.apiUrl}/${id}`);
  }
}
```

### HTTP Headers and Parameters

```typescript
import { HttpHeaders, HttpParams } from '@angular/common/http';

// Using headers
const headers = new HttpHeaders({
  'Content-Type': 'application/json',
  'Authorization': 'Bearer token'
});

this.http.get<User[]>(this.apiUrl, { headers });

// Using query parameters
let params = new HttpParams()
  .set('page', '1')
  .set('limit', '10');

this.http.get<User[]>(this.apiUrl, { params });
```

### Interceptors

```typescript
import { Injectable } from '@angular/core';
import {
  HttpRequest,
  HttpHandler,
  HttpEvent,
  HttpInterceptor
} from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable()
export class AuthInterceptor implements HttpInterceptor {
  constructor(private authService: AuthService) {}
  
  intercept(
    request: HttpRequest<any>,
    next: HttpHandler
  ): Observable<HttpEvent<any>> {
    // Add authorization header
    const token = this.authService.getToken();
    if (token) {
      request = request.clone({
        setHeaders: {
          Authorization: `Bearer ${token}`
        }
      });
    }
    
    return next.handle(request);
  }
}

// Provide the interceptor
@NgModule({
  providers: [
    {
      provide: HTTP_INTERCEPTORS,
      useClass: AuthInterceptor,
      multi: true
    }
  ]
})
export class AppModule { }
```

### Error Handling

```typescript
import { catchError } from 'rxjs/operators';
import { throwError } from 'rxjs';

getUsers(): Observable<User[]> {
  return this.http.get<User[]>(this.apiUrl).pipe(
    catchError(error => {
      console.error('Error fetching users', error);
      return throwError(() => new Error('Something went wrong. Please try again later.'));
    })
  );
}
```

## Performance

### Change Detection

Angular uses Zone.js to automatically trigger change detection when:
- Events (click, input, etc.)
- XMLHttpRequest or fetch
- Timers (setTimeout, setInterval)

Strategies:
- **Default**: Check the entire component tree
- **OnPush**: Only check when inputs change or events emit

```typescript
@Component({
  selector: 'app-efficient',
  template: '...',
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class EfficientComponent {
  // Component will only update when inputs change or events emit
}
```

### Lazy Loading

Lazy loading helps reduce the initial bundle size by loading features on demand:

```typescript
// In the routing module
const routes: Routes = [
  { path: 'customers', loadChildren: () => import('./customers/customers.module').then(m => m.CustomersModule) }
];
```

### AOT Compilation

Ahead-of-Time compilation compiles templates during build time instead of runtime:

```bash
ng build --aot
```

### Memoization with Pure Pipes

```typescript
@Pipe({
  name: 'filter',
  pure: true // Default is true
})
export class FilterPipe implements PipeTransform {
  transform(items: any[], property: string, term: string): any[] {
    return items.filter(item => item[property].includes(term));
  }
}
```

### Using Trackby with ngFor

```html
<div *ngFor="let item of items; trackBy: trackById">
  {{ item.name }}
</div>
```

```typescript
trackById(index: number, item: any): number {
  return item.id;
}
```

## Angular CLI

The Angular CLI is a command-line interface tool for initializing, developing, scaffolding, and maintaining Angular applications.

### Common Commands

```bash
# Create a new project
ng new my-app

# Generate components, services, etc.
ng generate component my-component
ng generate service my-service
ng generate directive my-directive
ng generate pipe my-pipe
ng generate class my-class
ng generate interface my-interface
ng generate enum my-enum
ng generate module my-module

# Serve the application
ng serve

# Build the application
ng build

# Run tests
ng test
ng e2e

# Lint the application
ng lint

# Update Angular
ng update
```

### Configuration

Angular CLI uses configuration files:

- **angular.json**: Main configuration file
- **tsconfig.json**: TypeScript configuration
- **package.json**: NPM package configuration

### Schematics

Schematics are templates that the Angular CLI uses to generate code.

Custom schematics can be created:

```typescript
// collection.json
{
  "$schema": "../node_modules/@angular-devkit/schematics/collection-schema.json",
  "schematics": {
    "my-component": {
      "description": "Generate a custom component",
      "factory": "./my-component/index#myComponent",
      "schema": "./my-component/schema.json"
    }
  }
}

// Using custom schematics
ng generate my-collection:my-component --name=example
```

## Conclusion

This document provides a comprehensive overview of Angular v19's features, components, and development concepts. For more detailed information, refer to the official Angular documentation at https://angular.dev/.