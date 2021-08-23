# BASIC Vue Cheat Sheet

## Lifecycle Hooks
```text
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated
beforeDestroy
destroyed
```

## Expressions
```html
<div id="app">
  <p> I have a {{ product }} </p>
  <p>{{ isWorking ? 'YES' : 'NO' }}</p>
  <p>{{ product.getSalePrice() }}</p>
</div>
```

## Binding
```html
<a v-bind:href="url">...</a>
Shorthand
<a :href="url">...</a>
```
Adding/removing attributes based on true/false
```html
<button :disabled="isButtomDisabled">...</button>
```
CSS Class
```html
<div :class="{ active: isActive}">...</div>
If isActive then add class 'active'
```
Style
```html
Style color set to value of activeColor
<div :style="{ color: activeColor}"
```

## Actions / Events
```html
<button v-on:click="addToCart">...</button>
Shorthand
<button @click="addToCart">...</button>
```
With arguments
```html
<button @click="addToCart(product)">...</button>
```
Prevent default behavior
```html
<form @submit.prevent="addProduct">...</form>
.stop // stop all event propagation
.self // only trigger if event.target is element itself
```
    

## Directives
Conditional
```html
<p v-if="inStock">{{ product }}</p>
<p v-else-if="onSale">...</p>
<p v-else>...</p>
```
Show
```html
<p v-show="showProductDetails">...</p>
```
Two-way Data Binding
Only on input fields
```html
<input v-model="firstName">

v-model.lazy="..." // Sync inpus after change event
v-model.number="..." // Always returns a number
v-model.trim="..." // Strips whitespace
```
## List Rendering
```html
<li v-for="item in items" :key="item.id">
  {{ item }}
</li>
```
Using index to get position in an array
```html
<li v-for="(item, index) in items">...</li>
```
Iterate through objects
```html
<li v-for="(value, key) in object">...</li>
```
v-for inside a component
```html
<cart-product v-for="item in products" :product="item" :key="item.id">...</cart-product>
```

## Component
```js
Vue.component('component-name', {
  components: {
    // components that can be used in the template
    ProductComponent, ReviewComponent
  },
  props: {
    // parameters the component accepts
    message: String,
    product: Object,
    email: {
      type: String,
      required: true,
      default: "none",
      validator: function(value) {
        // should return true if value is valid
      }
    }
  },
  data: function() {
    return {
      firstName: 'Henry',
      lastName: 'Mastery'
    }
  },
  computed: {
    // return cached values until dependencies change
    fullName: function() {
      return this.firstName + ' ' + this.lastName
    }
  },
  watch: {
    // called when firstName changes value
    firstName: function(value, oldValue) {
    
    }
  },
  methods: {...},
  template: '<span>{{ message }} </span>'
})
```

Single File Component
- Create a .vue file, src/app/components/my-component.vue
```html
<template>
  <span>{{ firstName }}</span> 
</template>
<script>
  // do your imports here
  import AnotherComponent from "@/components/another-component.vue";
  export default {
    components: {
      // list of components used in this component  
      AnotherComponent
    },
    props: {
    
    },
    data() {
      return {
        firstName: 'Henry'
      }
    },
    computed: {},
    watch: {},
    mounted() {...},
    methods: {...},
    
  }
</script>
<style>
  ...
</style>
```
- Register the coponment in index.js
```js
import MyComponent from '@/components/my-component.vue';
Vue.component('my-component', MyComponent);

new Vue({
  el: #app
})
```
## Custom Events
Use props to pass data into child components,
custom events to pass data to parent elements.

Set listener on component, within its parent
```html
<button-counter v-on:incrementBy="incWithVal">
  
```
Inside parent component
```js
methods: {
  incWithVal: function(toAdd) {...}
}
```
Inside button-counter template
```js
this.$emit('incrementBy', 5)
'incrementBy' - custom event name
5 - data sent up to parent
```
#### Another Example
Child component passing data back to parent with custom events.
```html
<template>
  <button @click="changeMsg">Click to change message</button>
</template>
<script>
  export default {
    methods: {
      changeMsg() {
        this.$emit('customEventName', data want to pass to the parent);
        this.$emit('changeMsg', 'Hello');
      }
    }
  }
</script>

```
Parent using child component and receiving data from child.
Listening to custom event from child.
```html
<template>
  <HelloWorld @customEventName="methodToCall" />
   <HelloWorld @changeMsg="setMessage" />
</template>
<script>
  export default {
    data() {
      return {
        welcomeMsg: "Hello World"
      };
    },
    methods: {
      setMessage(msg) {
        this.welcomeMsg = msg;
      }
    }
  }
</script>
 ```
## Event Bus
The event bus / publish-subscribe pattern is a way for getting components to talk to each other without parent child relationship. Any component, anywhere.
Need to create the event bus and export it so other modules and components can use it. 
- Create a new js file
- Import Vue library
- Export an instance of it

```js
// src/event-bus.js
import Vue from 'vue';
export const EventBus = new Vue();
```
What you are essentially getting is a component that is entirely decoupled from the DOM or the rest of the app. All that exists on it are its instance methodds.

You will need to import it into your components and call the same methods that you would use if you were passing messages between parent and child components.

#### Sending Events
`EventBus.$emit(channel: string, payload1: any, ...)`
```js
// src/components/Button.vue
<template>
  <button @click='emitGlobalClickEvent()'>{{ clickCount }}</button
</template>
<script>
  import { EventBus } from '@/event-bus';
  export default {
    data() {
    return {
      clickCount: 0
    }
  },
  methods: {
    emitGlobalClickEvent() {
      this.clickCount++;
      EventBus.$emit('clicked', this.clickCount);
    }
  }
  }
</script>
```



## Slot/Slots
### Default/Single Slot
Component Template
```html
/* my-component.vue */
<div>
  <h2> Heading </h2>
  <slot>
    Only show if no slot in content
  </slot>
</div>
```
Using the component with slot
```html
<my-component>
  <p>This will go inside the slot</p>
</my-component>
```

### Multiple Slots
Component Template
```html
/* my-component.vue */
<div>
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot>Default content</slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```
Usage
```html
<app-layout>
  <h1 slot="header">Heading</h1>
  <p>Main content</p>
  <p slot="footer">Contact info</p>
</app-layout>

```


