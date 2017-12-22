# Vue.js 2.0

## 04. Event Listeners [[src](04-vue-event-listeners.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/4)]

```html

<button  v-on:click="addName">Add Name</button>

<!-- short hand for v-on: you can use the @ symbol-->
<button @click="addName">Add Name</button>

```


```javascript

new Vue({

    methods : {
       
      addName : function(){
          
      }
    }

})

```