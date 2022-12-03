<template>
  <!-- <nav>
    <router-link to="/crear">Crear</router-link> |
    <router-link to="/editar">Editar</router-link> |
    <router-link to="/listar">Listar Categorias</router-link> |
  </nav> -->

  <nav class="navbar navbar-expand navbar-light bg-light">
      <div class="nav navbar-nav">
          <router-link to="/listar" class="nav-link">Listar Categorias</router-link>
          <router-link to="/listarPost" class="nav-link">Listar Posts</router-link>
      </div>
  </nav>

  <router-view/>
</template>

<!-- <style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;
}

nav a {
  font-weight: bold;
  color: #2c3e50;
}

nav a.router-link-exact-active {
  color: #42b983;
}
</style> -->
