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
import NavBar from './components/NavBar.vue';
import Swal from 'sweetalert2';

export default {
  name: 'App',
  components: {
    NavBar,
  },
  data() {
    return {
      inputValue: '',
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
        return; // Salir si el input está vacío
      }

      try {
        // Realizar la solicitud POST al backend usando Fetch
        const response = await fetch('http://localhost:5148/api/messages', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({ text: this.inputValue }), // Asegúrate de que el campo coincida con el backend
        });

        // Verificar si la respuesta es exitosa
        if (response.ok) {
          await response.json();
          Swal.fire({
            icon: 'success',
            title: 'Publicado!',
            text: `Se ha publicado el siguiente mensaje: ${this.inputValue}`,
          });

          // Limpiar el input después de la publicación
          this.inputValue = '';
        } else {
          // Manejar errores en la respuesta
          const errorText = await response.text();
          Swal.fire({
            icon: 'warning',
            title: 'Error en la respuesta',
            text: `No se pudo publicar el mensaje: ${errorText}`,
          });
        }
      } catch (error) {
        // Manejar errores de red
        console.error('Error al enviar el mensaje:', error);
        Swal.fire({
          icon: 'error',
          title: 'Error',
          text: 'Ocurrió un problema al enviar el mensaje.',
        });
      }
    },
  },
};
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