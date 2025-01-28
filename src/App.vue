<script setup lang="ts">
import { Plus } from 'lucide-vue-next'
import { computed, ref } from 'vue'
import HeadingLarge from './components/HeadingLarge.vue'
import ListItem from './components/ListItem.vue'
import SectionContainer from './components/SectionContainer.vue'

export interface Item {
  id: number
  label: string
  quantity: number
}

const items = ref([
  { id: 1, label: 'Cabbage', quantity: 1, purchased: false },
  { id: 2, label: 'Potato', quantity: 5, purchased: false },
])

const reversedItems = computed(() => {
  return [...items.value].reverse().sort((a, b) => {
    if ((a.purchased && b.purchased) || (!a.purchased && !b.purchased)) {
      return 0
    } else if (a.purchased && !b.purchased) {
      return 1
    } else {
      return -1
    }
  })
})

const itemColumns = [
  { key: 'name', header: 'Item Name' },
  { key: 'quantity', header: 'Quantity' },
]

const inputError = ref('')

const newItemName = ref('')
const newItemQuantity = ref(null)

const removeItem = (id: Item['id']) => {
  items.value = items.value.filter((currItem) => currItem.id != id)
}

const submitHandler = () => {
  if (!newItemName.value || !newItemQuantity.value) {
    inputError.value = 'Fill out all fields'
    return
  }

  items.value.push({
    id: items.value.length > 0 ? items.value[items.value.length - 1].id + 1 : 1,
    label: newItemName.value,
    quantity: newItemQuantity.value,
    purchased: false,
  })

  newItemName.value = ''
  newItemQuantity.value = null
  inputError.value = ''
}

const charCount = computed(() => {
  return newItemName.value.length
})
</script>

<template>
  <SectionContainer>
    <HeadingLarge />
    <div class="p-4 border max-w-screen-md rounded flex flex-col gap-8">
      <form @submit.prevent="submitHandler" class="flex gap-4">
        <div class="flex flex-col relative">
          <input
            maxlength="200"
            v-model="newItemName"
            placeholder="Item Name"
            class="p-2 border rounded-md text-sm opacity-80 focus:opacity-100"
          />
          <p class="absolute text-xs -bottom-5 right-0 opacity-40">{{ charCount }}/200</p>
        </div>
        <input
          min="1"
          max="1000"
          v-model="newItemQuantity"
          type="number"
          placeholder="Quantity"
          class="p-2 border rounded-md text-sm opacity-80 focus:opacity-100 max-w-24"
        />
        <button
          :disabled="!newItemName || !newItemQuantity"
          class="opacity-80 hover:opacity-100 text-sm p-2 rounded-md border flex items-center gap-1 divide-x-2 disabled:opacity-50 disabled:hover:opacity-50"
        >
          Add Item <Plus :size="18" stroke-width="1" />
        </button>
      </form>
      <p v-if="inputError" class="text-red-600 text-sm">{{ inputError }}</p>

      <table class="text-left table-auto overflow-scroll divide-y w-full text-sm">
        <thead class="divide-y">
          <tr>
            <th v-for="{ key, header } in itemColumns" :key="key">{{ header }}</th>
            <th></th>
          </tr>
        </thead>
        <tbody class="divide-y">
          <ListItem v-for="item in reversedItems" :item="item" @delete="removeItem" />
        </tbody>
      </table>
    </div>
  </SectionContainer>
  <SectionContainer> </SectionContainer>
</template>
