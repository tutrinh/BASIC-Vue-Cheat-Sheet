# BASIC Vue Cheat Sheet

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

