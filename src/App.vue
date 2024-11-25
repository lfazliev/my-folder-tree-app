<template>
  <div id="app">
    <button @click="openModal">Открыть</button>
    <FolderModal
      v-if="isModalOpen"
      :title="'Выберите папку'"
      @close="closeModal"
      @select="handleSelect"
    />
    <p v-show="selectedFolderId">Выбранная папка ID: {{ selectedFolderId }}</p>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import FolderModal from './components/FolderModal.vue';

const isModalOpen = ref(false);
const selectedFolderId = ref<number | null>(null);

const openModal = () => {
  isModalOpen.value = true;
};

const closeModal = () => {
  isModalOpen.value = false;
};

const handleSelect = (folderId: number) => {
  console.log(`Выбранная папка ID: ${folderId}`);
  selectedFolderId.value = folderId;
  isModalOpen.value = false;
};
</script>

<style scoped>
#app {
  display: flex;
  height: 100vh;
  justify-content: center;
  align-items: center;
  gap: 1em;
}
button {
  padding: 10px 20px;
  font-size: 16px;
}
</style>
