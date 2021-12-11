<template>

  <div id="app" class="app">

    <div class="header">

      <h1>  MisiónBurguer </h1>
      <nav>
        
        <button v-if="is_auth" v-on:click="loadHome"> Inicio </button>
        <button v-if="is_auth" v-on:click="loadAccount"> Cuenta </button>
        <button v-if="is_auth" v-on:click="loadTransaction"> Transacción </button>
        <button v-if="is_auth" v-on:click="logOut"> Cerrar Sesión </button>
        <button v-if="!is_auth" v-on:click="loadLogIn" > Iniciar Sesión </button>
        <button v-if="!is_auth" v-on:click="loadSignUp" > Registrarse </button>
        <button v-if="is_auth" v-on:click="loadDelivery" > Domicilios </button>
      </nav>
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

</template>


<script>
export default {
  name: 'App',

  computed: {
    is_auth: {
      get: function() {
        return this.$route.meta.requiresAuth;
      },
      set: function() { }
    }
  },

  methods:{
    

    loadLogIn: function(){
      this.$router.push({name: "logIn"})
    },

    loadSignUp: function(){
      this.$router.push({name: "signUp"})
    },

    completedLogIn: function(data) {
			localStorage.setItem("username", data.username);
			localStorage.setItem("token_access", data.token_access);
			localStorage.setItem("token_refresh", data.token_refresh);
			alert("Autenticación Exitosa");
			this.loadHome();
    },

    completedSignUp: function(data) {
			alert("Registro Exitoso");
			this.completedLogIn(data);
    },

    loadHome: function() {
      this.$router.push({ name: "home" });
    },

    loadAccount: function () {
			this.$router.push({ name: "account" });
      
		},
    loadDelivery: function(){
      this.$router.push({name: "delivery"})
    },

    // loadTransaction: function(){
    //   this.$router.push({ name: "transaction" });
    // },

    logOut: function () {
			localStorage.clear();
			alert("Sesión Cerrada");
      this.loadLogIn();
		},
  }
}
</script>


<style>
  /* .section {
  width: 100%;
  background-color: white;
}
.banner {
  height: 340px;
  max-width: 100%;
  background-image: url("/assets/banner.png");
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
  width: 100px;
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
} */




  body{
    margin: 0 0 0 0;
  }

  .header{
    margin: 0%;
    padding: 0;
    width: 100%;
    height: 10vh; 
    min-height: 100px;

    background-color: #da0c0c ;
    /* background-image: url("/assets/banner.png"); */
    color:#E5E7E9  ;

    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .header h1{
    width: 20%;
    text-align: center;
  }

  .header nav {
    height: 100%;
    width: 30%;

    display: flex;
    justify-content: space-around;
    align-items: center;

    font-size: 20px;
  }

  .header nav button{
    color: #E5E7E9;
    background: #283747;
    border: 1px solid #E5E7E9;

    border-radius: 5px;
    padding: 10px 20px;
  }

  .header nav button:hover{
    color: #283747;
    background: #E5E7E9;
    border: 1px solid #E5E7E9;
  }

  
  .main-component{
    height: 75vh;
    margin: 0%;
    padding: 0%;

    background: #FDFEFE ;
  }

 
  .footer{
    margin: 0;
    padding: 0;
    width: 100%;
    height: 10vh;
    min-height: 100px; 

    background-color: #cc0c0c;
    color: #E5E7E9;

  }

  .footer h2{
    width: 100%;
    height: 100%;
    
    display: flex;
    justify-content: center;
    align-items: center;
  }

</style>
