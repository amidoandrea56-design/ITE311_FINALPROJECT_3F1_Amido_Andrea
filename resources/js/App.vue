<script setup>
import { ref, computed, watch } from 'vue'
import GroceryItem from './GroceryItem.vue'

const itemName = ref('')
const itemPrice = ref('')
const errorMessage = ref('')
const isFormVisible = ref(true)
const groceryList = ref([])

function addItem() {
  if (itemName.value === '' || itemPrice.value === '') {
    errorMessage.value = 'Please fill in all fields.'
    return
  }

  errorMessage.value = ''

  groceryList.value.push({
    name: itemName.value,
    price: Number(itemPrice.value)
  })

  itemName.value = ''
  itemPrice.value = ''
}

function removeItem(index) {
  groceryList.value.splice(index, 1)
}

const totalPrice = computed(() => {
  let total = 0
  for (const item of groceryList.value) {
    total = total + item.price
  }
  return total.toFixed(2)
})

watch(
  () => groceryList.value.length,
  (newCount) => {
    console.log('Total items:', newCount)
  }
)
</script>

<template>
  <div class="container">
    <h1>Grocery List</h1>

    <button @click="isFormVisible = !isFormVisible">
      {{ isFormVisible ? 'Hide' : 'Show' }} Form
    </button>

    <div v-show="isFormVisible" class="form-box">
      <input v-model="itemName" placeholder="Item name" />
      <input v-model="itemPrice" type="number" placeholder="Price" />
      <button @click="addItem">Add</button>

      <p v-if="errorMessage" class="error">{{ errorMessage }}</p>
    </div>

    <p v-if="groceryList.length === 0">No items yet.</p>
    <ul v-else>
      <GroceryItem
        v-for="(item, index) in groceryList"
        :key="index"
        :item="item"
        @remove="removeItem(index)"
      />
    </ul>

    <p v-if="groceryList.length > 0">
      Items: {{ groceryList.length }}
    </p>

    <h3 class="total" :style="{ color: totalPrice > 500 ? 'red' : 'green' }">
      Total: ₱{{ totalPrice }}
    </h3>
  </div>
</template>

<style>
body {
  background: #f0f4f8;
}

.container {
  max-width: 450px;
  margin: 40px auto;
  padding: 24px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  font-family: sans-serif;
}

h1 {
  text-align: center;
  color: #2d6a4f;
  margin-bottom: 16px;
}

.form-box {
  margin-top: 12px;
}

input {
  padding: 8px;
  margin: 4px 4px 4px 0;
  border: 1px solid #ccc;
  border-radius: 6px;
}

button {
  padding: 8px 12px;
  background: #2d6a4f;
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

button:hover {
  background: #1b4332;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 8px 0;
  border-bottom: 1px solid #eee;
}

.error {
  color: red;
}

.total {
  text-align: center;
  margin-top: 16px;
}
</style>