# Vue.js 2.0

## 06. The Need for Computed Properties [[src](06-vue-computed-properties.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/6)]

Most of the time you gonna have strings, objects or arrays. <br>
but for other times you might need a bit of computation before you render it on the page.<br>
Thats exactly what **computed properties** are for.<br>

Computed Values are **CACHED** (calculated once), but are still **REACTIVE**

**Used for:**<br>
Filtering data, further process data<br>
  
**Instead of:**

```html

<div>
    {{ message.split('').reverse().join('') }}
</div>

```

**Use computed values:**

```html

<div>
    {{ reversedMessage }}
</div>

```   


```javascript

new Vue({

     el: '#root',
     
     data : {
         message : 'hello world'
     },
     
     computed : {
         reversedMessage : function(){
             return message.split('').reverse().join('');
         }
     }

})

``` 