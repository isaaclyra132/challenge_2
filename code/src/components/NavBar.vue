<template>
  <div class="title">
    <b-navbar toggleable="lg" type="dark" variant="info">
      <b-navbar-brand class="ml-2">
        <img :src="require('../assets/logo.png')" class="logo"
          @click="methodFoo"
        />
        <b>{{ appName }}</b>
      </b-navbar-brand>
    </b-navbar>
  </div>
</template>

<script>
import {
  BNavbar,
  BNavbarBrand,
} from 'bootstrap-vue'

export default {
  components: {
    BNavbar,
    BNavbarBrand,
  },
  data() {
    const appName = ''

    return {
      appName,
    }
  },
  created() {
    // Implemente aqui o GET dos dados da API REST
    // para que isso ocorra na inicialização da pagina
    this.appName = 'Melhor Frete'
  },
  methods: {
    // Implemente aqui os metodos utilizados na pagina
    methodFoo() {
      console.log(this.appName)
    },
  },
}
</script>

<style scoped>
.title .navbar {
  background-color: #00aca6 !important;
  display: flex;
  align-items: center;
  justify-content: left;
}

.title .navbar-brand {
  margin-left: 20px;
  cursor: pointer;
}

.logo {
  width: 2.5rem;
  margin-right: 1rem;
}

.logo:hover {
  background: red;
}
</style>
