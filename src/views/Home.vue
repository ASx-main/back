<template>
  <div>
    <Table
      :data="getProducts"
      :headers="productsHeaders"
      @sort="sort"
      @filter="filter"
      @update="update"
    />
    <div v-if="isEdit">
      <input class="input" v-model="productNewValue" type="text">
      <button class="btn-add" @click="updateProduct">Обновить</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import Table from '@/components/Table'

export default {
  name: 'Home',
  components: { Table },
  data () {
    return {
      isEdit: false,
      products: [],
      productsHeaders: [],
      key: '',
      productId: '',
      direction: '',
      productNewValue: '',
      search: ''
    }
  },
  async created () {
    await this.getData()
  },
  computed: {
    getProducts () {
      let newProducts = JSON.parse(JSON.stringify(this.products))

      if (this.search !== '') {
        newProducts = newProducts.filter((product) => {
          return product[this.key].toString().toLowerCase().includes(this.search.toLowerCase())
        })
      }

      newProducts.sort((a, b) => {
        if (this.direction === 'asc') {
          if (a[this.key] < b[this.key]) {
            return -1
          }

          if (a[this.key] > b[this.key]) {
            return 1
          }
        } else if (this.direction === 'desc') {
          if (a[this.key] > b[this.key]) {
            return -1
          }

          if (a[this.key] < b[this.key]) {
            return 1
          }
        }
        return 0
      })

      return newProducts
    }
  },
  methods: {
    async getData () {
      try {
        const res = await axios.get('/products')
        this.products = res.data
        this.productsHeaders = Object.keys(res.data[0])
      } catch (e) {
        console.error(e)
      }
    },
    async updateProduct () {
      try {
        await axios.put(`/products/${this.productId}`, {
          ...this.products.find(product => product.id === this.productId),
          [this.key]: this.productNewValue
        })

        await this.getData()
      } catch (e) {
        console.error(e)
      } finally {
        this.isEdit = false
        this.productNewValue = ''
      }
    },
    update (id, key) {
      this.isEdit = true
      this.key = key
      this.productId = id
    },
    filter (val, key) {
      this.search = val
      this.key = key
    },
    sort (key) {
      this.key = key
      if (this.direction === '') {
        this.direction = 'asc'
      } else if (this.direction === 'asc') {
        this.direction = 'desc'
      } else {
        this.direction = ''
      }
    }
  }
}
</script>

<style scoped>
.input {
  margin: 10px;
  padding: 5px;
  outline: none;
  border-radius: 5px;
}

.btn-add {
  padding: 5px;
  background: lightsalmon;
  outline: none;
  border: none;
  border-radius: 10px;
  width: 12%;
}
</style>
