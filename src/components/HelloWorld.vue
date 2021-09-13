<template>
<div class="hello">
    <v-row
      align="center"
      justify="space-around"
      class="mt-10 mb-10"
    >
      <v-btn
        depressed
        color="error"
        @click="buttonClicked"
      >
        {{ message }}
      </v-btn>
      <v-btn
        depressed
        color="blue-grey"
        @click="getEntryList"
        class="ma-2 white--text"
      >
        UPDATE
      </v-btn>
      <v-dialog
        v-model="dialog"
        persistent
        max-width="600px"
      >
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            color="primary"
            dark
            v-bind="attrs"
            v-on="on"
          >
            POST
          </v-btn>
        </template>
        <v-card>
          <v-card-title>
            <span class="text-h5">POST FORM</span>
          </v-card-title>
          <v-card-text>
            <v-container>
              <v-row>
                <v-col cols="12">
                  <v-text-field
                    label="article_id*"
                    required
                    v-model="article_id"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="title*"
                    required
                    v-model="title"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="body*"
                    required
                    v-model="body"
                  ></v-text-field>
                </v-col>
                <v-col cols="12">
                  <v-select
                    label="status*"
                    required
                    v-model="status"
                    :items="['draft', 'public']"
                  ></v-select>
                </v-col>
                <v-col cols="12">
                  <v-text-field
                    label="author*"
                    v-model="author"
                    required
                  ></v-text-field>
                </v-col>
              </v-row>
            </v-container>
            <small>*indicates required field</small>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
              color="blue darken-1"
              text
              @click="dialog = false"
            >
              Close
            </v-btn>
            <v-btn
              color="blue darken-1"
              text
              @click="dialog = false; postEntryList()"
            >
              Save
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-row>
    <v-simple-table v-if="button_clicked">
      <template v-slot:default>
        <thead>
          <tr>
            <th class="text-left">
              id
            </th>
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
            <td>{{ item.article_id }}</td>
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
      message: "show lists",
      dialog: false,
      article_id: '',
      title: '',
      body: '',
      status: '',
      author: '',
    }
  },
  methods: {
    getEntryList: function () {
      const axios = axiosBase.create(this.$axiosCreate)
      axios.get('http://127.0.0.1:8000/v1/api/entries/')
      .then((response) => {
        this.lists = response.data
        console.log(response)
      })
      .catch((error)=> {
        console.log(error);
        console.log('omg!error!')
      })
      .finally(()=> {
        console.log('final')
      });
    },
    postEntryList: function () {
      const axios = axiosBase.create(this.$axiosCreate)
      const postData={
        "article_id": this.article_id,
        "title": this.title,
        "body": this.body,
        "created_at": "2021-09-10T03:51:34.369147Z",
        "status": this.status,
        "author": this.author
      }
      console.log(postData)
      axios.post('http://127.0.0.1:8000/v1/api/entries/',postData)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      })
      .finally(() => {
        this.getEntryList()
        this.button_clicked= true
        this.message = "show lists"
      })
    },
    buttonClicked:function () {
      this.button_clicked= !this.button_clicked
      if (this.button_clicked){
        this.message = "hide lists"
      }else{
        this.message = "show lists"
      }
      this.getEntryList()
    }
  }
}

</script>
<style scoped>
  .button{
    font-family: 'Lucida Sans', 'Lucida Sans Regular', 'Lucida Grande', 'Lucida Sans Unicode', Geneva, Verdana, sans-serif;
    font-size: 30px;
  }
</style>





