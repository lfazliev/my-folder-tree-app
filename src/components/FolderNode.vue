<template>
  <li>
    <div class="folder" :class="{ selected: isSelected }" @click="selectFolder">
      <span v-if="hasChildren" @click.stop="toggle">
        {{ isExpanded ? '▼ 📂' : '▶ 📁' }}
      </span>
      <span v-else>📁</span>
      {{ folder.name }}
    </div>
    <ul v-if="hasChildren && isExpanded">
      <FolderNode
        v-for="child in folder.children"
        :key="child.id"
        :folder="child"
        :selectedFolderId="selectedFolderId"
        @update:selectedFolderId="updateSelectedFolderId"
      />
    </ul>
  </li>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import type { Folder } from '../types'
import FolderNode from './FolderNode.vue'

const props = defineProps<{
  folder: Folder
  selectedFolderId: number | null
}>()

const emit = defineEmits<{
  (e: 'update:selectedFolderId', id: number | null): void
}>()

const isExpanded = ref(false)

const hasChildren = computed(() => props.folder.children && props.folder.children.length > 0)

const toggle = () => {
  isExpanded.value = !isExpanded.value
}

const isSelected = computed(() => props.folder.id === props.selectedFolderId)

const selectFolder = () => {
  emit('update:selectedFolderId', props.folder.id)
}

const updateSelectedFolderId = (id: number | null) => {
  emit('update:selectedFolderId', id)
}
</script>

<style scoped>
.folder {
  cursor: pointer;
  padding: 5px;
  display: flex;
  align-items: center;
}
.folder.selected {
  background-color: #d0eaff;
}
.folder span {
  margin-right: 8px;
}
ul {
  list-style: none;
  padding-left: 20px;
}
</style>
