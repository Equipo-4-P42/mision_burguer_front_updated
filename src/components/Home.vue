<template>
  <h1 class="h1_description_menu">Mi Perfil</h1>
  
  <div class="information">
    <div class="lol"><h1 class="h1_description_menu">Información Personal:</h1></div>
    
    <div class="details">
      <h2>
        Nombre:
        <span>{{ userDetailById.name }}</span>
      </h2>
      <h2>
        Nombre de usuario:
        <span>{{ userDetailById.username }}</span>
      </h2>
      <h2>
        Correo electrónico:
        <span>{{ userDetailById.email }}</span>
      </h2>
      <h2>
        Dirección:
        <span>{{ userDetailById.address }}</span>
      </h2>
    </div>
  </div>
  
</template>


<script>
import gql from "graphql-tag";
import jwt_decode from "jwt-decode";

export default {
  name: "Home",

  data: function () {
    return {
      userId: jwt_decode(localStorage.getItem("token_refresh")).user_id,
      userDetailById: {
        username: "",
        name: "",
        address:"",
        phone:"",
        email: "",
      },
    };
  },

  apollo: {
    userDetailById: {
      query: gql`
        query ($userId: Int!) {
          userDetailById(userId: $userId) {
            username
            name
            email
            phone
            address
          }
        }
      `,
      variables() {
        return {
          userId: this.userId,
        };
      }
    },
  }
};
</script>


<style>
.information{
  display: flex;
  justify-content: center;
  align-items: center;
  gap:5%;
  margin-bottom: 160px;
}
.details{
background-color: #9C2713;
border-radius: 10px;
margin-top: 80px;
padding:15px;
box-shadow: 6px 6px 6px #888888;
}
.details h2{
  font-size: 30px;
  padding: 15px;
  color:white;
}
.lol{

}
.lol h1{
  font-weight: bold;
  font-size: 40px;
}


@media (max-width: 360px) {
  .information{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap:5%;
  margin-bottom: 160px;
}
.details h2{
  font-size: 20px;
  padding: 15px;
  color:white;
}
.details{
width: 70%;
}
}
@media (max-width: 576px) {
  .information{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap:5%;
  margin-bottom: 160px;
}
.details{
width: 70%;
}
.details h2{
  font-size: 20px;
  padding: 15px;
  color:white;
}
}

@media (max-width: 768px) {
  .information{
  margin-right: 10%;
  margin-left: 10%;
}

}
@media (max-width: 1024px) {
  .information{
  margin-right: 10%;
  margin-left: 10%;
}

}
</style>
