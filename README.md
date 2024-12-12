### New Module Creation Using ng

- ng generate module home
- ng generate module reservation

### Creating Component Inside A Module

- ng generate component home --module=home
- ng generate component reservation-form --module=reservation
- ng generate component reservation-list --module=reservation

### Creating Service 
- ng generate service reservation/reservation

### Creating Interface
- ng generate i model/reservation

## NgModule

#### What is @NgModule?

- NgModule is a decorator in Angular that defines a module.
- Modules are used to organize your application into cohesive blocks of functionality. 
- Every Angular application has at least one root module, typically called AppModule, which is the entry point of the app.

``` js
@NgModule({
  declarations: [
    AppComponent // Components that belong to this module.
  ],
  imports: [
    BrowserModule // Other Angular modules required for this module.
  ],
  providers: [], // Services available throughout the module.
  bootstrap: [AppComponent] // The root component to start the app.
})
export class AppModule { }

```

#### What does NgModule do?

It provides metadata (configuration) for the Angular module. The metadata tells Angular:
 -  Which components, directives, or pipes belong to this module (declarations).
 -  Which other modules this module depends on (imports).
 -  Any global services required (providers). 
 -  The root component to bootstrap when the app starts (bootstrap).


## BrowserModule

#### What is BrowserModule?

- BrowserModule is an Angular module that is essential for running your application in the browser.
- It provides the core functionality needed for rendering components, handling user interactions, and other browser-specific features.

``` js
imports: [
    BrowserModule // Other Angular modules required for this module.
  ],
```

#### Why do we need to import BrowserModule?

- It includes directives and services necessary for bootstrapping and running an Angular app in a browser.
- For example:
    - Directives like *ngIf and *ngFor are part of BrowserModule.
    - The service required to run the app in the browser, such as DomSanitizer and Renderer, are provided by BrowserModule.

## imports 

- The imports array tells Angular which other Angular modules or third-party modules your application depends on.
- If you use any functionality from a module, you must import it here. Examples
    - If you need forms, import FormsModule.
    - If you want routing, import RouterModule.

```js
imports: [
  BrowserModule,  // Core Angular functionality for browsers.
  FormsModule,    // For building forms in the app.
  HttpClientModule // For making HTTP requests.
]
```
--- 

## providers: []

#### What is providers?
- The providers array is used to register services that the module will provide.
- Services are classes that handle business logic, data fetching, or state management and are typically injected into components using Angular's dependency injection system.

```js
providers: [
  UserService,  // A custom service to manage user data.
  AuthGuard     // A service for guarding routes.
]
```

If we had a service like UserService, we would register it here to make it available to the entire app.

--- 


## bootstrap: [AppComponent] 

#### What is bootstrap?
The bootstrap array defines the root component that Angular should load and bootstrap (initialize) when starting the app.

```js
bootstrap: [AppComponent]
```
When the app starts, Angular will:

- Render the AppComponent template.
- Load child components referenced within AppComponent

#### Why do we use AppComponent here?
- AppComponent is the main component of your app, where the entire application starts rendering.
- It acts as the entry point, and all other components are loaded inside it.

## Example of a AppModule

```js
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { HttpClientModule } from '@angular/common/http';
import { AppComponent } from './app.component';
import { UserService } from './user.service';
import { HeaderComponent } from './header/header.component';

@NgModule({
  declarations: [
    AppComponent,     // Main app component
    HeaderComponent   // A child component
  ],
  imports: [
    BrowserModule,    // For running in the browser
    FormsModule,      // For template-driven forms
    HttpClientModule  // For making HTTP requests
  ],
  providers: [
    UserService       // A service available throughout the app
  ],
  bootstrap: [AppComponent] // App starts with AppComponent
})
export class AppModule { }

```

## Angular Module Metadata

The `NgModule` decorator in Angular uses metadata to configure the module. Below is a breakdown of the key properties:

| **Property**   | **Description**                                                                                       |
|-----------------|-----------------------------------------------------------------------------------------------------|
| **declarations** | Lists all the components, directives, and pipes that belong to this module.                        |
| **imports**      | Lists all the Angular modules or third-party modules this module depends on.                       |
| **providers**    | Registers services (business logic) that are shared across the module.                             |
| **bootstrap**    | Specifies the root component that Angular should render and bootstrap at startup.                  |

--- 
## Routing in Angular

```js
app.module.ts

import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';

// Importing the AppRoutingModule, which manages the application's routes.
import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';


@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule, // Handles the routing configuration for navigating between views.
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { } // Exporting the AppModule so it can be used by the main entry file (main.ts).

```

#### AppRoutingModule:

- This is the dedicated module where all the application's routing logic resides.
- It defines routes (URL paths) and maps them to specific components.
- Keeps the routing configuration separate from the main AppModule, ensuring modularity and cleaner code.

