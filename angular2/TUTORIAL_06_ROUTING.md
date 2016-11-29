# Angular 2: Tour of Heroes

## Routing


Angular 2 applications can handle routing using the provided **RouterModule**.

> The Angular router is an external, optional Angular NgModule called RouterModule. The 
> router is a combination of multiple provided services (RouterModule), multiple directives
> (RouterOutlet, RouterLink, RouterLinkActive), and a configuration (Routes).
>
> -- <cite>[Angular 2 Official Documentation](https://angular.io/docs/ts/latest/tutorial/toh-pt5.html)</cite>

----------
### Current Project Structure

At this point in the tutorial, the file structure for our application should appear as follows:

```bash
angular-tour-of-heroes
|-app
|  |-app.component.ts
|  |-app.module.ts
|  |-hero.service.ts
|  |-hero.ts
|  |-hero-detail.component.ts
|  |-main.ts
|  |-mock-heroes.ts
|-node_modules ...
|-index.html
|-package.json
|-styles.css    # Not vital to the application, just styling
|-systemjs.config.js
|-tsconfig.json
```
----------
### RouterModule

Angular 2 applications are made up of _components_ that generate _views_. Navigating
  between these views can be done with the **RouterModule**, which is imported into
  the application at the **NgModule** level. By specifying it as an import inside
  the NgModule decorator, we are giving every component that is bootstrapped by
  this module access to the RouterModule.

**Steps**:

  1. Import **RouterModule** into application at **NgModule** level, and declare it
    as an import inside the @NgModule decorator.


  2. Set base href in _head_ section of _index.html_ page.
      - Ex: `<base href="/">`


  3. Create an array of _route definitions_, objects with `path` and `component` 
    properties set to a _URL and the corresponding component_ which should be displayed when 
    navigating to said URL. The array is of type **Route**, which can be imported
    from `@angular/router` along with the RouterModule.


  4. Create a link to the defined path via a `routerLink` anchor tag and place it in
    the application's template.
      - Ex: `<a routerLink="/heroes">Heroes</a>`


  5. Provide a `<router-outlet>` element at the bottom of the application's template.
    **RouterOutlet** is a directive provided by the **RouterModule** where
    the components will generate their views when called upon by a **RouterLink**.


----------
### Handling Redirects

To define a redirect route, we create another _route definition_, but this time
  it will have the properties `path, redirectTo,` and `pathMatch`. The `path` 
  property should be set to the path which you wish to _redirect from_, and the
  `redirectTo` property should be set to the path which you wish to _redirect to_
  (pretty self explanatory, but hey, there you go). `pathMatch` can either be 
  'full' or 'prefix', where 'full' will redirect if the _remaining segments
  of the relative URL_ match the `path` property and `prefix` will redirect if the
  _relative URL **begins** with the redirect route's prefix path_. Wordy? Yes. Hard to
  understand? Replace the `pathMatch` property with 'prefix' and try it!


----------
### URL Parameters

To define a path which should carry a parameter we create a standard _route
  definition_. For the `path` property, simply specify the parameter with a colon (:)
  beforehand.

  - Example:
      ```typescript
      {
        path: 'detail/:id', // The colon indicates that :id will be replaced with a parameter.
        component: HeroDetailComponent
      },...
      ```
To pass the value of the parameter to a component, the component will import `ActivatedRoute`
  and `Params` from `@angular/router`. The component should have the ActivatedRoute
  injected into it's constructor and saved as a private field, which it will
  utilize when this component is initiated via the `ngOnInit` method. When initiated,
  the `ngOnInit` method will use the ActivatedRoute service to gather it's parameters.


----------
### But Wait, There's More!

The Angular 2 RouterModule is vast and very, very capable. To develop a thorough
  understanding of the router is well outside the scope of the tutorial, however,
  my summary (aka this file) is _even more limited_. The above information will
  most likely generate more questions than answers, but that is intentiona. Once
  again, **the router is too large to cover in a tutorial**. We've covered
  the essentials of setting up routing; import the module, define some routes,
  provide a router outlet.
