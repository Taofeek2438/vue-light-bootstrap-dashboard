<template>
  <card>
    <h4 slot="header" class="card-title">Create Inventory</h4>
    <form>
      <div class="row">
        <div class="col-md-6">
          <base-input
            type="date"
            label="Date"
            placeholder="Select Date"
            v-model="inventory.date"
          >
          </base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="text"
            label="Description"
            placeholder="Description"
            v-model="inventory.description"
          >
          </base-input>
        </div>
      </div>

      <div class="row">
        <div class="col-md-6">
          <base-input
            type="number"
            label="Quantity Received"
            placeholder="Quantity"
            v-model="inventory.quantityReceived"
          >
          </base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="number"
            label="Quantity Issued"
            placeholder="Quantity"
            v-model="inventory.quantityIssued"
          >
          </base-input>
        </div>
        <div class="col-md-6">
          <base-input
            type="number"
            label="Balance"
            placeholder="Balance"
            v-model="inventory.balance"
          >
          </base-input>
        </div>
      </div>

      <div class="text-center">
        <button
          type="submit"
          class="btn btn-info btn-fill float-right"
          @click.prevent="createInventory"
        >
          Create Inventory
        </button>
      </div>
      <div class="clearfix"></div>
    </form>
  </card>
</template>

<script>
import Card from 'src/components/Cards/Card.vue'
import axios from 'axios'

export default {
  components: {
    Card
  },
  data() {
    return {
      inventory: {
        date: '',
        description: '',
        quantityReceived: '',
        quantityIssued: '',
        balance: ''
      }
    }
  },
  methods: {
    async createInventory() {
      try {
        const response = await axios.post('http://localhost:3000/inventories/create', {
          ...this.inventory
        }, {
          headers: {
            'Content-Type': 'application/json'
          }
        })
        if (response.status !== 201) {
          throw new Error('Failed to create inventory')
        }
        alert('Inventory created successfully!')
        // Optionally reset the form or navigate to another page
        this.resetForm()
      } catch (error) {
        console.error('Error creating inventory:', error)
        alert('Failed to create inventory. Please try again.')
      }
    },
    resetForm() {
      this.inventory = {
        date: '',
        description: '',
        quantityReceived: '',
        balance: ''
      }
    }
  }
}
</script>

<style>

</style>
