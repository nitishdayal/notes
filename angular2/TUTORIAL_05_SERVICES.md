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
import { Injectable } from '@angular/core';

@Injectable()
export class HeroService { }
```

# !! **IMPORTANT** !!
**DO NOT INSTANTIATE NEW INSTANCES OF A SERVICE!** This will lead to _many hours
  of regret and disappointment_. If a component is told to create a new instance
  of a service, it will do so **EVERY TIME** the component is reloaded. The component
  then has to be aware of how to make that service (no longer a separate concern).
  Not to mention weird caching issues (two services with two different caches = wat?) and
  all sorts of not-fun things no one wants to deal with. So. Don't do it. There's no point.
  It doesn't provide any additional benefit to the development process.

