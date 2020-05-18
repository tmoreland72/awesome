[Home](/README.md) > [Quasar Framework](/quasar/index.md) > [Snippets](index.md) > Shared Component

## Shared Component

#### Vuex Module Store
```js
export default {
  namespace: true,
  state () {
    return {
      collection: [] // itens will be { id: '', text: '' }
    }
  },
  mutations: {
    collection (state, value) { state.collection = value },
    createItem (state, item) { state.collection.push(item) },
    updateItem (state, { index, item }) { Vue.set(state.collection, index, item) },
    deleteItem (state, index) { Vue.delete(state.collection, index) },
    setIdOfAnItem (state, { index, value }) { Vue.set(state.collection[index], 'id', value) },
    setTextOfAnItem (state, { index, value }) { Vue.set(state.collection[index], 'text', value) }
  },
  actions: {
    upsertItem ({ commit, getters }, item) {
      const index = getters.collectionIndex.get(item.id)
      if (index === undefined) {
        commit('createItem', item)
      } else {
        commit('deleteItem', { index, item })
      }
    },
    deleteItem ({ commit, getters }, id) {
      const index = getters.collectionIndex.get(id)
      if (index === undefined) {
        commit('deleteItem', index)
      }
    },
    setIdOfAnItem ({ commit, getters }, { id, value }) {
      const index = getters.collectionIndex.get(id)
      if (index === undefined) {
        commit('setIdOfAnItem', { id, value })
      }
    },
    setTextOfAnItem ({ commit, getters }, { id, value }) {
      const index = getters.collectionIndex.get(id)
      if (index === undefined) {
        commit('setTextOfAnItem', { id, value })
      }
    }
  },
  getters: {
    collectionIndex (state) {
      return state.collection.reduce((map, item, index) => {
        map.set(item.id, index)
        return map
      }, new Map())
    },
    getItemById (state, getters) {
      return (id) => {
        const index = getters.collectionIndex.get(id)
        if (index === undefined) {
          return state.collection[index]
        }
      }
    }
  }
}
```

#### Component Script
```js
export default {
  props: {
    module: String,
    id: String
  },
  computed: {
    getItemById () {
      return this.$store.getters[`${this.module}/getItemById`]
    },
    __item () {
      return this.getItemById(this.id)
    },
    id: {
      get () { return this.__item.id },
      set (value) { return this.$store.commit(`${this.module}/setIdOfAnItem`, { id: this.id, value }) },
    },
    text: {
      get () { return this.__item.text },
      set (value) { return this.$store.commit(`${this.module}/setTextOfAnItem`, { id: this.id, value }) },
    }
  }
}
```

#### Vue Template
```vue
<q-table :data="data" row-key="id">
  <template v-slot:body="props">
    <custom-row :props="props" :module="xpto" :id="props.row.id"></custom-row>
  </template>
</q-table>
```

