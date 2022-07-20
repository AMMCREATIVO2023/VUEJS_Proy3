<template>
  <div>
    <!-- TODO: Crear componente GitHub -->
    <input
      @keydown.enter="obtenerUsuario"
      type="text"
      v-model="nombreUsuario"
      class="form-control container"
      placeholder="Introducir usuario"
    />

    <div
      v-if="existe"
      v-show="visible"
      class="card container"
      style="width: 90%"
    >
      <img
        class="card-img-top"
        :src="datosUsuario.avatar_url"
        alt="Avatar GitHub"
      />
      <div class="card-body">
        <h5 class="card-title">{{ datosUsuario.login }}</h5>
        <p class="card-text">
          <a :href="datosUsuario.html_url" class="card-link"
            >Enlace a la url del usuario</a
          >
        </p>
        <button @click="obtenerRepositorios" class="btn btn-primary">
          Repositorios
        </button>
      </div>
      <div v-if="repositoriosVer" class="card-body container">
        <GitHubRepos :repolist="listado"></GitHubRepos>
      </div>
    </div>
    <div v-else class="alert alert-danger container" role="alert">
      El usuario introducido no existe. Intentelo de nuevo con otro usuario.
    </div>
  </div>
</template>

<script>
// TODO: Importar componente GitHubRepos
import GitHubRepos from "./GitHubRepos.vue";

export default {
  name: "GitHub",
  components: {
    // TODO: Importar componente GitHubRepos
    GitHubRepos,
  },
  data: function () {
    return {
      // TODO: crear variables de datos para el funcionamiento del componente
      nombreUsuario: "",
      datosUsuario: {},
      listado: {},
      visible: false,
      existe: true,
      repositoriosVer: false,
    };
  },
  methods: {
    obtenerUsuario: function () {
      // TODO: Función para obtener los datos de usuario de la API de GitHub

      // TODO: Añadir lógica para resetear los cambios en el interfaz: desactivar campo de envío,
      // resetear mensaje de error, mostrar lista de repositorios,...

      // Obtener datos de autenticación de usuario para hacer peticiones
      // autenticadas a la API de GitHub
      var url = "https://api.github.com/users/" + this.nombreUsuario;
      var userAuth = process.env.VUE_APP_USERNAME || "user";
      var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

      let miheader = new Headers();
      miheader.set("Autorization", "Basic " + btoa(userAuth + ":" + passAuth));
      // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
      fetch(url, { method: "GET", miheader })
        .then((response) => response.json())
        .then((result) => {
          if (result.login) {
            this.datosUsuario = result;
            this.visible = true;
            this.existe = true;
            this.repositoriosVer = false;
          } else {
            this.visible = false;
            this.existe = false;
          }
        });
      // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
    },
    obtenerRepositorios: function () {
      // TODO: Función para obtener los repositorios del usuario desde la API de GítHub
      // La URL de los repositorios de usuario se puede obtener a través del campo 'repos_url' de los datos del usuario
      var urlRepositorios = this.datosUsuario.repos_url;
      // Obtener datos de autenticación de usuario para hacer peticiones
      // autenticadas a la API de GitHub
      var userAuth = process.env.VUE_APP_USERNAME || "user";
      var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

      // TODO: realizar petición fetch par obtener los datos y mostrar la información en la página
      let miheader = new Headers();
      miheader.set("Autorization", "Basic " + btoa(userAuth + ":" + passAuth));

      fetch(urlRepositorios, { method: "GET", miheader })
        .then((response) => response.json())
        .then((result) => {
          this.listado = result;
          this.repositoriosVer = true;
        });

      // Ejemplo de paso de datos de autorización con fetch: https://stackoverflow.com/questions/43842793/basic-authentication-with-fetch
    },
  },
};
</script>

<style scoped>
img {
  width: 20%;
  margin: 30px;
}

.container {
  margin-bottom: 20px;
}
</style>
