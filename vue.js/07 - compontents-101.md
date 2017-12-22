# Vue.js 2.0

## 07. Compontents 101 [[src](07-vue-components-101.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/7)]

### Hello Component

Best practice is to use a "dash" and maybe prefix the component name with a certain name
```html    
    <my-cool-component></my-cool-component>
```


In order to slot everything that the user enters between the tags into the component, use "slot"
```html
    <task>hallo</task>
```


Components can be created like this:    

```javascript

    // Define a global components like this: 
    Vue.component('my-cool-component',{
        template : '<li>Foobar</li>',
        data : function() {
                    return {
                        message : 'instance Variable?'
                    }
                }
    });
```

```javascript
    Vue.component('task',{
        template : '<li><slot></slot></li>'
    });
```

They need to be defined before VUE is instanciated.


**ATTENTION:** For regular Vue instances you can set data equals to an OBJECT.<br>
However, for Components, you can't do that because it is not linked to any single instance.<br>
Instead you can make data equal to a function that returns an object
