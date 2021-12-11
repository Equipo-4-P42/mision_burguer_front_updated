<template>
    <div class="container-card">
        <div class="card" v-for="(dish,k) in dishes_data" :key="k">
            <div class="in-card-container">
                <img class="card-img" :src="dish.url" alt="img">
                <div class="card-body">
                    <h2 class="h2_description_menu">{{ dish.name }}</h2>
                    <ul class="description_title"> Texto Texto Texto Texto</ul>
                     <ul> $$$$$ </ul>
                </div>
                <dir class="card-buttons">
                    <button class="big-card-button" v-if="dish.amount==0" @click="plusButton(k)">Agregar</button>
                    <button class="small-card-button" v-if="!dish.amount==0" @click="minusButton(k)">-</button>
                    <ul class="card-add-status" v-if="!dish.amount==0"> {{ dish.amount }} </ul>
                    <button class="small-card-button" v-if="!dish.amount==0" @click="plusButton(k)">+</button>
                </dir>
            </div>
        </div>
    </div>
    <div class="container-button">
        <button class="float" id="myBtn" @click="openModal()">
        </button>

    </div>

    <div id="myModal" class="modal">

        <!-- Modal content -->
        <div class="modal-content">
            <div class="modal-header">
                <span class="close" @click="closeModal()">&times;</span>
                <h2>Carrito de Compras</h2>
            </div>
            <div class="modal-body">
                <p>Some text in the Modal Body</p>
                <p>Some other text...</p>
            </div>
            <div class="modal-footer">
                <button @click="closeModal()">Continuar Comprando</button>
                <button>Comprar</button>
            </div>
        </div>

    </div>

</template>

<script>

export default {
  name: "Delivery",

  data: function(){
      return {
          dishes_data: [{
              name: "hamburguesa",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "perro",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "hamburguesa",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "perro",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "hamburguesa",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "perro",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "hamburguesa",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "perro",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "hamburguesa",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          },
          {
              name: "perro",
              amount: 0,
              url: "https://media.istockphoto.com/photos/juicy-hamburger-on-white-background-picture-id1206323282?k=20&m=1206323282&s=612x612&w=0&h=yatlq6BHRCCvoTzFZLSwaJc0O8Quct_tRPWtH0dj9Fc="
          }],
          show_modal: false,
      }
  },

  methods: {
    plusButton: function (key) {
        this.dishes_data[key].amount += 1;
    },
    minusButton: function (key) {
        this.dishes_data[key].amount -= 1;    
    },
    closeModal() {
        var modal = document.getElementById("myModal");
        modal.style.display = "none";
    },
    openModal() {
        var modal = document.getElementById("myModal");
        modal.style.display = "block";
    }
},
};

</script>

<style>

.container-card {
    margin-bottom: 35px;
    display: flex;
    flex-direction: row;
    flex-wrap: wrap;
    justify-content: flex-start;
    align-items: stretch;
    align-content: space-around;
    row-gap: 22px;
    column-gap: 4%;
    margin-left: 10%;
    margin-right: 10%;
    margin-top: 2%;
}

.card {
    flex-grow: 0;
    flex-shrink: 0;
    flex-basis: 200px;
    display: inline-block;
    justify-content: space-around;
}

.in-card-container {
    max-width: 200px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    align-items: baseline;
    flex-wrap: wrap;
    align-items: stretch;
}

img {
    width: 200px;
}

.card-buttons {
    margin: 7px 16px;
    padding: 0 0 0 0;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-content: stretch;
}

.big-card-button {
    align-self: stretch;
}

.container-button {
    margin-right: 15%;
    margin-left: 15%;
    margin-bottom: 20px;
}

/*
Floating button
*/
.float{
	position:fixed;
	width:60px;
	height:60px;
	bottom:40px;
	right:40px;
	background-color:#9c2713;
	color:#FFF;
	border-radius:50px;
	text-align:center;
}

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
    overflow: auto; /* Enable scroll if needed */
    background-color: rgb(0,0,0); /* Fallback color */
    background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
}

/* Modal Header */
.modal-header {
    padding: 6px 16px;
    background-color: #9c2713;
    color: white;
}

/* Modal Body */
.modal-body {
    padding: 2px 16px;
}

/* Modal Footer */
.modal-footer {
    padding: 2px 16px;
    background-color: #9c2713;
    color: white;
}


/* Modal Content/Box */
.modal-content {
    background-color: #fefefe;
    margin: 15% auto; /* 15% from the top and centered */
    /* padding: 20px; */
    border: 1px solid #888;
    width: 80%; /* Could be more or less, depending on screen size */
    position: relative;
    /* margin: auto; */
    padding: 0;
    box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2),0 6px 20px 0 rgba(0,0,0,0.19);
    animation-name: animatetop;
    animation-duration: 0.4s
}

/* Add Animation */
@keyframes animatetop {
  from {top: -300px; opacity: 0}
  to {top: 0; opacity: 1}
}

/* The Close Button */
.close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}

</style>