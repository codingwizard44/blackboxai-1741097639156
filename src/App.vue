<template>
  <div id="app">
    <button @click="showModal = true" class="open-modal-btn">
      Open Customer Search
    </button>
    
    <CustomerSearchModal 
      v-if="showModal"
      @close="showModal = false"
    />
  </div>
</template>

<script>
import { ref } from 'vue'
import CustomerSearchModal from './components/CustomerSearchModal.vue'

export default {
  name: 'App',
  components: {
    CustomerSearchModal
  },
  setup() {
    const showModal = ref(false)
    
    return {
      showModal
    }
  }
}
</script>

<style>
body {
  margin: 0;
  font-family: 'Inter', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

#app {
  padding: 20px;
}

.open-modal-btn {
  padding: 12px 24px;
  background-color: #3b82f6;
  color: white;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-weight: 500;
  transition: background-color 0.2s;
}

.open-modal-btn:hover {
  background-color: #2563eb;
}
</style>
