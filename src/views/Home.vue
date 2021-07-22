<template>
  <div>
    <div v-for="product in products" :key="product.id">
      <span @click="edit(product.id, 'name')">{{ product.name }}</span>
      :
      <span @click="edit(product.id, 'price')">{{ product.price }}</span>
    </div>
    <div v-if="isEdit">
      <input v-model="productNewValue" type="text">
      <button @click="updateProduct">Обновить</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'Home',
  data () {
    return {
      products: [],
      productKey: '',
      productId: '',
      productNewValue: '',
      isEdit: false
    }
  },
  async created () {
    await this.getData()
  },
  methods: {
    async getData () {
      try {
        const res = await axios.get('/products')
        this.products = res.data
      } catch (e) {
        console.error(e)
      }
    },

    async updateProduct () {
      try {
        await axios.put(`/products/${this.productId}`, {
          ...this.products.find(product => product.id === this.productId),
          [this.productKey]: this.productNewValue
        })

        await this.getData()
      } catch (e) {
        console.error(e)
      } finally {
        this.isEdit = false
        this.productNewValue = ''
      }
    },
    edit (id, key) {
      this.isEdit = true
      this.productId = id
      this.productKey = key
    }
  }
}
</script>
