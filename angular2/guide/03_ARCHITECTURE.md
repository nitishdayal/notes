# Angular 2: Guide

## Architecture Overview

Angular 2 applications consist of many moving pieces and bits. Approaching this without
  guidelines can be overwhelming and messy; thankfully, we have the Angular 2 documentation
  to explain how these pieces intertwine and work together to create a fully-fledged
  application.

## Table Of Contents 

- [High-Level Overview](#high-level-overview)
- [Modules](#modules)
- [Components](#components)
- [Templates](#templates)
- [Metadata](#metadata)
- [Data binding](#data-binding)
- [Directives](#directives)
- [Services](#services)
- [Dependency Injection](#dependency-injection)

### High-Level Overview

Angular 2 is a client-side framework that consists of several libraries. This provides an
  added benefit of _only using the pieces you need_ as opposed to pulling in an entire library
  and using only a portion of it's functionality.

Angular 2 applications are written as HTML _templates_ with Angular-specific syntax, writing
  _component_ classes that manage individual templates and adding _services_ where application
  logic is needed. Related components and services are bundled into _modules_.

The module containing the main application, the _root module_, is then bootstrapped to the main
  HTML page, at which point the Angular compiler takes over to handle presentation and 
  user interaction as per the instructions defined within the application's components.

----------

## Modules

In the not-so-distant past, JavaScript applications had **terrible** load times, because
  the entire application would need to be processed by the browser's JavaScript parser
  before anything could be displayed; fine for small applications, unbearable for anything
  even remotely big. Splitting our application into _modules_ allows for the application to
  be loaded in chunks as needed, rather than having to process everything at once.

Angular apps are modular; Angular provides it's own modularity system with **NgModules**.
  All Angular apps have at least one module, usually named `AppModule`, known as the _root
  module_. It is the application's starting point. In smaller applications it is possible
  to have the _root module_ as the only module, however most applications will also have 
  multiple _feature modules_ which contain related application data and logic.

**Angular modules are classes with an @NgModule decorator.** The @NgModule decorator 
  function takes one metadata _object_ as an argument. The object has property
  values that describe the module. The most common properties are:

  - **declarations**: the _view classes_ (components, directives, pipes) that belong to
    a given module.
  - **exports**: Declarations which can be used by component templates in other modules.
  - **imports**: Modules that contain _exported classes needed by components in **this**
    module_.
  - **providers**: Service creators which a given module contributes to the _global 
    collection of services_, which are then available throughout the rest of the application.
  - **bootstrap**: Only done by the _root module_. Acts as the starting/loading point for 
    the rest of the application.

Example:
```TypeScript
// app/app.module.ts
import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';


@NgModule({
  imports:      [ BrowserModule ],
  providers:    [ Logger ],
  declarations: [ AppComponent ],
  exports:      [ AppComponent ],     // Strictly for example's sake; root module does not export. It's the root.
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```

So in this example, we have a module which has declared it's AppComponent. Assuming that the
  AppComponent is set up correctly and we're ready to go, the final step is to _bootstrap the
  **AppModule** in a main.ts_ file.

Example:
```TypeScript
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';
import { AppModule } from './app.module';

platformBrowserDynamic().bootstrapModule(AppModule);
```

### Angular Modules vs. JavaScript Modules

Angular modules and JavaScript modules are _separate but complementary systems_. 
  Angular modules must be declared with the @NgModule decorator; in JavaScript,
  every file is considered a module and objects are made _publicly available_
  when marked with the `export` keyword. If another JavaScript module wishes
  to call upon a publicly available object, it uses an _import statement_.

### Angular Libraries

Angular 2 is a collection of JavaScript modules, referred to as 'libraries'. Each
  Angular library begins with `@angular`, followed by the name of the library.
  Angular libraries are installed via a package manager, and we bring them into
  our applications via _import statements_.


**__!!IMPORTANT!!__**  
**Do not confuse import statements with the _import_ property of the @NgModule
  decorator**; imports done at the NgModule level provide access to everything that
  the NgModule is in charge of, where as standard JavaScript import statements done
  at the file level provide _that specific file_ with the items imported.

----------

## Components

Angular applications manage their views through _components_. A component
  is an Angular 2 decorator (`@Component({})`) which contains metadata that
  informs the Angular compiler how to generate and interact with a given template.
  Any application logic pertaining to the component is defined within a _class_.
  The class contains an API of properties and methods that can interact or manipulate
  the view.

Simplified: You make views with Components. The @Component decorator contains
  the name, the template, styling, and other information relating to how the 
  view should be displayed. The _component class_ which the decorator relates to
  contains the application logic which provides a level of interactivity with the
  view.

## Templates

A component's view is defined within a _template_. Templates are primarly made up
  of HTML with some Angular-ized template syntax, which the Angular compiler utilizes to 
  render components. 

## Metadata

Metadata is what makes Angular, Angular. The Angular compiler looks to metadata
  to understand how and when to process data. The _component_, discussed earlier,
  is nothing more than a class until _metadata_ is added to communicate with
  the Angular compiler. To make a class into a component, we add the `@Component`
  decorator directly before the component class and provide it with a _configuration
  object_ that contains the information necessary for Angular to generate the component
  and its view.

_Components, Templates, and Metadata together describe a view._

Common decorators include: `@Injectable`, `@Input`, and `@Output`

**Main Point**: In order for Angular to know what to do with your code, you must
  add metadata.

----------

## Data Binding

**Data Binding** is a mechanism supported by Angular which coordinates the template
  with parts of a component. Adding binding markup to a template tells Angular how
  to connect that template with it's component.

Angular 2 supports _four forms of data binding syntax_:

- `{{value}}` : One-way binding from the component to the template; _interpolation_
- `[property] = "value"` : One-way binding from the component to the template; _property binding_
- `(event) = "handler"` : One-way binding from the template to the component; _event binding_
- `[(ngModel)] = "property"` : Two-way data binding; any change on one side
  will reflect on the other; _two-way property/event binding_

Angular processes _all_ data binding once per JavaScript event cycle, starting from the
  root component and working its way through the component tree through all child components.

Data binding can be used to communicate between templates and components, as well as between
  parent and child components.

## Directives

The Angular compiler renders templates _dynamically_; it transforms the DOM based on
  instructions provided by **directives**. A directive is _a class with directive 
  metadata_ (`@Directive` decorator with a _configuration object_). Components are
  actually directives that have been _extended_ with template-oriented features; we
  differentiate between them, however, because while components are technically directives,
  they are very distinct in their overall usage.

There are two additional types of directives which genrally appear within _element tags_,
  similar to attributes:

1. Structural directives: Directives which modify the DOM by _adding/removing/replacing_
  elements.
2. Attribute directive: Directives which change the behavior of an _existing_ element.

We can also define our own directives; the `HeroListComponent` in the Tour of Heroes
tutorial is considered a custom directive.

----------

## Services

A service provides a _specific_ feature, value, or function to an application. Services
  handle data queries, error logging, app configuration...almost anything. There is no
  **official** definition of a service, no service base class, no place to register services,
  no service decorator. Regardless of this, they are a core aspect of any Angular application.

Services can be utilized for _separation of concern_. A component should only be in charge of a view;
  it should not be responsible for server communication, validations, logging, or anything else. Their
  only job is to enable a user experience. Anything outside of view manipulation should be handled by a **service**. A component should only be concerned with properties and methods for data binding.

**THIS IS NOT MANDATORY**, but may the Angular Gods be on your side if you choose to throw everything
  into one big ugly file. Separate your logic. Make it easy to maintain and understand. Make it easy to test.
  Save yourself the headache.

Services are made available to components through _dependency injection_.

## Dependency Injection

If a class or component needs to utilize a service during it's lifecycle, the service must be
  _injected_ into the class in it's **constructor method** as a parameter. The Angular compiler
  then looks at the _parameter types_ and asks an **injector** for the
  services the component requires. An **injector** _maintains a container of service_
  **singletons** that it has previously created. If a component requests a service that
  has yet to be instantiated, the injector _creates and adds the service to it's container_
  before returning the service to Angular. Once the injector resolves and returns the
  services called for in a component's constructor method, Angular **then** calls upon
  component's constructor method and provides the necessary services as arguments.

**WELCOME TO DEPENDENCY INJECTION, LADIES & GERMS**

In order to be available for dependency injection, services must be registered as **providers**.
  This can be done at the module _or_ component level, though best practice is to register them
  at the module level. If a service is registered as a provider at the component level, every
  time that component is instantiated, a new instance of that service is generated. This can
  be useful for private caching, individual data edits, etc.

**Key Takeaways**:
- Dependency injection is used everywhere in the Angular framework
- The _injector_ is the main mechanism
  - An injector _maintains a container of service instances_ that it created.
  - An injector _can create a new service instance **from a provider**_.
- A _provider_ is a recipe for a service.
- Register _providers with injectors_.