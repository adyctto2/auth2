<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="d-flex align-center">
          <v-img
            alt="Vuetify Logo"
            class="shrink mr-2"
            contain
            src="./assets/logo.jpg"
            transition="scale-transition"
            width="40"
          />
           <h2><strong> TSJ </strong></h2>

        
      </div>
      <div  v-if="showAdminBoard">
        <v-btn
          to="/admin"
          text
        >
          <span class="mr-2">Administrador Board</span>
        </v-btn>
      </div>
      
      <div  v-if="showModeratorBoard">
        <v-btn
          to="/mod"
          text
        >
          <span class="mr-2">Moderator Board</span>
        </v-btn>
      </div>
      
      <div>
        <v-btn
          v-if="currentUser"
          to="/user"
          text
        >
          <span class="mr-2">Usuario</span>
        </v-btn>
      </div>
      <v-spacer></v-spacer>
    
      <div v-if="!currentUser" class="navbar-nav ml-auto">
          
        <v-btn
          to="/register"
          text
        >
          <span class="mr-2">Registrarse</span>
        </v-btn>
        <v-btn
          to="/login"
          text
        >
          <span class="mr-2">Iniciar Sesion</span>
        </v-btn>
      </div>
      
      <div v-if="currentUser" class="navbar-nav ml-auto">
        <v-btn
          to="/profile"
          text
        >
          <span class="mr-2">{{ currentUser.username }}</span>
        </v-btn>
        <v-btn
          @click.prevent="logOut"
          text
        >
          <span class="mr-2">Cerrar Sesión</span>
        </v-btn>
      </div>
    </v-app-bar>

    <v-main> 
       <router-view />
    
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',
computed: {
    currentUser() {
      return this.$store.state.auth.user;
    },
    showAdminBoard() {
      if (this.currentUser && this.currentUser.roles) {
        return this.currentUser.roles.includes('ROLE_ADMIN');
      }
      return false;
    },
    showModeratorBoard() {
      if (this.currentUser && this.currentUser.roles) {
        return this.currentUser.roles.includes('ROLE_MODERATOR');
      }

      return false;
    }
  },
  methods: {
    logOut() {
      this.$store.dispatch('auth/logout');
      this.$router.push('/login');
    }
  }
};
</script>
