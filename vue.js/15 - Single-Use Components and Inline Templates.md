# Vue.js 2.0

## 15. Single-Use Components and Inline Templates [[src](15-inline-templates.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/15)]

* inline-components need to be defined with: `inline-template`
* eventually for Server-side Rendering

Not every component needs to be generic and reusable. <br>
Sometimes, a single, view-specific component is needed. <br>
The inline-template attribute that nests your template directly in your HTML file can be used in such cases<br>

If you have for example: 1 area of 1 page that you want to make dynamic,<br>
but there is not need for a generic, reusable component.<br>



Tipp: naming-convention of view-dependend component: "*name*-view"

```html
<progress-view inline-template>
        <h1>Hello CompletionRate: {{ completionRate}}</h1>
        <button @click="completionRate+=0.1">Increase</button>
    </progress-view>

```


```javascript

   Vue.component('progress-view',{

        data : function(){
            return{
                completionRate : 0.5
            }
        }
    });

```

