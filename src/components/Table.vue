<template>
  <div>
    <table class="main" border="1">
      <thead>
      <tr class="table" v-if="headers.length">
        <th class="headers" v-for="header of headers" :key="header">
          {{ header }}
          <input class="input-filter" @input="filter($event, header)" type="text">
          <button class="btn-sort" @click="sort(header)">Sort</button>
        </th>
      </tr>
      </thead>

      <tbody>
      <template v-if="data.length">
        <tr  v-for="(item, index) of data" :key="index">
          <td class="rows padding-table" v-for="(value, key) of item" :key="key">
            {{ value }}
            <button class="btn-change" v-if="key !== 'id'" @click="update(item.id, key)">
              <img class="img" src="@/assets/pencil.png" alt="изменить">
            </button>

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
    update (index, key) {
      this.$emit('update', index, key)
    },
    sort (key) {
      this.$emit('sort', key)
    },
    filter (e, key) {
      this.$emit('filter', e.target.value, key)
    }
  }
}

</script>

<style scoped>
.input-filter {
  border-radius: 5px;
  padding: 3px;
  outline: none;
}

.btn-sort {
  padding: 5px;
  background: lightsalmon;
  outline: none;
  border: none;
  border-radius: 10px;
  width: 12%;
}

.padding-table {
  padding: 10px;
}
.main {
  width: 100%;
}

.headers {
  cursor: pointer;
}

.btn-change {
  background: #ffffff;
  border: none;
  outline: none;
  width: 10%;
}

.rows {
  text-align: left;
}

.img {
  width: 15px;
}
</style>
