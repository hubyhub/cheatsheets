# Angular 7


## CLI - Installation

```bash
#https://angular.io/guide/quickstart
npm install -g @angular/cli

# Angular requires Node.js version 8.x or 10.x.
```

## Create new project
```bash
 
 ng new my-app

```
## Start built-in dev Server

```bash

cd my-app
ng serve --open 
# in npm script in Intellij: "start"

```


## Add new component or service
```

ng generate component heroes
ng generate service hero

# .css
# .html
# .spec.ts
# .ts
```


# Decorators
```
@Input() hero:Hero

# Services
@Injectable({
  providedIn: 'root',
})

```

# Services
# Providers
A provider is something that can create or deliver a service; 
in this case, it instantiates the HeroService class to provide the service.

# Observables
Angular's HttpClient methods return RxJS Observables.


# Dependency Injection


# Typescript






