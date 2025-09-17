# Angular 
[Angular](https://angular.dev/installation)

## Angular CLI Installation

```bash
# Installing New
npm install -g @angular/cli

# Updating Existing:
# Uninstall the current version of Angular CLI (if installed):
npm uninstall -g @angular/cli

# Clear the npm cache (optional but recommended):
npm cache clean --force

# Install the latest version of Angular CLI globally:
npm install -g @angular/cli@latest

# Verify the installation:
ng version
```

## Create new project
```bash
ng new my-app 
npm start
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
## keep Types in angular templates
```javascript
// in template .html
 <ng-template [ngIf]="identity(node)" let-node="ngIf">
  <div>{{node.propertyIsRecognized}}
 </ng-template>

// in .ts
  identity(value: any): MyType {
    return value as MyType;
  }

```



