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

