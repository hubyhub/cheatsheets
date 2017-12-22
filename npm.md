

### npm version
```
npm -v
```

### npm initialiasing
```
npm init
```

### Updating npm
```
###DONT DO IT... Destroyed npm
npm install npm@latest -g
```

### install all packages from existing package.json
```
npm install
```

### Installing new packages
```
npm install <package_name> 	
npm install lodash

npm install <package_name> --save-dev	
npm install lodash --save-dev

```

### Installing packages globally
```
npm install -g <package_name> 	

npm install -g jshint
```


### Updating local packages
```
npm update
```


### Updating global packages
```
npm update -g jshint
```

### Uninstalling local packages
```
npm uninstall lodash
npm uninstall --save lodash		    # To remove it from the dependencies in package.json 
npm uninstall --save-dev lodash		# To remove it from the devDependency in package.json 
```


### npm test
```
npm test [-- <args>]				# This runs a package's "test" script, if one was provided.
```


### upgrading NPM intself
```
# npm install -g npm-windows-upgrade
# npm-windows-upgrade
# Note: Do not run npm i -g npm. Instead use npm-windows-upgrade to update npm going forward.
```


bower
gulp
lessc
npm 
npx
webpack
npm install webpack-dev-server
