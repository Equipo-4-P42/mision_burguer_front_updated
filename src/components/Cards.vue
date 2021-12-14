<template>
  <div class="cards-container">
    <div class="menu_inicio">
      <h1 class="h1_description_menu">Productos Estrellas</h1>
      <div class="products_container">
        <div class="item1">
          <div class="card">
            <div class="producto1"></div>
            <h2 class="h2_description_menu">Hamburguesa Máster</h2>
            <ul class="description_title"></ul>
          </div>
        </div>
        <div class="item2">
          <div class="card">
            <div class="producto2"></div>
            <h2 class="h2_description_menu">Hamburguesa de Pollo Crispy</h2>
            <ul class="description_title"></ul>
          </div>
        </div>
        <div class="item3">
          <div class="card">
            <div class="producto3"></div>
            <h2 class="h2_description_menu">Hamburguesa Vegetariana</h2>
            <ul class="description_title"></ul>
          </div>
        </div>
        <div class="item4">
          <div class="card">
            <div class="producto4"></div>
            <h2 class="h2_description_menu">Hamburguesa Campeona</h2>
            <ul class="description_title"></ul>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cards",
};
</script>

<style>
/* INICIO CSS CARD */
.cards-container {
  width: 100%;
  margin-bottom: 5%;
}
.card {
  border-radius: 15px;
  box-shadow: 0px 0px 7px -3px black;
  padding: 10px;
  padding-bottom: 25px;
}

.header-title {
  margin: 0;
  padding: 0;
  width: 30%;
  min-height: 90px;
  background-color: yellow;
  background-image: url("../assets/mision_burguer.png");
  background-size: 55% 150%;
  background-repeat: no-repeat;
  background-color: #ff1616;
  background-position: -5px;
}

.description_title {
  color: black;
  font-family: arial;
  font-size: 12px;
  font-weight: bold;
}

.h1_description_menu {
  color: red;
  font-family: impact;
  font-size: 40px;
  font-weight: bold;
  text-align: center;
  letter-spacing: 5px;
  word-spacing: 8px;
  margin-top: 2%;
}

.h2_description_menu {
  margin-bottom: 0px;
  color: black;
  font-family: impact;
  font-size: 25px;
  font-weight: bold;
  text-align: center;
  letter-spacing: 1px;
  word-spacing: 1px;
}

.h4_description_menu {
  margin: 0px;
  margin-top: 15px;
  margin-bottom: 15px;
  color: red;
  font-family: impact;
  font-size: 25px;
  font-weight: bold;
  text-align: center;
  letter-spacing: 3px;
  word-spacing: 1px;
}

.products_container {
  display: flex;
  flex-direction: row;
  justify-content: center;
  gap: 4%;
  margin-left: 10%;
  margin-right: 10%;
  margin-top: 2%;
}

.item1 {
  display: inline-block;
  justify-content: space-around;
  align-items: center;
}

.producto1 {
  height: 150px;
  width: 100%;
  background-image: url("../assets/producto1.png");
  background-repeat: no-repeat;
  background-position: center;
}

.item2 {
  display: inline-block;
  justify-content: space-around;
  align-items: center;

}

.producto2 {
  height: 150px;
  width: 100%;
  background-image: url("../assets/producto2.png");
  background-repeat: no-repeat;
  background-position: center;
}

.item3 {
  display: inline-block;
  justify-content: space-around;
  align-items: center;

}

.producto3 {
  height: 150px;
  width: 100%;
  background-image: url("../assets/producto3.png");
  background-repeat: no-repeat;
  background-position: center;
}

.item4 {
  display: inline-block;
  justify-content: space-around;
  align-items: center;
}

.producto4 {
  height: 150px;
  width: 100%;
  background-image: url("../assets/producto4.png");
  background-repeat: no-repeat;
  background-position: center;
}



/*// Extra small devices (portrait phones, less than 576px)
// No media query for `xs` since this is the default in Bootstrap*/

/*// Small devices (landscape phones, 576px and up)*/
@media (max-width: 360px) {
  .card{
    margin-bottom: 10%;
  }
  .h1_description_menu{
    margin-top: 10%;
  }
  .buttons{
    
  }

  .products_container {
    flex-direction: column;
    margin:10%;
  }
}
@media (max-width: 576px) {
  .card{
    margin-bottom: 10%;
  }
  .h1_description_menu{
    margin-top: 10%;
  }
  .buttons{
    margin-right: 10px;
    padding-top:2px
  }
.buttons ul li {
  display: flex;
  justify-content: center;
  align-items: center;
  color: black;
  font-size: 8px;
  font-weight: bold;
  font-family: Arial, Helvetica, sans-serif;
  height: 25px;
  width: 60px;
  border: none;
  background-color: white;
  border-radius: 5px;
  cursor: pointer;
}
  .banner{
    height: 90px;
  }

  .products_container {
    flex-direction: column;
    margin:10%;
  }
}

/*// Medium devices (tablets, 768px and up)*/
@media (max-width: 768px) {

}

/*// Large devices (desktops, 992px and up)*/
@media (max-width: 992px) {

}

/*// Extra large devices (large desktops, 1200px and up)*/
@media (max-width: 1200px) {

}
/* FIN CSS CARD */
</style>