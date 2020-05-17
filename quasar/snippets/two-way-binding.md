[Home](/README.md) > [Quasar Framework](/quasar/index.md) > [Snippets](index.md) > Two Way Binding

## Two Way Binding

#### Vuex (store.js)
```js
const state = {
   formData: {
      field1: "",   
   }
}

const mutations = {
   setFormData(state, value) {
      state.formData = value
   }
}

export default {
   namespaced: true,
   state,
   mutations,
}
```

#### Parent Component (Form.vue)
```vue
<template>
   <q-form>
      <field-component
         v-model="formData.field1"
         @update="(val) => handleUpdate(val, 'field1')" 
      />
   </q-form>
</template>

<script>
   import { mapMutations, mapState } = "vuex"

   export default {
      computed: {
         ...mapState('store', ['formData'])   
      },

      methods: {
         ...mapMutations('store', ['setFormData']),

         handleUpdate(val, field) {
            let formData = {...this.formData}
            this.setFormData = {
               ...this.formData,
               [field]: value,            
            }            
         }      
      },
   
      components: {
         FieldComponent: require("components/Field.vue").default,
      }
   }
</script>
```

#### Child Component (Field.vue)
```vue
<template>
   <q-input 
      :value="value"
      @input="handleUpdate"
   />
</template>

<script>
   export default {
      props: ["value"],

      methods: {
         handleUpdate(val) {
            this.$emit("update", val)
         }      
      },
   }
</script>
```

<p class="codepen" data-height="265" data-theme-id="light" data-default-tab="result" data-user="rstoenescu" data-slug-hash="VgQbdx" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="Quasar Framework v1">
  <span>See the Pen <a href="https://codepen.io/rstoenescu/pen/VgQbdx">
  Quasar Framework v1</a> by Razvan Stoenescu (<a href="https://codepen.io/rstoenescu">@rstoenescu</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://static.codepen.io/assets/embed/ei.js"></script>