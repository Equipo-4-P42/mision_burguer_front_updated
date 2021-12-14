<template>
  <h1 class="h1_description_menu">Iniciar Sesión</h1>
  <div class="login-box-container">
    <div class="login-boxx">
      <form v-on:submit.prevent="processLogInUser">
        <!--username-->
        <label class="login-label" for="username">Usuario</label>
        <input
          name="username"
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
        <input type="submit" value="INGRESAR" />
      </form>
    </div>
  </div>
</template>


<script>
import gql from "graphql-tag";

export default {
  name: "LogIn",

  data: function () {
    return {
      user: {
        username: "",
        password: "",
      },
    };
  },

  methods: {
    processLogInUser: async function () {
      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($credentials: CredentialsInput!) {
              logIn(credentials: $credentials) {
                refresh
                access
              }
            }
          `,
          variables: {
            credentials: this.user,
          },
        })
        .then((result) => {
          let dataLogIn = {
            username: this.user.username,
            token_access: result.data.logIn.access,
            token_refresh: result.data.logIn.refresh,
          };
          console.log(dataLogIn);
          this.$emit("completedLogIn", dataLogIn);
        })
        .catch((error) => {
          alert("ERROR 401: Credenciales Incorrectas.");
        });
    },
  },
};
</script>


<style>
/* INICIO CSS LOGIN */
.login-box-container {
  display: flex;
  justify-content: center;
  width: 100%;
}
.login-boxx {
  display: flex;
  margin-top: 2%;
  margin-bottom: 4%;
  justify-content: center;
  width: 300px;
}

.login-boxx form {
  display: flex;

  flex-direction: column;
  justify-content: center;
  align-items: center;
  width: 100%;
  border-radius: 10px;
  background-color: #9c2713;
}

.password-label {
  margin-top: 15px;
  font-size: 25px;
  color: white;
  margin-bottom: 15px;
}
.login-label{
  font-size: 25px;
  color: white;
  margin-bottom: 15px;
  margin-top: 15px;
}
form input {
  text-align: center;
  height: 40px;
  border: none;
  width: 80%;
  border-radius: 10px;
}
form input[type="submit"] {
  margin-top: 20px;
  margin-bottom: 30px;
  cursor: pointer;
  background-color: white;
  font-weight: bold;
}
form input[type="submit"]:hover {
  background-color: rgb(241, 241, 241);
}
/* FIN CSS LOGIN */
</style>