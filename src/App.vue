<template>
  <div id="app">
    <NavBar />
    <h1>Aplicación Publicador</h1>
    <div>
      <input v-model="inputValue" type="text" placeholder="Escribe algo para publicar..." />
      <select v-model="selectedTopic">
        <option value="" disabled>Selecciona un tópico</option>
        <option value="topico1">Tópico 1</option>
        <option value="topico2">Tópico 2</option>
      </select>
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
      selectedTopic: '',
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
        return;
      }

      if (this.selectedTopic === '') {
        Swal.fire({
          icon: 'error',
          title: 'Oops...',
          text: 'Debes seleccionar un tópico!',
        });
        return;
      }

      try {
        const response = await fetch('http://localhost:5148/api/messages', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify({
            text: this.inputValue,
            topic: this.selectedTopic,
          }),
        });

        if (response.ok) {
          await response.json();
          Swal.fire({
            icon: 'success',
            title: 'Publicado!',
            text: `Se ha publicado el siguiente mensaje en ${this.selectedTopic}: ${this.inputValue}`,
          });

          this.inputValue = '';
          this.selectedTopic = ''; // Limpiar el dropdown
        } else {
          const errorText = await response.text();
          Swal.fire({
            icon: 'warning',
            title: 'Error en la respuesta',
            text: `No se pudo publicar el mensaje: ${errorText}`,
          });
        }
      } catch (error) {
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

input[type="text"],
select {
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
