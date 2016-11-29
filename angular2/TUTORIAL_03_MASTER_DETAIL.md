# Angular 2: Tour of Heroes

## Master/Detail

----------
### Display a Collection of Items

To display a collection or list of items, use the `*ngFor` _directive_.

```html
<h2>My Heroes</h2>
<ul class="heores">
  /** Assuming the component has a 'heroes' property,
    * the following line of code will generate an 'li'
    * HTML element for each iterable item within the 
    * 'heroes' property.
    **/
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>  
</ul>
```

----------
### Event-Handling in Angular 2

To register an event-handler with Angular 2, we **define the method** we want
  to call within the **component's class definition.** Then we attach the method
  to an HTML element on which we are expecting the event to occur. This is 
  done by providing the event we're listening for inside a set of parenthesis 
  `(click)` and set it equal to the name of the method.


**Example** (relevant lines are preceded by multi-line comments):

```typescript
import { Component } from '@angular/core';

export class Hero {                         // Create a class for the data model
  id: number;
  name: string;
}

const HEROES: Hero[] = [                    // Create mock data set
  { id: 11, name: 'Mr. Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];

@Component({
  selector: 'my-app',
  template: `
    <h1>{{title}}</h1>
    <h2>My Heroes</h2>
    <ul class="heroes">
      <!-- 
      The list item has three properties: *ngFor, [class.selected], and (click).
      
      The *ngFor property is an Angular 2 built-in directive which will iterate through
      a provided collection/list and generate as many HTML nodes as there are list items.

      The [class.selected] property is another Angular 2-provided tool. We can set any
      class property by using this syntax (ie; [class.color], [class.font-style]). In this
      example we are giving the list item a class of 'selectedHero' if the expression
      'hero === selectedHero' resolves to a true statement.

      The (click) property is an example of how Angular 2 handles event binding. 
      In this example, we want to listen for a click on one of the list items, and 
      then call the 'onSelect()' method, passing in the contents of the list item 
      as an argument (hero).
      -->
      <li *ngFor="let hero of heroes"
        [class.selected]="hero === selectedHero"
        (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
      </li>
    </ul>
    <!--
    The *ngIf property on this div specifies that the element will not be displayed
    unless the 'selectedHero' property of the corresponding component class is 
    valid.
    -->
    <div *ngIf="selectedHero">
      <h2>{{selectedHero.name}} details!</h2>
      <div><label>id: </label>{{selectedHero.id}}</div>
      <div>
        <label>name: </label>
        <input [(ngModel)]="selectedHero.name" placeholder="name"/>
      </div>
    </div>
  `,
  styles: [`
    /*
      CSS styles only affect the component for which they are defined; they do not
      'bleed' into other components.
    */
    .selected {
      background-color: #CFD8DC !important;
      color: white;
    }
    .heroes {
      margin: 0 0 2em 0;
      list-style-type: none;
      padding: 0;
      width: 15em;
    }
    .heroes li {
      cursor: pointer;
      position: relative;
      left: 0;
      background-color: #EEE;
      margin: .5em;
      padding: .3em 0;
      height: 1.6em;
      border-radius: 4px;
    }
    .heroes li.selected:hover {
      background-color: #BBD8DC !important;
      color: white;
    }
    .heroes li:hover {
      color: #607D8B;
      background-color: #DDD;
      left: .1em;
    }
    .heroes .text {
      position: relative;
      top: -3px;
    }
    .heroes .badge {
      display: inline-block;
      font-size: small;
      color: white;
      padding: 0.8em 0.7em 0 0.7em;
      background-color: #607D8B;
      line-height: 1em;
      position: relative;
      left: -1px;
      top: -4px;
      height: 1.8em;
      margin-right: .8em;
      border-radius: 4px 0 0 4px;
    }
  `]
})
export class AppComponent {
  
  title = 'Tour of Heroes';
  heroes = HEROES;

  selectedHero: Hero;

  // The method that will be called if a list item is selected
  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }

}
```