<template>
  <div class="modal-overlay" @click.self="close">
    <div class="modal">
      <h3>{{ title }}</h3>
      <FolderTree v-model:selectedFolderId="selectedFolderId" :folders="folders" />
      <div class="modal-actions">
        <button @click="handleOk" :disabled="selectedFolderId === null">Ок</button>
        <button @click="close">Закрыть</button>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import FolderTree from './FolderTree.vue';

// Mock-данные для дерева папок
const folders = [
  {
    id: 1,
    name: 'Папка 1',
    children: [
      { id: 2, name: 'Папка 1.1', children: [] },
      {
        id: 3,
        name: 'Папка 1.2',
        children: [{ id: 4, name: 'Папка 1.2.1', children: [] }],
      },
    ],
  },
  { id: 5, name: 'Папка 2', children: [] },
];

const props = defineProps<{
  title: string;
}>();

const emit = defineEmits<{
  (e: 'close'): void;
  (e: 'select', folderId: number): void;
}>();

const selectedFolderId = ref<number | null>(null);

const handleOk = () => {
  if (selectedFolderId.value !== null) {
    emit('select', selectedFolderId.value);
  }
};

const close = () => {
  emit('close');
};
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal {
  background: white;
  padding: 20px;
  border-radius: 8px;
  width: 400px;
  max-height: 80vh;
  overflow-y: auto;
}
.modal-actions {
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
button {
  padding: 8px 16px;
}
</style>
