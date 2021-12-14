<template>
  <div id="Historial">
    <h1 class="h1_description_menu">Platos</h1>
    <div class="create-plato_button">
      <button title="Crear" class="crear" id="myBtn" @click="openModalCrear()">
        Crear Plato
      </button>
    </div>

    <div class="container-table">
      <table>
        <tr>
          <th>No.</th>
          <th>PLATO</th>
          <th>DESCRIPCIÓN</th>
          <th>PRECIO</th>
          <th>URL</th>
          <th>Opciones</th>
          <!-- <th>Fecha</th>
            <th>Origen</th>
            <th>Destino</th>
            <th>Valor</th> -->
        </tr>
        <tr v-for="(plato, k) in listPlatos" :key="k">
          <td>{{ k + 1 }}</td>
          <td>{{ plato.name_plato }}</td>
          <td>{{ plato.desc_plato }}</td>
          <td>${{ plato.price }}</td>
          <td>{{ plato.url_img.substring(0, 20) }}</td>
          <!-- BOTONES -->
          <td>
          <button
            title="editar"
            class="editar"
            @click="
              openModalUpdate(
                plato.id,
                plato.name_plato,
                plato.desc_plato,
                plato.price,
                plato.url_img
              )
            "
          >
            <box-icon class="box-icons" type="solid" name="edit"></box-icon>
          </button>

          <button
            title="eliminar"
            class="eliminar"
            @click="processDelete(plato.id)"
          >
            <box-icon class="box-icons" type="solid" name="trash"></box-icon>
          </button>
          </td>
        </tr>
      </table>
      <!-- Modal Create -->
      <div id="myModal" class="modal">
        <div class="modal-content2">
          <div class="modal-header">
            <span class="close" @click="closeModal()">&times;</span>
            <h2>Crear Plato</h2>
          </div>
          <div class="modal-body">
            <form v-on:submit.prevent="processNewPlato">
              <input
                type="text"
                v-model="createPlato.name_plato"
                placeholder="Nombre del Plato"
                required
              />
              <br />
              <input
                type="text"
                v-model="createPlato.desc_plato"
                placeholder="Descripción"
                required
              />
              <br />
              <input
                type="number"
                v-model="createPlato.price"
                placeholder="Precio"
                required
              />
              <br />
              <input
                type="text"
                v-model="createPlato.url_img"
                placeholder="Url"
                required
              />
              <br />
              <div class="modal-footer">
                <button type="submit">Crear</button>
                <button @click="closeModal()">Cancelar</button>
              </div>
            </form>
          </div>
        </div>
      </div>

      <!-- Modal Update -->
      <div id="modalUpdate" class="modal">
        <div class="modal-content">
          <div class="modal-header">
            <span class="close" @click="closeModal2()">&times;</span>
            <h2>Actualizar Plato</h2>
          </div>
          <div class="modal-body">
            <form v-on:submit.prevent="processUpdate">
              <label for="name_plato">Nombre del Plato</label>
              <input
                type="text"
                id="name_plato"
                v-model="createPlato.name_plato"
                placeholder="Digitar Nombre del Plato"
              />
              <br />
              <label for="desc_plato">Descripción</label>
              <input
                type="text"
                id="desc_plato"
                v-model="createPlato.desc_plato"
                placeholder="Digitar una breve Descripción"
              />
              <br />
              <label for="price">Precio</label>
              <input
                type="number"
                id="price"
                v-model="createPlato.price"
                placeholder="Precio"
              />
              <br />
              <label for="url_img">Url</label>
              <input
                type="text"
                id="url_img"
                v-model="createPlato.url_img"
                placeholder="Url"
              />
              <br />
              <div class="modal-footer">
                <button type="submit">Actualizar</button>
                <button @click="closeModal2()">Cancelar</button>
              </div>
            </form>
          </div>
        </div>
      </div>
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
      listPlatos: null,
      createPlato: {
        name_plato: "",
        desc_plato: "",
        price: "",
        url_img: "",
      },
      updatePlatoByIdId: {
        id: "",
      },
    };
  },

  apollo: {
    listPlatos: {
      query: gql`
        query {
          listPlatos {
            id
            name_plato
            desc_plato
            price
            url_img
          }
        }
      `,
    },
  },

  created: function () {
    this.$apollo.queries.listPlatos.refetch();
  },

  methods: {
    closeModal() {
      var modal = document.getElementById("myModal");
      modal.style.display = "none";
    },
    closeModal2() {
      var modalu = document.getElementById("modalUpdate");
      modalu.style.display = "none";
    },
    openModalUpdate(idd, name_plato, desc_plato, price, url_img) {
      this.updatePlatoByIdId.id = idd;
      document.getElementById("name_plato").value = name_plato;
      document.getElementById("desc_plato").value = desc_plato;
      document.getElementById("price").value = price;
      document.getElementById("url_img").value = url_img;
      var modalu = document.getElementById("modalUpdate");
      modalu.style.display = "block";
    },
    openModalCrear() {
      var modal = document.getElementById("myModal");
      modal.style.display = "block";
    },

    //CREATE

    processNewPlato: async function () {
      if (
        localStorage.getItem("token_access") === null ||
        localStorage.getItem("token_refresh") === null
      ) {
        this.$emit("logOut");
        return;
      }

      localStorage.setItem("token_access", "");

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($refresh: String!) {
              refreshToken(refresh: $refresh) {
                access
              }
            }
          `,
          variables: {
            refresh: localStorage.getItem("token_refresh"),
          },
        })
        .then((result) => {
          localStorage.setItem("token_access", result.data.refreshToken.access);
        })
        .catch((error) => {
          this.$emit("logOut");
          return;
        });

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($newPlato: PlatoInput!) {
              createPlato(newPlato: $newPlato) {
                name_plato
                desc_plato
                price
                url_img
              }
            }
          `,
          variables: {
            newPlato: this.createPlato,
          },
        })
        .then((result) => {
          Swal.fire({
            position: "top-center",
            icon: "success",
            title: "¡Plato Creado Exitosamente!",
            showConfirmButton: false,
            timer: 2000,
          });
        })
        .catch((error) => {
          Swal.fire({
            icon: "error",
            title: "Oops...",
            text: "Ocurrió un error, no se pudo crear Plato!",
          });
        });
    },

    //UPDATE
    processUpdate: async function () {
      if (
        localStorage.getItem("token_access") === null ||
        localStorage.getItem("token_refresh") === null
      ) {
        this.$emit("logOut");
        return;
      }

      localStorage.setItem("token_access", "");

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($refresh: String!) {
              refreshToken(refresh: $refresh) {
                access
              }
            }
          `,
          variables: {
            refresh: localStorage.getItem("token_refresh"),
          },
        })
        .then((result) => {
          localStorage.setItem("token_access", result.data.refreshToken.access);
        })
        .catch((error) => {
          this.$emit("logOut");
          return;
        });

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation (
              $newPlato: PlatoInput!
              $updatePlatoByIdId: PlatoIdInput!
            ) {
              updatePlatoById(newPlato: $newPlato, id: $updatePlatoByIdId) {
                id
                name_plato
                desc_plato
                price
                url_img
              }
            }
          `,
          variables: {
            newPlato: this.createPlato,
            updatePlatoByIdId: this.updatePlatoByIdId,
          },
        })
        .then((result) => {
          Swal.fire({
            position: "top-center",
            icon: "success",
            title: "¡Se ha actualizado el Plato correctamente!",
            showConfirmButton: false,
            timer: 2000,
          });
        })
        .catch((error) => {
          Swal.fire({
            icon: "error",
            title: "Oops...",
            text: "Ocurrió un error, no se pudo actualizar Plato!",
          });
        });
    },
    //Delete
    processDelete: async function (idplato) {
      if (
        localStorage.getItem("token_access") === null ||
        localStorage.getItem("token_refresh") === null
      ) {
        this.$emit("logOut");
        return;
      }

      localStorage.setItem("token_access", "");

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($refresh: String!) {
              refreshToken(refresh: $refresh) {
                access
              }
            }
          `,
          variables: {
            refresh: localStorage.getItem("token_refresh"),
          },
        })
        .then((result) => {
          localStorage.setItem("token_access", result.data.refreshToken.access);
        })
        .catch((error) => {
          this.$emit("logOut");
          return;
        });

      await this.$apollo
        .mutate({
          mutation: gql`
            mutation ($deletePlatoByIdId: PlatoIdInput!) {
              deletePlatoById(id: $deletePlatoByIdId)
            }
          `,
          variables: {
            deletePlatoByIdId: {
              id: idplato,
            },
          },
        })
        .then((result) => {
          Swal.fire({
            position: "top-center",
            icon: "success",
            title: "¡Se ha eliminado el Plato correctamente!",
            showConfirmButton: false,
            timer: 2000,
          });
        })
        .catch((error) => {
          Swal.fire({
            icon: "error",
            title: "Oops...",
            text: "Ocurrió un error, no se pudo eliminar Plato!",
          });
        });
    },
  },
};
</script>


<style>
/* 
The Modal (background) 
*/
.modal {
  display: none; /* Hidden by default */
  position: fixed; /* Stay in place */
  z-index: 1; /* Sit on top */
  left: 0;
  top: 0;
  width: 100%; /* Full width */
  height: 100%; /* Full height */
  overflow: hidden; /* Enable scroll if needed */
  background-color: rgb(0, 0, 0); /* Fallback color */
  background-color: rgba(0, 0, 0, 0.4); /* Black w/ opacity */
}

/* Modal Header */
.modal-header {
  display: flex;
  background-color: #9c2713;
  height: 50px;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  flex-wrap: nowrap;
  align-content: center;
  justify-content: space-between;
  align-items: center;
  padding: 6px 16px;
  color: white;
}

/* Modal Body */
.modal-body {
  padding: 2px 16px;
}
.modal-body form {
  display: flex;
  flex-direction: column;
  /* flex-wrap: nowrap; */
  margin-top: 10px;
  align-content: center;
  justify-content: center;
  align-items: center;
}
form label {
  font-size: 15px;
  font-weight: bold;
  color: black;
}
.modal-body input {
  border: 1px solid gray;
}
/* Modal Footer */
.modal-footer {
  padding: 2px 16px;
  color: white;
  display: flex;
  gap: 10%;
}
.modal-footer button {
  width: 130px;
  height: 40px;
  border: none;
  background-color: rgb(180, 5, 5);
  border-radius: 10px;
  color: white;
  cursor: pointer;
  font-size: 15px;
  font-weight: bold;
  margin-bottom: 15px;
}
.modal-footer button:hover {
  background-color: rgb(141, 4, 4);
}
.modal-footer button[type="submit"] {
  background-color: rgb(66, 66, 248);
}
.modal-footer button[type="submit"]:hover {
  background-color: rgb(49, 49, 243);
}
/* Modal Content/Box */
.modal-content {
  background-color: #fefefe;
  margin: 10% auto; /* 15% from the top and centered */
  /* padding: 20px; */
  border-radius: 10px;
  width: 50%; /* Could be more or less, depending on screen size */
  position: relative;
  /* margin: auto; */
  padding: 0;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  animation-name: animatetop;
  animation-duration: 0.4s;
}

.modal-content2 {
  background-color: #fefefe;
  margin: 10% auto; /* 15% from the top and centered */
  /* padding: 20px; */
  border-radius: 10px;
  width: 50%; /* Could be more or less, depending on screen size */
  position: relative;
  /* margin: auto; */
  padding: 0;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  animation-name: animatetop;
  animation-duration: 0.4s;
}
/* Add Animation */
@keyframes animatetop {
  from {
    top: -300px;
    opacity: 0;
  }
  to {
    top: 0;
    opacity: 1;
  }
}

/* The Close Button */
.close {
  color: #aaa;
  float: right;
  font-size: 28px;
  font-weight: bold;
  order: 2;
}

.close:hover,
.close:focus {
  color: black;
  text-decoration: none;
  cursor: pointer;
}

#Historial {
  width: 100%;

  display: flex;
  justify-content: flex-start;
  align-items: center;
  flex-direction: column;
}

#Historial .container-table {
  width: 80%;
  margin-bottom: 160px;
  max-height: 400px;
  overflow-y: auto;
}

#Historial table {
  width: 100%;
  border-collapse: collapse;
  text-align: center;
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
  background-color: #9c2713;
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

#Historial .container h2 {
  font-size: 25px;
  color: #283747;
}
#Historial .container span {
  color: crimson;
  font-weight: bold;
}
.create-plato_button {
  width: 80%;
  margin-top: 70px;
  margin-bottom: 20px;
}
.create-plato_button button {
  width: 130px;
  height: 50px;
  border: none;
  background-color: #43c300;
  color: white;
  font-weight: bold;
  font-size: 15px;
  font-family: Arial, Helvetica, sans-serif;
  border-radius: 10px;
  cursor: pointer;
}
.create-plato_button button:hover {
  background-color: #43b803;
}
.box-icons {
  cursor: pointer;
}

@media (max-width: 360px) {
  .modal-footer {
    flex-direction: column;
  }

  @media (max-width: 411px) {
    .modal-footer {
      flex-direction: column;
    }
  }
}
</style>