# Vue.js 2.0

## 12. Component Component Communication - Custom Events [[src](12-component-communication-custom-events.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/12)]

### Child Parent Communication via `this.$emit('applied')`
How can one component notify another about a particular action or event that just took place?

**"v-on"**<br>directive is great for child-parent communication
This is a really good way to communicate between a child and a parent.<br>
The parent simply asks the child: "let me know when you have been *applied*" and I will trigger some kind of method (onApplied)<br>
(In our example the root instance Vue is the parent)

**v-on** and **this.$emit** is great for child-parent communication<br>
```html
<coupon v-on:applied="onApplied"></coupon>
```

```javascript

methods : {
 onApplied : function(){
    this.$emit('applied');
 }
}

```

When the coupon makes an announcement, that it has been applied, the parent can pick up the announcement and make same action.

```javsacript
// just a reminder @ is a shortform for v-on
<coupon @applied="onApplied"></coupon>
```
