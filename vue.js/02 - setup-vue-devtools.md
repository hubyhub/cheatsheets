# Vue.js 2.0

## 02. Setup Vue DevTools [[video]](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/2)


1. [Vue.js devtools- Chrome extension](https://chrome.google.com/webstore/search/vue.js%20devtools?hl=de)
2. In Chrome extension-settings: enable *"Allow access to file URLs"* for Vue.js devtools
3. Use *vue.js* for running Vue in development mode and **not** *vue.min.js* 

Now you can inspect the underlying data in Vue in an easy way.<br>
The **<Root>** Element of Vue is saved to a temporary variable: **$vm0** 

Vue will proxy all data, so you can access them directly in the console:

```javascript
$vm0.message    // hello world
$vm0.message = 'you can change it in console';

```
