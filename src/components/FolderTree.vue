<template>
  <ul>
    <FolderNode
      v-for="folder in folders"
      :key="folder.id"
      :folder="folder"
      :selectedFolderId="selectedFolderId"
      @update:selectedFolderId="updateSelectedFolderId"
    />
  </ul>
</template>

<script setup lang="ts">
import type { Folder } from '../types'
import FolderNode from './FolderNode.vue'

const props = defineProps<{
  folders: Folder[]
  selectedFolderId: number | null
}>()

const emit = defineEmits<{
  (e: 'update:selectedFolderId', id: number | null): void
}>()

const updateSelectedFolderId = (id: number | null) => {
  emit('update:selectedFolderId', id)
}
</script>

<style scoped>
ul {
  list-style: none;
  padding-left: 20px;
}
</style>
