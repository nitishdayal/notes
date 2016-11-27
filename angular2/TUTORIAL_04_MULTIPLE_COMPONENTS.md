# Angular 2: Tour of Heroes

## Multiple Components

----------
### Separation of Concerns + Reusability + Modularity

Angular 2 applications are made out of multiple _modular_ components. Each component
   should be responsible for it's own interactions and data; this simplifies development,
   testing, and maintenance while also providing us with the ability to _reuse_ portions
   of the application without having to repeat code **(DRY principle)** or worry about
   breaking something. If a component is only responsible for it's own functionality, then
   we should be able to treat it as it's own _module_ and call it whenever we need to
   without having to call for the entire application.

Angular 2 components should follow the 'component-name.component.ts' naming convention.
  File names should be hyphenated, while functions/classes/variables should be camelCased.
  All components require the `@Component` decorator to store the corresponding metadata that
  the Angular compiler relies on to inject functionality into the Component class.

----------
### Passing Data Between Components

Components can communicate with one another for whatever purpose they may need to. A
  common use for communication between components is to send information between them.
  Earlier we looked at one-way data binding; data being sent from the component to the
  UI. This was done using double curly brackets `{{}}`. We then looked at two-way data
  binding, utilizing the `ngModel` built-in directive and the banana-style brackets `[()]`.
  What if we want to send data from one component to the other?

In order to accomplish this, we need to do two things: declare a variable as an _input_
  on the component that will be _receiving the data_, and specify that the _input variable_
  will be the **target** of property binding from a parent component.

**Example** (Relevant lines are preceded by multi-line comments):

```TypeScript
// hero-detail.component.ts
import { Component, Input } from '@angular/core'; // Import Input directive

import { Hero } from './hero';


@Component({
  selector: 'my-hero-detail',
  template: `
    <div *ngIf="hero">
      <h2>{{hero.name}} details!</h2>
      <div><label>id: </label>{{hero.id}}</div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="hero.name" placeholder="name"/>
      </div>
    </div>
  `
})
export class HeroDetailComponent {
  /**
   * The 'hero' property of the HeroDetailComponent is being declared
   * as an 'Input', meaning that it will be defined when the component
   * received an input from somewhere in regards to what the hero
   * property should be.
   **/
  @Input()
  hero: Hero;
}



// app.component.ts
import { Component } from '@angular/core';

import { Hero } from './hero';

@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <h2>My Heroes</h2>
    <ul class="heroes">
      <li *ngFor="let hero of heroes"
        [class.selected]="hero === selectedHero"
        (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
      </li>
    </ul>
    <!--
      We have placed the 'hero' property of the MyHeroDetail component
      within square brackets to specify that it is the target, and it is 
      defined as the 'selectedHero' property of the AppComponent. 
    -->
    <my-hero-detail [hero]="selectedHero"></my-hero-detail>
`
})
export class AppComponent {
  title = 'Tour of Heroes';
  heroes = HEROES;
  selectedHero: Hero;

  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
}

// app.module.ts

import { NgModule }      from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule }   from '@angular/forms';   // Import forms module for the @Input property

import { AppComponent }  from './app.component';
import { HeroDetailComponent } from './hero-detail.component';

@NgModule({
  imports: [
    BrowserModule,
    FormsModule    // Specify that we are importing the FormsModule from outside of the application
  ],
  declarations: [
    AppComponent,
    HeroDetailComponent
  ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }

```