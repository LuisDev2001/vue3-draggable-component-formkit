<!-- eslint-disable @typescript-eslint/no-explicit-any -->
<script setup lang="ts">
  import { computed, watch } from 'vue'
  import { useDragAndDrop } from '@formkit/drag-and-drop/vue'

  interface ListItem {
    order: number
    [key: string]: any
  }

  const props = defineProps<{
    list: ListItem[]
    handle?: string
  }>()

  const emits = defineEmits(['updateList'])

  const listModel = computed(() => props.list)

  const [refElement, listDraggable] = useDragAndDrop(listModel.value, {
    dragHandle: props.handle,
  })

  watch(() => listDraggable.value, (newListDraggable) => {
    if (newListDraggable) {
      emits('updateList', newListDraggable)
    }
  }, { deep: true })
</script>

<template>
  <div ref="refElement">
    <div
      v-for="item in listDraggable"
      :key="item.order"
    >
      <slot
        name="item"
        :item="item"
      />
    </div>
  </div>
</template>

