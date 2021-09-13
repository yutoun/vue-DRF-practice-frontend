<template>
<div class="hello">
    <v-row
      align="center"
      justify="space-around"
      class="mt-10 mb-10"
    >
      <v-btn
        depressed
        color="primary"
        @click="getUserList"
      >
        show lists
      </v-btn>
    </v-row>
    <v-simple-table v-if="button_clicked">
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">
              Title
            </th>
            <th class="text-left">
              contents
            </th>
            <th class="text-left">
              created_at
            </th>
            <th class="text-left">
              status
            </th>
            <th class="text-left">
              author
            </th>
          </tr>
        </thead>
        <tbody>
          <tr
            v-for="item in lists"
            :key="item.title"
          >
            <td>{{ item.title }}</td>
            <td>{{ item.body }}</td>
            <td>{{ item.created_at }}</td>
            <td>{{ item.status }}</td>
            <td>{{ item.author }}</td>
          </tr>
        </tbody>
      </template>
    </v-simple-table>
  </div>
</template>

<script>
import axiosBase from 'axios'

export default {
  name: 'HelloWorld',
  data () {
    return {
      lists: null,
      button_clicked: false,
    }
  },
  methods: {
    getUserList: function () {
      const axios = axiosBase.create(this.$axiosCreate)
      axios.get('http://127.0.0.1:8000/v1/api/entries/')
      .then(response => (this.lists = response.data.results))
      .catch((error)=> {
        console.log(error);
        console.log('omg!error!')
      })
      .finally(()=> {
        console.log('final')
        this.button_clicked=true
      });
    },
  }
}

</script>
<style scoped>
  .button{
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-size: 30px;
  }
</style>





