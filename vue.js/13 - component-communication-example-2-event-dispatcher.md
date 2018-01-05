# Vue.js 2.0

## 13. Component Component Communication - Event Dispatcher [[src](13-component-communication-event-dispatcher.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/13)]

## If you want to make siblings communicate with each other.

Another way for communication between Vue components, is creating your own event dispatcher.<br>
Due to the fact that every Vue instance already implements the necessary interface, this is pretty easy!

### 1. Create a new Vue instance before the components.
```javascript

    window.Event = new Vue();

```


### 2. Fire the Event to the Vue instance
```javascript
    //
    methods: {
        onCouponApplied: function() {
            console.log("now emmit")
            Event.$emit('applied');
        }
    }

```

### 3. Listen to the event from any component
```javascript

   created: function() {
       var that = this;

       Event.$on('applied', function(){
           that.couponApplied = true;
       })
   }

```


