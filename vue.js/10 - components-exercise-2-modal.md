# Vue.js 2.0

## 10. Components Exercise 2 - Modal [[src](10-components-exercise-2.html)] [[video](https://laracasts.com/series/learn-vue-2-step-by-step/episodes/10?autoplay=true)]

* [Bulma Modal](https://bulma.io/documentation/components/modal/)
* custom events

In the x-button within the component you cannot just say:
```
<button class="modal-close is-large" @click="showModal=false"></button>
```

because the component does not know about the variable **showModal**

We want to have a decaoupled way how we can close the modal-dialog.
We want a **custom-event**, like: "close":
Emit custom event: @click="$emit('close')"

```html
<modal v-if="showModal" @close="showModal = false" >hey modal</modal>

```

```javascript

Vue.component('message', {

         template: `
                    <div class="modal is-active">
                        <div class="modal-background"></div>
                        <div class="modal-content">
                            <div class="box"><slot></slot></div>
                        </div>
                        <button class="modal-close is-large" @click="$emit('close')"></button>
                    </div>
                `,

    });

```



