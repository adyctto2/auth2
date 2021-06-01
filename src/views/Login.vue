<template>
<div class="login">
 <v-card
    class="mx-auto align-center"
    max-width="450"
    elevation="9"
  >
    <v-img
      src="../assets/logo.jpg"
      :max-width="width"
      class="logo"
    ></v-img>
    <h2 class="text-center"><strong> Autenticación </strong></h2>
    <form name="form" @submit.prevent="handleLogin">
      <v-card-text >
          <v-text-field
              ref="user.username"
              
              v-model="user.username"
              :rules="[() => !!user.name || 'Este campo es requerido']"
              :error-messages="errorMessages"
              label="Ingrese su usuario"
              placeholder="Ingrese su usuario"
              required
              name="username"
          ></v-text-field>
          <v-text-field
              ref="user.password"
              v-model="user.password"
              :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
              :rules="[() => !!user.password || 'Este campo es requerido']"
              :type="show1 ? 'text' : 'password'"
              name="password"
              label="Ingrese su contraseña"
              hint="8 caracteres como minimo"
              counter
              id="password"
              @click:append="show1 = !show1"
            ></v-text-field>
      </v-card-text>

      <v-card-actions>
        <div class="logo">
          <v-btn
          dark
          color="cyan darken-1"
          :disabled="loading"
        >
          <span v-show="loading" class="spinner-border spinner-border-sm"></span>
          <v-icon left>
            mdi-key
          </v-icon> Iniciar Sesión
        </v-btn>
        </div>

      </v-card-actions>
    </form>
    <!-- <v-card-text>
      <v-alert
        v-model="alert"
        close-text="Close Alert"
        type="error"
        dark
        dismissible
      >
      Error de autenticacion</v-alert>
    </v-card-text> -->
    <br>
    <!-- <p class="tex}t-center">(- * -)</p> -->
    <!-- <h4 class="text-center">Ingresa con</h4>
    <v-card-text elevation="9" >
        <a href="" class="accion" block>
            <v-img class="logo "
            src="../assets/logo_ciudadania_digital.png"
            width="50%"
            ></v-img>
        </a>
    </v-card-text> -->
    <v-card-text>
      <v-row>
        <v-col>
          <a href="" block>¿Se te olvido tu contraseña?</a>

        </v-col>
        <v-col>
          <a href="" block>Solicita ciudadania digital</a>

        </v-col>
      </v-row>

    </v-card-text>
  </v-card>
</div>
</template>

<script>
import User from '../models/user';

export default {
  name: 'Login',
  data: () => ({
      show: false,
      width : '70%',
      show1: false,
      user: new User('', ''),
      loading: false,
      alert : '',
      errorMessages : '',
      rules: {
        required: value => !!value || 'Requerido.',
        min: value => value.length >= 8 || 'Min 8 caracteres',
        emailMatch: () => (`The email and password you entered don't match`),
      },
    }),
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    }
  },
  created() {
    if (this.loggedIn) {
      this.$router.push('/profile');
    }
  },
  methods: {
    handleLogin() {
      this.loading = true;
      this.$validator.validateAll().then(isValid => {
        if (!isValid) {
          this.loading = false;
          return;
        }

        if (this.user.username && this.user.password) {
          this.$store.dispatch('auth/login', this.user).then(
            () => {
              this.$router.push('/profile');
            },
            error => {
              this.loading = false;
              this.errorMessages =
                (error.response && error.response.data && error.response.data.message) ||
                error.message ||
                error.toString();
            }
          );
        }
      });
    }
  }
};
</script>

<style scoped>
.logo{
    display: block; margin: auto;
  }
  .acccion{
    border: 1px solid black imp !important;
  }
  .acccion:hover{
    opacity: 0.5 imp !important;
  }
</style>