# Gulp
1. [Installation](#installation)
2. [Gulpfile.js](#gulpfile)

Gulp is a BUILD SYSTEM. A build system is simply a collection of tasks (comonly called "task runners") that automate repetitive work. 

<a name="installation"></a>
##  1. Installation 
__There are 2 ways to work with Gulp:__

1. Install gulp **globally** and use gulp directly from command line. [Official Way](https://github.com/gulpjs/gulp/blob/master/docs/getting-started.md#getting-started)
2. Install gulp only **locally** without CLI and run gulp using the **npm run** command. (Prefered way, because deployment is easier.)

Install gulp **locally**:

```sh
$ npm install --save-dev gulp
```
and then add the command in the "scripts" in package.json

```json
"devDependencies": {
    "gulp": "3.5.2"
}
"scripts": {
    "build": "gulp mytask"
}
```

Now you can run in your cli: 
```sh 
npm run build
```  


<a name="gulpfile"></a>
## 2. Gulpfile.js
Create a gulpfile.js at the root of your project:

```javascript
var gulp = require('gulp');

gulp.task('mytask', function() {
  console.log("hey it works! task is running!");
});
```

run it by typing following in the command-line
```sh 
$ gulp mytask
```  

#### Default task
Calling a task by name is great, but it doesn't scale. That's where the default task comes in.
The default task calls our other tasks.
```javascript
gulp.task('default', ['mytask', 'myOtherTask']);
```
```sh 
$ gulp  #runs the 'default' task, which runs "mytask" and "myOtherTask"
``` 

## 3. Other Plugins
Search the web for plugins and install them via npm with the --save-dev parameter.<br>
[Gulp Plugins Page](http://gulpjs.com/plugins/)


```sh 
$ npm install gulp-rename --save-dev 
``` 

Then define the plugin as a dependency in your gulpfile.js:

```javascript
var gulp = require('gulp'),
  rename = require('gulp-rename');
```

## 4. Choose files to work on by using GLOBs


```javascript
gulp.src(['js/**/*.js', '!js/**/*.min.js'])
```

Glob patterns specify sets of filenames with wildcard characters.
| Pattern         | Matches       |
| --------------- |-------------- |
| js/app.js       | the exact file
| css/*.css       | all files ending in .css in the css directory only |
| css/**/*.css    | all files ending in .css in the css directory and all child directories      |
| !css/style.css  | excludes style.css filenames from the match     |
| *.+(js|css)     | Matches all files in the root directory ending in .js or .css      |


[https://github.com/isaacs/node-glob](https://github.com/isaacs/node-glob)<br>
[https://github.com/isaacs/minimatch](https://github.com/isaacs/minimatch)<br>
[https://www.smashingmagazine.com/2014/06/building-with-gulp/](https://www.smashingmagazine.com/2014/06/building-with-gulp/)<br>


## 5.Recipes

1. Copy/Paste all js files and folder-structure

    ```javascript
    gulp.task('scripts', function(){
            gulp.src('src/assets/scripts/**/*.js')
                .pipe(gulp.dest('releases/test/scripts'))
    });
    ```

2. Copy/Paste and uglify(= minify)

    ```javascript
    var gulp = require('gulp'),
        uglify = require('gulp-uglify');        
    
    gulp.task('scripts', function(){
            gulp.src('src/assets/scripts/**/*.js')
                .pipe(uglify())
                .pipe(gulp.dest('releases/test/scripts'))
    });
    ```

3. Copy/Paste, uglify and rename filenames to .min.js 

    ```javascript
    var gulp = require('gulp'),
        uglify = require('gulp-uglify'),
        rename = require('gulp-rename');
    
    gulp.task('scripts', function(){
        gulp.src('src/assets/scripts/**/*.js')
            .pipe(rename({suffix:'.min'}))
            .pipe(uglify())
            .pipe(gulp.dest('releases/test/scripts'))
    });
    ```

4. Takes files, uglifies them, concatenates them to 1 files and saves it as app.js 

    ```javascript
    var gulp = require('gulp'),
        uglify = require('gulp-uglify'),
        rename = require('gulp-rename'),
        concat = require('gulp-concat');    
    
    gulp.task('scripts', function(){
        gulp.src('src/assets/scripts/**/*.js')
            .pipe(uglify())
            .pipe(concat('app.js'))
            .pipe(gulp.dest('releases/test/scripts/'))
    });
    ```

[Checkout recipes!](https://github.com/gulpjs/gulp/tree/master/docs/recipes)


## 6. Watch Task
You don't want to run **"gulp"** manually in the cli every time you change something in your code. 
Watch-task watches the file for changes and automatically starts a task.

```javascript
    var gulp = require('gulp'),
        uglify = require('gulp-uglify'),
        rename = require('gulp-rename'),
        concat = require('gulp-concat');    
    
    gulp.task('scripts', function(){
        gulp.src('src/assets/scripts/**/*.js')
            .pipe(uglify())
            .pipe(concat('app.js'))
            .pipe(gulp.dest('releases/test/scripts/'))
    });
    
    // now on every change in one of those files it calls the "scripts" task 
    gulp.task('watch', function(){
        gulp.watch('src/assets/scripts/**/*.js', ['scripts']);
    });
```

## 7. Build Task

1. Clean Folder<br> 
Delete the build directory
    
2. Copy App Directory<br>
Clone the app directory and pipe the contents into a new build directory

3. Remove Unwanted Files/Folders<br>
Delte any files and folders you don't want to deploy in your final build.
    
**Attention:** gulp would run them asyncronously, but we need to run them sequentially. To do that you need to give the functios a callback

TODO:<br>
gulp-order<br>
gulp-html-replacer<br>


```javascript
    var gulp = require('gulp'),
        uglify = require('gulp-uglify'),
        rename = require('gulp-rename'),
        concat = require('gulp-concat');    
    
    gulp.task('scripts', function(){
        gulp.src('src/assets/scripts/**/*.js')
            .pipe(uglify())
            .pipe(concat('app.js'))
            .pipe(gulp.dest('releases/test/scripts/'))
    });
    
    // now on every change in one of those files it calls the "scripts" task 
    gulp.task('watch', function(){
        gulp.watch('src/assets/scripts/**/*.js', ['scripts']);
    });
```
