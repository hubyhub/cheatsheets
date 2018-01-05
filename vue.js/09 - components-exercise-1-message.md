# Vue.js 2.0

## 09. Components Exercise 1 - Message [[src](09-components-exercise-1.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/9)]


* vue-ify bulma-components: [Bulma css framework](https://cdnjs.cloudflare.com/ajax/libs/bulma/0.6.1/css/bulma.min.css)



```javascript

Vue.component('message', {

        // EXPLICITLY DEFINE PROPS
        props : ['title'],

        template: `
           <div>{{title}}
           </div>
        `,

        created: function() {

        },

        methods: {

        },

        data: function() {
            return {
                isVisible : true,
            }
        }

    });

```



