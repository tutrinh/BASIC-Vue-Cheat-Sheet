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
