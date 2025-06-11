<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App')
const reversedItems = computed(()=>[...items.value].reverse())
const editing = ref(false)
const items = ref([
  { label: '10 party hats', purchased: true, highPriority: false, id: 1 },
  { label: '2 board games', purchased: true, highPriority: false, id: 2 },
  { label: '20 cups', purchased: false, highPriority: true, id: 3 }
])
const newItem = ref('')
const newItemHighPriority = ref(false)
const iceCreamFlavors = ref([])
const addItem = () => {
  items.value.push({ label: newItem.value, purchased: false, id: items.value.length + 1, highPriority: newItemHighPriority.value})
  newItem.value = ''
  newItemHighPriority.value = false;
}
const doEdit = (e) => {
  editing.value = e
  newItem.value = ''
}
const togglePurchased = (item) => {
  item.purchased = ! item.purchased
}
</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">
      Cancel
    </button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">
      Add Item
    </button>
  </div>
  <form class="add-item-form" @submit.prevent="addItem" v-if="editing">
    <input v-model.trim="newItem" type="text" placeholder="Add an item">
    <label>
      <input v-model="newItemHighPriority" type="checkbox">
      High Priority
    </label>
    <button class="btn btn-primary" :disabled="!newItem.length">
      Add Item
    </button>
  </form>
  <ul>
    <li @click="togglePurchased(item)" v-for="(item, index) in reversedItems" key="item.id" :class="{strikeout: item.purchased, priority: item.highPriority}">
      {{ item.label }}
    </li>
  </ul>
  <p v-if="!items.length">Nothing to see here</p>
</template>