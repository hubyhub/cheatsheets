# Vue.js 2.0

## 05. Attribute Binding & Class Binding [[src](05-vue-attribute-binding.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/5)]

Attribute Binding work via v-bind or shortform with ":"

```html
<button  v-bind:title="myTitle">Hover Over Me</button>

<!-- short hand for v-on: you can use the @ symbol-->
<button :title="myTitle""">Hover Over Me</button>

<!-- class-binding by condition-->
<span :class="{ 'is-loading': isLoading }">is Loading...</span>

```


```javascript

new Vue({

    data : {
        isLoading : false,
        myTitle : "Attribute Binding seems to work as well"
    }

})

```