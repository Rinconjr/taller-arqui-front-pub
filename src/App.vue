<template>
  <div id="app">
    <NavBar />
    <h1>Aplicación Publicador</h1>
    <div>
      <input v-model="inputValue" type="text" placeholder="Escribe algo para publicar..." />
      <button @click="sendData">Enviar</button>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import NavBar from './components/NavBar.vue';
import Swal from 'sweetalert2';

export default {
  name: 'App',
  components: {
    NavBar,
  },
  data() {
    return {
      inputValue: ''
    };
  },
  methods: {
    async sendData() {
      if (this.inputValue.trim() === '') {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'Debes escribir algo para publicar!',
        });
      } else {
        try {
          // Realiza la solicitud POST al backend con el formato JSON correcto
          await axios.post('http://localhost:7108/api/messages', {
            text: this.inputValue
          }, {
            headers: {
              'Content-Type': 'application/json'
            }
          });


          // Muestra una alerta de éxito
          Swal.fire({
            icon: 'success',
            title: 'Publicado!',
            html: `Tu mensaje ha sido publicado correctamente! <strong>${this.inputValue}</strong>`,
          });

          // Limpia el campo de entrada
          this.inputValue = '';
        } catch (error) {
          // Muestra una alerta de error en caso de fallo
          Swal.fire({
            icon: 'error',
            title: 'Error',
            text: `Hubo un problema al publicar el mensaje: ${error.response ? error.response.data : error.message}`,
          });
        }
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
}

h1 {
  margin-top: 1em;
  font-size: 2.5em;
  font-weight: bold;
  color: #42b983;
}

input[type="text"] {
  padding: 0.5em;
  margin-right: 0.5em;
}

button {
  padding: 0.5em 1em;
  background-color: #42b983;
  color: white;
  border: none;
  cursor: pointer;
}

button:hover {
  background-color: #369b72;
}
</style>
