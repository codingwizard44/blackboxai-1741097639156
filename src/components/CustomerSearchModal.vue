<template>
  <div class="modal-overlay" @click.self="closeModal">
    <div class="modal-container">
      <!-- Header -->
      <div class="modal-header">
        <button class="back-button" @click="closeModal">
          <i class="fas fa-arrow-left"></i>
        </button>
        <div class="header-content">
          <h2>Return items</h2>
          <h3>Select customer</h3>
        </div>
        <button class="close-button" @click="closeModal">
          <i class="fas fa-times"></i>
        </button>
      </div>

      <!-- Search -->
      <div class="search-container">
        <div class="search-input">
          <i class="fas fa-search search-icon"></i>
          <input 
            type="text" 
            v-model="searchQuery" 
            placeholder="Search for ID or name"
            @input="searchCustomers"
          >
        </div>
        <div class="results-count">{{ filteredCustomers.length }} of {{ customers.length }}</div>
      </div>

      <!-- Customer List -->
      <div class="customers-list">
        <div 
          v-for="customer in filteredCustomers" 
          :key="customer.datasource_id" 
          class="customer-item"
        >
          <div class="customer-initials" :style="{ backgroundColor: getRandomColor(customer.datasource_id) }">
            {{ getInitials(customer.name) }}
          </div>
          <div class="customer-info">
            <div class="customer-id">{{ customer.datasource_id }}</div>
            <div class="customer-name">{{ customer.name }}</div>
          </div>
          <div class="customer-city">
            {{ customer.store_locations?.[0]?.city || 'No location' }}
          </div>
          <button class="start-return" @click="startReturn(customer)">
            Start return
            <i class="fas fa-arrow-right"></i>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref, computed } from 'vue'
import { customers } from '../data/customers'

export default {
  name: 'CustomerSearchModal',
  emits: ['close'],
  
  setup(props, { emit }) {
    const searchQuery = ref('')
    const customersList = ref(customers.result)

    const filteredCustomers = computed(() => {
      if (!searchQuery.value) return customersList.value
      const query = searchQuery.value.toLowerCase()
      return customersList.value.filter(customer => 
        customer.name.toLowerCase().includes(query) ||
        customer.datasource_id.toLowerCase().includes(query)
      )
    })

    const getInitials = (name) => {
      return name
        .split(' ')
        .slice(0, 2)
        .map(word => word[0])
        .join('')
        .toUpperCase()
    }

    const getRandomColor = (id) => {
      const colors = ['#34D399', '#60A5FA', '#F472B6', '#A78BFA', '#FBBF24']
      const index = parseInt(id.replace(/\D/g, '')) % colors.length
      return colors[index]
    }

    const startReturn = (customer) => {
      console.log('Starting return for customer:', customer)
      // Here you would typically navigate to the return process
      // with the selected customer
    }

    const closeModal = () => {
      emit('close')
    }

    return {
      searchQuery,
      customers: customersList,
      filteredCustomers,
      getInitials,
      getRandomColor,
      startReturn,
      closeModal
    }
  }
}
</script>

<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 32px;
}

.modal-container {
  background: white;
  border-radius: 16px;
  width: 100%;
  max-width: 600px;
  max-height: calc(100vh - 64px);
  display: flex;
  flex-direction: column;
}

.modal-header {
  display: flex;
  align-items: center;
  padding: 16px;
  border-bottom: 1px solid #e5e7eb;
}

.back-button,
.close-button {
  width: 40px;
  height: 40px;
  border: none;
  background: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  color: #6b7280;
}

.header-content {
  flex: 1;
  margin: 0 16px;
}

.header-content h2 {
  font-size: 14px;
  color: #6b7280;
  margin: 0;
}

.header-content h3 {
  font-size: 18px;
  font-weight: 600;
  margin: 4px 0 0;
}

.search-container {
  padding: 16px;
  display: flex;
  align-items: center;
  gap: 16px;
}

.search-input {
  flex: 1;
  position: relative;
}

.search-icon {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: #6b7280;
}

input {
  width: 100%;
  padding: 12px 12px 12px 40px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  font-size: 16px;
}

.results-count {
  color: #6b7280;
  font-size: 14px;
  white-space: nowrap;
}

.customers-list {
  flex: 1;
  overflow-y: auto;
  padding: 0 16px 16px;
}

.customer-item {
  display: flex;
  align-items: center;
  padding: 12px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  margin-bottom: 8px;
  gap: 16px;
}

.customer-initials {
  width: 40px;
  height: 40px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: white;
  font-weight: 600;
  flex-shrink: 0;
}

.customer-info {
  flex: 1;
  min-width: 0;
}

.customer-id {
  font-size: 14px;
  color: #6b7280;
}

.customer-name {
  font-weight: 600;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.customer-city {
  color: #6b7280;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  margin-right: 16px;
  min-width: 100px;
}

.start-return {
  background-color: #3b82f6;
  color: white;
  border: none;
  padding: 8px 16px;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  gap: 8px;
  font-weight: 500;
  flex-shrink: 0;
  transition: background-color 0.2s;
}

.start-return:hover {
  background-color: #2563eb;
}

@media (max-width: 768px) {
  .modal-overlay {
    padding: 16px;
  }
  
  .customer-city {
    display: none;
  }
}
</style>
