<template>
  <div id="app" class="app">
    <div class="global_container">
      <div class="section">
        <div class="banner">
          <div class="buttons">
            <ul>
              <li v-if="!is_auth" v-on:click="loadCards"><a href="">Inicio</a></li>
              <li v-if="is_auth" v-on:click="loadHome"><a href="">Inicio</a></li>
              <li v-if="is_auth" v-on:click="loadDish">Menú</li>
              
              <li v-if="!is_auth" v-on:click="loadLogIn">
                <a href="">Iniciar Sesión</a>
              </li>
              <li v-if="!is_auth" v-on:click="loadSignUp">
                <a href="">Registrarse</a>
              </li>
              <li v-if="is_auth" v-on:click="loadDelivery">
                Domicilios
              </li>
              <li v-if="is_auth" v-on:click="logOut">Cerrar Sesión</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="main-component">
        <router-view
          v-on:completedLogIn="completedLogIn"
          v-on:completedSignUp="completedSignUp"
          v-on:logOut="logOut"
        >
        </router-view>
      </div>

      <div class="footer">
        <h2>Todos los derechos reservados @Team4 - P42. Misión Mintic 2022</h2>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: "App",

  computed: {
    is_auth: {
      get: function () {
        return this.$route.meta.requiresAuth;
      },
      set: function () {},
    },
  },

  methods: {
    loadLogIn: function () {
      this.$router.push({ name: "logIn" });
    },

    loadSignUp: function () {
      this.$router.push({ name: "signUp" });
    },

    completedLogIn: function (data) {
      localStorage.setItem("username", data.username);
      localStorage.setItem("token_access", data.token_access);
      localStorage.setItem("token_refresh", data.token_refresh);
      Swal.fire(
                '¡Buen trabajo!',
                '¡Has Iniciado Sesión!',
                'success'
                );
      this.loadHome();
    },

    completedSignUp: function (data) {
      Swal.fire(
                '¡Buen trabajo!',
                '¡Registro Exitoso',
                'success'
                )
      this.completedLogIn(data);
    },

    loadHome: function () {
      this.$router.push({ name: "home" });
    },

    loadDish: function () {
      this.$router.push({ name: "dish" });
    },
    loadDelivery: function () {
      this.$router.push({ name: "delivery" });
    },
        loadCards: function () {
      this.$router.push({ name: "App" });
    },

    // loadTransaction: function(){
    //   this.$router.push({ name: "transaction" });
    // },

    logOut: function () {
      localStorage.clear();
      Swal.fire({
                position: 'top-center',
                icon: 'success',
                title: '¡Has Cerrado Sesión correctamente!',
                showConfirmButton: false,
                timer: 2000
              })
      this.loadLogIn();
    },
  },
};
</script>


<style>
@import "./css/Styles.css";
.global_container {
  max-width: 1920px;
}

/* INICIO CSS HEADER */
.section {
  width: 100%;
  background-color: white;
}
.banner {
  height: 340px;
  max-width: 100%;
  background-image: url("./assets/banner.png");
  background-size: cover;
  position: relative;
}
.buttons {
  float: right;
  display: flex;
  gap: 20px;
  justify-content: flex-end;
  flex-direction: row;
  padding-top: 23px;
  margin-right: 50px;
}
.buttons ul{
  display: flex;
  gap:5%;
  align-items: center;
  justify-content: center;
}
.buttons ul li {
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-size: 15px;
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  height: 50px;
  width: 130px;
  border: none;
  background-color: white;
  border-radius: 10px;
  cursor: pointer;
}
.buttons ul a{
  text-decoration: none;
}
.buttons ul{
  list-style: none;
  
}
.buttons li:hover {
  background-color: rgb(238, 236, 236);
}
/* FIN CSS HEADER */

/* INICIO CSS FOOTER */
.footer {
  display: flex;
  height: 80px;
  width: 100%;
  background-color: #9c2713;
  justify-content: center;
  align-items: center;
}
.footer h2 {
  color: white;
  font-family: arial;
  font-size: 15px;
  font-weight: bold;
  text-align: center;
  letter-spacing: 3px;
  word-spacing: 1px;
  margin: 0px;
}
/* FIN CSS FOOTER */
</style>
