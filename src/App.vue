<template>
  <div id="app">
    <nav class = "navbar navbar-dark bg-primary">
      <a href="/" class="navbar-brand">Proyecto Vuejs</a>
    </nav>
    <div class="container">
      <div class="row mt-5">
        <div class=col-sm-4>
          <div class=card>
            <div class=card-header>
              <h3>Mi Sitio Web</h3>
            </div>
            <div class="card-body">
              <form @submit.prevent="addLista">
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newLista.nombre"
                  placeholder="Nombre">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newLista.autor"
                  placeholder="Autor">
                </div>
                <div class="form-group">
                  <input type="text" class="form-control" v-model="newLista.url"
                  placeholder="URL">
                </div>
                <button type="submit" class="btn btn-primary">
                  GUARDAR
                </button>
              </form>
            </div>
          </div>
        </div>
        <div class="col sm-8 text-center">
          <img src="./assets/logo.png">
          <div>
            <div class=card>
              <div class=card-header>
                <h3>Listas</h3>
              </div>
              <div class="card-body">
              <table class="table table-striped table-bordered">
                <thead>
                  <tr>
                    <th>Nombre</th>
                    <th>Autor</th>
                    <th>Operaciones</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="lis in listas">
                    <td>
                      <a v-bind:href="lis.url" target="_blank" >{{lis.nombre}}</a>
                    </td>
                    <td>
                      {{lis.autor}}
                    </td>
                    <td>
                      <button class="btn btn-danger" @click="deleteLista(lis)">
                        ELIMINAR
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
              </div>
            </div>
          </div>
          </div>
        </div>
      </div>
    </div>

  </div>
</template>

<script>
import Firebase from 'firebase';
import config from './config';
let app = Firebase.initializeApp(config);
let db = app.database();
let listasRef = db.ref('listas');

export default {
  name: 'App',
  firebase:{
    listas: listasRef
  },
  data() {
    return{
      newLista: {
      nombre: '',
      autor: '',
      url: ''
      }
    }
  },
  methods: {
    addLista(){
    listasRef.push(this.newLista);
    this.newLista.nombre = '';
    this.newLista.autor = '';
    this.newLista.url = '';
    },
    deleteLista(regitro) {
    if(confirm('Desea eliminar el Registro?')){
    listasRef.child(regitro['.key']).remove();
      }
    }
  }
}
</script>

<style>
#app {

}
</style>
