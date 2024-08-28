<template>
  <card>
    <h4 slot="header" class="card-title">Create Inventory</h4>
    <form @submit.prevent="createInventory">
      <div class="row">
        <div class="col-md-6">
          <base-input
            type="date"
            label="Date"
            placeholder="Select Date"
            v-model="date"
          ></base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="text"
            label="Description"
            placeholder="Description"
            v-model="description"
          ></base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
          <base-input
            type="text"
            label="Quantity Received"
            placeholder="Quantity"
            v-model="quantityReceived"
          ></base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="text"
            label="Quantity Issued"
            placeholder="Quantity"
            v-model="quantityIssued"
          ></base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="number"
            label="Balance"
            placeholder="Balance"
            v-model.number="balance"
          ></base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="text"
            label="Inventory Name"
            placeholder="Inventory Name"
            v-model="inventory_name"
          ></base-input>
        </div>
      </div>

      <div class="text-center">
        <button type="submit" class="btn btn-info btn-fill float-right" :disabled="loading">
          <span v-if="loading" class="loader"></span>
          <span v-else>Create Inventory</span>
        </button>
      </div>
      <div class="clearfix"></div>

      <div v-if="error" class="error">{{ error }}</div>
      <div v-if="success" class="success">{{ success }}</div>
    </form>
  </card>
</template>

<script>
import Card from 'src/components/Cards/Card.vue'
import axios from 'axios'
import { ref } from 'vue'

export default {
  components: {
    Card
  },
  setup() {
    const date = ref('')
    const description = ref('')
    const quantityReceived = ref('')
    const quantityIssued = ref('')
    const balance = ref(0)
    const inventory_name = ref('')
    const loading = ref(false)
    const error = ref('')
    const success = ref('')

    const createInventory = async () => {
      loading.value = true
      error.value = ''
      success.value = ''

      try {
        const response = await axios.post('/api/inventories/create', {
          date: date.value,
          description: description.value,
          quantityReceived: quantityReceived.value,
          quantityIssued: quantityIssued.value,
          balance: balance.value,
          inventoryName: inventory_name.value
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        })

        if (response.status !== 201) {
          throw new Error('Failed to create inventory')
        }

        success.value = 'Inventory created successfully!'
        resetForm()
      } catch (err) {
        error.value = 'Failed to create inventory. Please try again.'
      } finally {
        loading.value = false
      }
    }

    const resetForm = () => {
      date.value = ''
      description.value = ''
      quantityReceived.value = ''
      quantityIssued.value = ''
      balance.value = 0
      inventory_name.value = ''
    }

    return {
      date,
      description,
      quantityReceived,
      quantityIssued,
      balance,
      inventory_name,
      loading,
      error,
      success,
      createInventory
    }
  }
}
</script>

<style scoped>
.error {
  color: red;
  margin-top: 1rem;
}

.success {
  color: green;
  margin-top: 1rem;
}

.loader {
  border: 4px solid #f3f3f3; /* Light grey */
  border-top: 4px solid #004080; /* Blue */
  border-radius: 50%;
  width: 20px;
  height: 20px;
  animation: spin 2s linear infinite;
  display: inline-block;
  vertical-align: middle;
  margin-right: 10px;
}

@keyframes spin {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
