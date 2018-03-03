<template>
  <div id="app" class="container">
      <img src="./assets/logo.png" width="12%">
      <img src="./assets/logoF.png" width="12%">
      <h1>Vue and Firebase</h1>
      <div class="card">
        <div class="card-header">
          <h3>Add a Link</h3>
        </div>
        <div class="card-body">
          <form v-on:submit.prevent="addLink" class="form-inline">
            <div class="form-group">
              <label>Title</label>
              <input v-model="newLink.title" class="form-control" type="text" placeholder="Title">
            </div>
            <div class="form-group">
              <label>Author</label>
              <input v-model="newLink.author" class="form-control" type="text" placeholder="Author">
            </div>
            <div class="form-group">
              <label>URL</label>
              <input v-model="newLink.url" class="form-control" type="text" placeholder="URL">
            </div>
            <input type="submit" class="btn btn-success" value="Add a Link">
          </form>    
        </div>
      </div><hr>
      <div class="card">
        <div class="card-header">
          <h3 class="card-title">Link list</h3>
        </div>
        <dir class="card-body">
          <table class="table table-striped">
            <thead>
              <tr>
                <th>Title</th>
                <th>Author</th>
                <th>Delete</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="link in links">
                <td><a v-bind:href="link.url" class="alert-link" target="_blank">{{ link.title }}</a></td>
                <td>{{ link.author }}</td>
                <td><button v-on:click="deleteLink(link)" class="btn btn-danger"><i class="fa fa-trash-o" aria-hidden="true"></i></button></td>
              </tr>
            </tbody>
          </table>
        </dir>
      </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'
import Firebase from 'firebase'

let config = { // Agregar Firebase a la app web 
    apiKey: "AIzaSyDS2_Nqxl_oqQ5NVxG4RVWC_2wt-E34jD8",
    authDomain: "vuefire-dcfac.firebaseapp.com",
    databaseURL: "https://vuefire-dcfac.firebaseio.com",
    projectId: "vuefire-dcfac",
    storageBucket: "vuefire-dcfac.appspot.com",
    messagingSenderId: "497887099392"
}

let app = Firebase.initializeApp(config) //Conexión a la DB
let db = app.database() // Nos devuelve la DB

let linksRef = db.ref('links')

export default {
  name: 'App',
  firebase: {
    links: linksRef
  },
  data() {
    return {
      newLink: {
        title: '',
        author: '',
        url: ''
      }
    }
  },
  methods: {
    addLink: function () {
      linksRef.push(this.newLink) // Insertar
      this.newLink.title = ''
      this.newLink.author = ''
      this.newLink.url = ''
    },
    deleteLink: function (link) {
      linksRef.child(link['.key']).remove() // Eliminar 
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

.container input {
  margin: 10px;
}
</style>
