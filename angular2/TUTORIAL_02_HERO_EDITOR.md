# Angular 2: Tour of Heroes

## The Hero Editor

----------
### Setting Up Local Dev Environment

Create a local development environment for this application by cloning the official
  [Quickstart](https://github.com/angular/quickstart.git) repo. The repo provides **FAR
  MORE** than what is needed. The files necessary for this app are:

  - The `app` folder
    - Only need `app.component.ts, app.module.ts`, and `main.ts`. 
  - The `.gitignore` file
    - Preconfigured .gitignore file. Why not?
  - `index.html`
    - Initial app loading point
  - `package.json`
    - Angular 2 is split up into multiple modules. Specifying each module necessary
      for a particular application is a great way to reduce clutter, load-time, and
      dev-tool reliance, but for now we can safely assume that we'll be using
      everything listed within the 'dependencies' section of the `package.json`
      file. The `devDependencies` section has a fair amount of additional
      packages for testing purposes; if you know which ones to remove, go for it.
      If you're not sure, _just leave it be._ There is also a section, `scripts`, which
      contains predefined NPM scripts that will be used frequently throughout Angular 2
      development.
  - `styles.css` (not mandatory)
    - Styling to make the application less boring.
  - `systemjs.config.js`
    - Angular 2 takes advantage of the latest JavaScript syntax; one of the major 
      benefits of this is that we can now _modularize_ our application, or split it up into
      multiple tiny manageable pieces rather than one big chunk of
      JavaScript hell. To load the application into the browser correctly, we use a _module
      loader_ called SystemJS. This is not the only option available, but is the one 
      with the most support in Angular 2 documentation. The `config.js` file contains
      everything necessary to load Angular 2 applications appropriately in the browser.
  - `tsconfig.json`
    - Angular 2 apps can be written in JavaScript, Dart, or TypeScript. The framework itself
      is built on TypeScript, so it seems logical to use it as well. The `tsconfig.json` file
      tells the TypeScript compiler how to transpile the TypeScript code into the appropriate
      format of JavaScript (ES5, ES6, Node, etc.).
  - `tslint.json`
     - TSLint is just a linter for TypeScript. Make sure your formatting and whatnot is correct.


----------
### Displaying Data (One-Way Data Binding)

Store data as _properties of a class_:

```TypeScript
export class AppComponent {
  title = 'Tour of Heroes'; // This is a property, title, of the class AppComponent.
  hero = 'Windstorm';       // This is a property, hero, of the class AppComponent.
}
```

Display data by inserting one of the property names in between double curly brackets `{{}}`
 somewhere inside the component HTML template. This is known as _interpolation_, and is 
 a method of one-way binding.

----------
### Displaying Data (Two-Way Data Binding)

If we try to use the double curly brackets as a way to receive data back from the user,
  it won't work. Interpolation provides _one way_ data binding, meaning that the data will always
  reflect what is on server-side/whatever information the application already has. In order to
  receive user inputs as well, we need to set up _two way_ data binding.

In order to take advantage of two-way data binding, we need to **import the `FormsModule` 
  into our application**. This is done at the app module level; by importing the `FormsModule` at
  the apps module level and providing it as an argument to the 'imports' property of 
  `@NgModule`, it is made avaiable to the rest of the application bootstrapped 
  from this particular module.

Before & After HTML Template:

```html
<!-- Before module import -->
<h1>{{title}}</h1>
<h2>{{hero.name}} details!</h2>
<div><label>id: </label>{{hero.id}}</div>
<div>
  <label>name: </label>
  <input value="{{hero.name}}" placeholder="name">
</div>

<!-- After module import-->
<h1>{{title}}</h1>
<h2>{{hero.name}} details!</h2>
<div><label>id: </label>{{hero.id}}</div>
<div>
  <label>name: </label>
  <input [(ngModel)] = "hero.name" placeholder = "name" />
</div>
```
