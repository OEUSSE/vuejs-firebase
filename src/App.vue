<template>
  <div id="app" class="container">
    <h1>Vue and Firebase</h1>
    <hr>
    <div class="card">
      <div class="card-header">
        <h3>Agrega un enlace</h3>
      </div>
      <div class="card-body ">
        <form v-on:submit.prevent="addLink" class="form-inline">
          <div class="form-group mr-3">
            <label for="title" class="mr-2">Título</label>
            <input
              type="text"
              name="title"
              placeholder="Título"
              v-model="newLink.title"
              class="form-control">
          </div>
          <div class="form-group mr-3">
            <label for="author" class="mr-2">Autor</label>
            <input
              type="text"
              name="author"
              placeholder="Título"
              v-model="newLink.author"
              class="form-control">
          </div>
          <div class="form-group mr-3">
            <label for="url" class="mr-2">URL</label>
            <input
              type="text"
              name="url"
              placeholder="Título"
              v-model="newLink.url"
              class="form-control">
          </div>
          <button type="submit" class="btn btn-success">Agregar</button>
        </form>
      </div>
    </div>
    <hr>
    <div class="card">
      <div class="card-header">
        <h3 class="card-title">Lista de links</h3>
      </div>
      <div class="card-body">
        <table class="table table-stripped">
          <thead>
            <tr>
              <th>Título</th>
              <th>Autor</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(link, index) in links" :key="index">
              <td>
                <a :href="link.url" target="_blank">{{ link.title }}</a>
              </td>
              <td>
                {{ link.author }}
              </td>
              <td>
                <button class="btn btn-danger" @click="deleteLink(link)"><i class="fa fa-trash"></i></button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'
import toastr from 'toastr'

const config = {
  apiKey: "AIzaSyCX5QUhXWPTJiRWHzKtNrSQGMuCIvAPWco",
  authDomain: "vuejs-links-b2188.firebaseapp.com",
  databaseURL: "https://vuejs-links-b2188.firebaseio.com",
  projectId: "vuejs-links-b2188",
  storageBucket: "vuejs-links-b2188.appspot.com",
  messagingSenderId: "684833092955"
}

const app = firebase.initializeApp(config)
const db = app.database()

const linksRef = db.ref('links')

export default {
  name: 'App',
  firebase: {
    links: linksRef
  },
  data () {
    return {
      newLink: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addLink () {
      linksRef.push(this.newLink)
      this.clearForm()
    },
    deleteLink (link) {
      linksRef.child(link['.key']).remove()
      toastr.success('Enlace eliminado con éxito')
    },
    clearForm () {
      // Object.entries(this.newLink).forEach(([key, val]) => this.newLink[key] = '')
      // Object.keys(a).forEach(key => a[key] = '')

      for (let val in this.newLink) {
        this.newLink[val] = ''
      }
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
