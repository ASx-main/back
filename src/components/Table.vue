/* eslint-disable global-require */
<template>
  <div>
    <table border="1">
      <thead>
      <tr>
        <th class="headers" v-for="header of headers" :key="header">
          {{ header }}
          <template v-if="header !== 'person'">
            <input type="text" @input="searching($event, header)">
            <button @click="sorting(header)">Sort</button>
          </template>
        </th>
      </tr>
      </thead>
      <tbody>
      <template v-if="users.length > 0">
        <tr v-for="(user, index) of users" :key="index">
          <td v-for="(item, key) of user" :key="key">
            <template v-if="key === 'person'">
              <span style="margin-right: 5px;"
                    v-for="person of item"
                    :key="person"
              >{{ person }}</span>
            </template>
            <template v-else>
              {{ item }}
            </template>
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
    users: {
      type: Array,
      required: true
    }
  },
  data () {
    return {
      headers: Object.keys(this.users[0])
    }
  },
  methods: {
    searching (e, key) {
      this.$emit('searching', e.target.value, key)
    },
    sorting (key) {
      this.$emit('sorting', key)
    }
  }
}

</script>

<style>
.headers {
  cursor: pointer;
}
</style>
