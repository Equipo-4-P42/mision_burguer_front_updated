<template>
  <div id="Historial">

    <div class="container">
      <h2>
        Titular Cuenta:
        <span>{{ username }}</span>
      </h2>
      <h2>
        Saldo:
        <!-- <span>${{ accountByUsername.balance }} COP</span> -->
      </h2>
      <h2>
        Último Movimiento:
        <span>
          <!-- {{ accountByUsername.lastChange.substring(0, 10) }}  
          {{ accountByUsername.lastChange.substring(11, 19) }} -->
        </span>
      </h2>
    </div>

    <h2>Platos</h2>     
    <div class="container-table">
        <table>
            <tr>
                <th> ID </th>
                <th> PLATO </th>
                <th> DESCRIPCIÓN </th>
                <th> PRECIO $ </th>
                <th> URL </th>
                <!-- <th>Fecha</th>
                <th>Hora</th>
                <th>Origen</th>
                <th>Destino</th>
                <th>Valor</th> -->
            </tr>
            
            
            
            <tr v-for="(plato,k) in listPlatos" :key="k">
                <td>  {{k+1}}   </td>
                <td> {{plato.name_plato}} </td>
                <td> {{plato.desc_plato}} </td>
                <td> ${{plato.price}} </td>
                <td> {{plato.url_img}} </td>
                <!-- BOTONES --> 
                <!-- <button  v-on:click="Editar"> Editar </button>  -->
                <!-- <button v-on:click="updatePlato" class="editar"><img src="https://img.icons8.com/officel/30/000000/edit.png"/></button> -->
                <!--<router-link to="/home"><li v-if="!is_auth"><a href=""> Home</a></li></router-link>-->
                <!-- <button v-on:click="deletePlato" class="borrar"><a th:href="@{/eliminar/plato/} + ${plato.IdPlato}" class="btn btn-outline-danger"><img src="https://img.icons8.com/color/35/000000/delete.png"/></a></button> -->     
            </tr>
            <!-- <tr v-for="transaction in transactionByUsername" :key="transaction.id">
                <td>{{ transaction.date.substring(0, 10) }}</td>
                <td>{{ transaction.date.substring(11, 19) }}</td>
                <td>{{ transaction.usernameOrigin }}</td>
                <td>{{ transaction.usernameDestiny }}</td>
                <td>${{ transaction.value }} COP</td>
            </tr> -->
        </table>
        <button  v-on:click="prueba"> <img src="https://img.icons8.com/officel/30/000000/edit.png"/> </button>
    </div>
  </div>

</template>


<script>
import gql from "graphql-tag";

export default {
  name: "Account",

  data: function () {
    return {
      username: localStorage.getItem("username") || "none",
      // transactionByUsername: [],
      listPlatos:null,
      // accountByUsername: {
      //   balance: "",
      //   lastChange: "",
      // }
      
    };
    
  },

  apollo: {
    listPlatos: {
      query: gql`
        query  {
           listPlatos {
            id
            name_plato
            desc_plato
            price
            url_img

          }
        }
      `,
     
      // variables() {
      //   return {
      //     username: this.username,
      //   };
      // },
      
    }, 
    // transactionByUsername: {
    //   query: gql`
    //     query ($username: String!) {
    //       transactionByUsername(username: $username) {
    //         id
    //         usernameOrigin
    //         usernameDestiny
    //         value
    //         date
    //       }
    //     }
    //   `,
    //   variables() {
    //     return {
    //       username: this.username,
    //     };
    //   },
    // },

    // accountByUsername: {
    //   query: gql`
    //     query ($username: String!) {
    //       accountByUsername(username: $username) {
    //         balance
    //         lastChange
    //       }
    //     }
    //   `,
    //   variables() {
    //     return {
    //       username: this.username,
    //     };
    //   },
    // },
    
  },
  
  created: function () {
    this.$apollo.queries.listPlatos.refetch();
    
    // this.$apollo.queries.transactionByUsername.refetch();
    // this.$apollo.queries.accountByUsername.refetch();
  },
  methods:{
      prueba: function(){
        console.log(this.listPlatos);
      }
  }
  
};

</script>


<style>
#Historial {
  width: 100%;

  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
}

#Historial .container-table{
    width:50%;
    
    max-height: 250px;
    overflow-y: scroll;
    overflow-x: hidden;
}

#Historial table {
  width: 100%;
  border-collapse: collapse;
  border: 1px solid rgba(0, 0, 0, 0.3);
  
}

#Historial table td,
#Historial table th {
  border: 1px solid #ddd;
  padding: 8px;
}

#Historial table tr:nth-child(even) {
  background-color: #f2f2f2;
}

#Historial table tr:hover {
  background-color: #ddd;
}

#Historial table th {
  padding-top: 12px;
  padding-bottom: 12px;
  text-align: left;
  background-color: crimson;
  color: white;
}

#Historial > h2 {
  color: #283747;
  font-size: 25px;
}

#Historial .container {
  padding: 30px;
  border: 3px solid rgba(0, 0, 0, 0.3);
  border-radius: 20px;
  margin: 5% 0 1% 0;
}

#Historial  .container h2 {
  font-size: 25px;
  color: #283747;
}
#Historial .container span {
  color: crimson;
  font-weight: bold;
}
</style>