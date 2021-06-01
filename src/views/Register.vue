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
    <h2 class="text-center"><strong> Crear Cuenta </strong></h2>
    <form name="form" @submit.prevent="handleRegister">
      <div v-if="!successful">
      
      </div>
      <v-card-text >
          <v-text-field
              ref="user.username"
              
              v-model="user.username"
              :rules="[() => !!user.username || 'Este campo es requerido']"
              :error-messages="errorMessages"
              label="Ingrese su usuario"
              placeholder="Ingrese su usuario"
              required
              name="username"
          ></v-text-field>
          
          <div
            v-if="submitted && errors.has('username')"
            class="alert-danger"
          >{{errors.first('username')}}</div>

          <v-text-field
              ref="user.email"
              
              v-model="user.email"
              :rules="[() => !!user.email || 'Este campo es requerido']"
              label="Ingrese su Correo Electronico"
              placeholder="Ingrese su Correo Electronico"
              required
              name="email"
              type="email"
          ></v-text-field>
          
          <div
            v-if="submitted && errors.has('email')"
            class="alert-danger"
          >{{errors.first('email')}}</div>


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
          </v-icon> Registrarse
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
  </v-card>
</div>
</template>

<script>
import User from '../models/user';

export default {
  name: 'Register',
  data() {
    return {
      user: new User('', '', ''),
      submitted: false,
      successful: false,
      message: '',
      loading: false,
      width : '70%',
      show :false,
      show1: false,
      errorMessages : '',
    };
  },
  computed: {
    loggedIn() {
      return this.$store.state.auth.status.loggedIn;
    }
  },
  mounted() {
    if (this.loggedIn) {
      this.$router.push('/profile');
    }
  },
  methods: {
    handleRegister() {
      this.message = '';
      this.submitted = true;
      this.$validator.validate().then(isValid => {
        if (isValid) {
          this.$store.dispatch('auth/register', this.user).then(
            data => {
              this.message = data.message;
              this.successful = true;
            },
            error => {
              this.message =
                (error.response && error.response.data && error.response.data.message) ||
                error.message ||
                error.toString();
              this.successful = false;
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