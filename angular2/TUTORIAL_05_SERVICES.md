# Angular 2: Tour of Heroes

## Services

Angular 2 services are intended to be the main point of contact between the 
  Angular 2 application and whatever database it might be communicating with. It is
  a singular source of data for all components. By having one instance of communication,
  we are taking an additional step to prevent overlapping writes/deletes to the data later.
  Data services are _asynchronous_; use Promises and Observables to ensure constant
  and valid communication.

To make Angular 2 services, we create a new file with the extension '.service.ts' 
  (ie; hero.service.ts). As stated earlier, Angular 2 services are _injected_ into
  components that need them. In order to do so, we need to import the `Injectable`
  function from the Angular 2 core library.

Example:
```typescript
// hero.service.ts
import { Injectable } from '@angular/core';

import { Hero } from './hero';
import { HEREOS } from './mock-heroes';


@Injectable()
export class HeroService {
  getHeroes(): Promise<Hero[]> {
    return Promise.resolve(HEROES);
  }
 }
```

# !! **IMPORTANT** !!
**DO NOT INSTANTIATE NEW INSTANCES OF A SERVICE!** This will lead to _many hours
  of regret and disappointment_. If a component is told to create a new instance
  of a service, it will do so **EVERY TIME** the component is reloaded. The component
  then has to be aware of how to make that service (no longer a separate concern).
  Not to mention weird caching issues (two services with two different caches = wat?) and
  all sorts of not-fun things no one wants to deal with. So. Don't do it. There's no point.
  It doesn't provide any additional benefit to the development process.

----------

If any part of our application utilizes data from a service, it is known as a _consumer_. A
  consumer is completely unaware of how the service gets data; the location of the data
  is irrelevant. This presents a massive benefit to the development process as the data 
  source can be altered at any point without having to alter the components that utilize
  the data _in any way_.

In order for a component to consume data from a service, that service needs to be _injected_
  into the component's **constructor** method. Every class has a constructor method, whether
  the user defines it or not. We can _override_ it and provide the service as an argument,
  so that whenever the component is constructed the service will be made available as well.

Example:
```TypeScript
// app.component.ts
import { Component, OnInit } from '@angular/core';

import { Hero } from './hero';
import { HeroService } from './hero.service';


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
    <my-hero-detail [hero]="selectedHero"></my-hero-detail>
  `
})
export class AppComponent implements OnInit {
  title = 'Tour of Heroes';
  heroes: Hero[];
  selectedHero: Hero;

  constructor(private heroService: HeroService) { }

  getHeroes(): void {
    this.heroService.getHeroes()
      .then(heroes => this.heroes = heroes);
  }

  ngOnInit(): void {
    this.getHeroes()
  }

  onSelect(hero: Hero): void {
    this.selectedHero = hero;
  }
}
```