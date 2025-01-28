<script setup lang="ts">
const props = defineProps({
  item: { type: Object, required: true },
})

const emit = defineEmits(['delete'])
</script>

<template>
  <tr :key="props.item?.id" class="cursor-pointer" @click="item.purchased = !item?.purchased">
    <td
      :class="{
        'line-through': item?.purchased,
      }"
      class="py-2"
    >
      {{ item?.label }}
    </td>
    <td class="py-2">{{ item?.quantity }}</td>
    <td class="py-2">
      <button class="text-red-600 hover:opacity-50" @click="emit('delete', item?.id)">
        Delete
      </button>
    </td>
  </tr>
</template>
