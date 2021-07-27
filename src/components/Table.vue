<template>
  <div>
    <table border="1">
      <thead>
      <tr v-if="headers.length">
        <th class="headers" v-for="header of headers" :key="header">
          {{ header }}
          <input @input="filter($event, header)" type="text">
          <button @click="sort(header)">Sort</button>
        </th>
      </tr>
      </thead>

      <tbody>
      <template v-if="data.length">
        <tr v-for="(item, index) of data" :key="index">
          <td v-for="(value, key) of item" :key="key">
            {{ value }}
            <button v-if="key !== 'id'" @click="update(item.id, key)" >Change</button>
          </td>
        </tr>
      </template>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  props: {
    headers: {
      type: Array,
      default () {
        return []
      }
    },
    data: {
      type: Array,
      required: true
    }
  },
  methods: {
    sort (key) {
      this.$emit('sort', key)
    },
    filter (e, key) {
      this.$emit('filter', e.target.value, key)
    },
    update (index, key) {
      this.$emit('update', index, key)
    }
  }
}

</script>

<style>
.headers {
  cursor: pointer;
}
</style>
