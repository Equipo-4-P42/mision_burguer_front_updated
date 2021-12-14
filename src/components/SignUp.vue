<template>
  <h1 class="h1_description_menu">Registro de Usuario</h1>
  <div class="register-box-container">
    <div class="register-boxx">
      <form v-on:submit.prevent="processSignUp">
        <!--username-->
        <label class="login-label" for="name">Nombre</label>
        <input
          name="name"
          type="text"
          v-model="user.name"
          placeholder="Digite Nombre "
        />
        <!--username-->
        <label class="login-label" for="direction">Dirección</label>
        <input
          name="direction"
          type="text"
          v-model="user.address"
          placeholder="Digite Dirección "
        />
        <!--username-->
        <label class="login-label" for="telephone">Teléfono</label>
        <input
          name="telephone"
          type="text"
          v-model="user.phone"
          placeholder="Digite Teléfono "
        />
        <!--username-->
        <label class="login-label" for="email">Correo Electrónico</label>
        <input
          name="email"
          type="email"
          v-model="user.email"
          placeholder="Digite Correo "
        />
        <!--username-->
        <label class="login-label" for="user">Usuario</label>
        <input
          name="user"
          type="text"
          v-model="user.username"
          placeholder="Digite Usuario "
        />
        <!--password-->
        <label class="password-label" for="password ">Contraseña</label>
        <input
          name="password"
          type="password"
          v-model="user.password"
          placeholder="Digite Contraseña"
        />
        <!--Botón-->
        <input type="submit" value="REGISTRARSE" />
      </form>
    </div>
  </div>
</template>


<script>
import gql from "graphql-tag";

export default {
  name: "SignUp",

  data: function () {
    return {
      user: {
        username: "",
        password: "",
        name: "",
        email: "",
        address: "",
        phone: "",
      },
    };
  },

  methods: {
    processSignUp: async function () {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($userInput: SignUpInput!) {
              signUpUser(userInput: $userInput) {
                refresh
                access
              }
            }
          `,
          variables: {
            userInput: this.user,
          },
        })
        .then((result) => {
          let dataLogIn = {
            username: this.user.username,
            token_access: result.data.signUpUser.access,
            token_refresh: result.data.signUpUser.refresh,
          };

          this.$emit("completedSignUp", dataLogIn);
        })
        .catch((error) => {
          alert("ERROR: Fallo en el registro.");
        });
    },
  },
};
</script>


<style>
/* INICIO CSS REGISTRO */
.register-box-container {
  display: flex;
  justify-content: center;
  width: 100%;
}
.register-boxx {
  display: flex;
  margin-top: 2%;
  margin-bottom: 4%;
  justify-content: center;
  width: 300px;
}

.register-boxx form {
  display: flex;

  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  border-radius: 10px;
  background-color: #9c2713;
}
/* FIN CSS REGISTRO */
</style>