#### Purpose of AppRoutingModule in imports:

- By including AppRoutingModule in the imports array, Angular registers the routes defined in AppRoutingModule and enables navigation functionality across the app.

#### Impact of Not Adding AppRoutingModule:

- If AppRoutingModule were not imported, Angular wouldn’t recognize the defined routes, and navigation between views wouldn’t work.

## App Routing Module

```js
import { NgModule } from '@angular/core';

// Importing Angular's RouterModule for configuring and providing routing in the app.
// Importing Routes, which is a type for defining route configurations.
import { RouterModule, Routes } from '@angular/router';

// Importing the components to which different routes will point.
import { HomeComponent } from './home/home.component';
import { ReservationListComponent } from './reservation-list/reservation-list.component';
import { ReservationFormComponent } from './reservation-form/reservation-form.component';

// Defining an array of routes, where each route specifies:
// 1. The path in the URL.
// 2. The component to be displayed when that path is accessed.
const routes: Routes = [
  {path: "", component: HomeComponent},
  {path: "list", component: ReservationListComponent},
  {path: "new", component: ReservationFormComponent}
];

// Angular module decorator that marks this class as an Angular module.
@NgModule({

  /**
   * Importing RouterModule and applying the route configurations using `.forRoot(routes)`.
   * The `.forRoot()` method configures the root-level routes and sets up the router service.
   **/
  imports: [RouterModule.forRoot(routes)],
  
  /**
   *  Exporting RouterModule so it can be used in other modules (e.g., AppModule).
   *  This allows the router directives like `routerLink` and `router-outlet` to be available app-wide.
   **/
  exports: [RouterModule]
})


export class AppRoutingModule { }

```

## Angular Routing in `AppRoutingModule`


### **1. Routes Array**
- Defines the navigation paths (`path`) and the components (`component`) displayed for those paths.
- Example: Navigating to `/list` in the browser URL displays the `ReservationListComponent`.

### **2. `RouterModule.forRoot(routes)`**
- Sets up the router with the defined routes at the application's root level.
- Ensures the app has a single instance of the router.

### **3. Exporting `RouterModule`**
- Makes the routing functionality, such as `<router-outlet>` and `[routerLink]`, available for use throughout the app.

### **4. Default Route (`""`)**
- The `""` path acts as the default and is typically used to show a landing or home page.
- Example: The `HomeComponent` is displayed when no specific route is provided.

### **5. Why Use a Separate Routing Module?**
- Keeps routing logic modular and separate from the main application logic.
- Helps maintain cleaner and more maintainable code as the app grows.


## Defining Route in Main Component 

```js
app.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'hotel-app';
}

```

```html
app.component.html
<router-outlet></router-outlet>
```

# Angular Routing with `<router-outlet>`

## Overview
The `<router-outlet>` directive in Angular acts as a placeholder in the DOM where routed components are dynamically rendered based on the current route. This allows for seamless navigation and dynamic content replacement in your application.

---

## How It Works

| **Component/File**       | **Description**                                                                                  |
|---------------------------|--------------------------------------------------------------------------------------------------|
| **`app.component.html`** | Contains `<router-outlet>` as a placeholder for routed views.                                    |
| **`AppRoutingModule`**    | Defines the routes array, mapping paths to their corresponding components.                      |
| **Router Module**         | Dynamically loads and renders the component associated with the current URL inside `<router-outlet>`. |

---

## Steps

1. **Define Routes**:
   - Routes are specified in `app-routing.module.ts` using the `Routes` array:
   ```typescript
   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'list', component: ReservationListComponent },
     { path: 'new', component: ReservationFormComponent }
   ];


2. **Place <router-outlet> in app.component.html**:
   ```html
   <router-outlet></router-outlet>
   ```

# Angular Route Matching and Dynamic Content Replacement

## Route Matching

Angular uses the `Router` to match the browser's URL with the defined routes in your application. Based on the URL, it dynamically loads the corresponding component.

### Example
| **URL**    | **Component Rendered**       |
|------------|-------------------------------|
| `/`        | `HomeComponent`              |
| `/list`    | `ReservationListComponent`   |
| `/new`     | `ReservationFormComponent`   |

---

## Dynamic Content Replacement

- The `<router-outlet>` directive in `app.component.html` serves as a placeholder for routed components.
- When a route is matched, Angular renders the template of the associated component inside the `<router-outlet>`.

### Workflow
1. User navigates to a URL (e.g., `/list`).
2. Angular matches the route (`/list`) with the `routes` array defined in `AppRoutingModule`.
3. The `ReservationListComponent` template is dynamically inserted into the `<router-outlet>`.

### Code Example

#### `app.component.html`
```html
<router-outlet></router-outlet>
