<template>
    <div class="container-card">
        <div class="card" v-for="(plato,k) in listPlatos" :key="k">
            <div class="in-card-container">
                <img class="card-img" :src="plato.url_img" alt="img">
                <div class="card-body">
                    <h2 class="h2_description_menu">{{ plato.name_plato }}</h2>
                    <ul class="description_title"> {{ plato.desc_plato }}</ul>
                    <ul> {{ plato.price }} </ul>
                </div>
                <dir class="card-buttons">
                    <button class="big-card-button" v-if="this.enumeration[k]==0" @click="plusButton(k)">Agregar</button>
                    <button class="small-card-button" v-if="!this.enumeration[k]==0" @click="minusButton(k)">-</button>
                    <ul class="card-add-status" v-if="!this.enumeration[k]==0"> {{ this.enumeration[k] }} </ul>
                    <button class="small-card-button" v-if="!this.enumeration[k]==0" @click="plusButton(k)">+</button>
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
                <div class="modal-cards" v-for="(n,k) in enumeration" :key=k>
                    <div v-if="!n==0">
                        <img class="card-img" :src="listPlatos[k].url_img" alt="img">
                    </div>
                    <div class="card-body" v-if="!n==0">
                        <h4> {{ this.listPlatos[k].name_plato }} </h4>
                        <ul> Precio unitario: ${{ this.listPlatos[k].price }} </ul>
                        <h3> ${{ this.listPlatos[k].price * n }} </h3>
                    </div>
                    <div class="card-buttons" v-if="!n==0">
                        <button class="small-card-button" v-if="!this.enumeration[k]==0" @click="minusButton(k)">-</button>
                        <ul class="card-add-status" v-if="!this.enumeration[k]==0"> {{ this.enumeration[k] }} </ul>
                        <button class="small-card-button" v-if="!this.enumeration[k]==0" @click="plusButton(k)">+</button>
                    </div>
                </div>
            </div>
            <div class="modal-footer">
                <h2>Total de la compra: {{ total }}</h2>
                <button @click="closeModal()">Continuar Comprando</button>
                <button @click="processSales()">Comprar</button>
            </div>
        </div>

    </div>

</template>

<script>
import gql from "graphql-tag";

export default {
    name: "Delivery",

    data: function(){
        return {
            enumeration: [],
            total: 0,
            listPlatos: [],
            show_modal: false,
            infoVenta:[],
        }
    },

    methods: {

        plusButton: function (key) {
            this.enumeration[key] += 1;
            this.total += this.listPlatos[key].price;
        },

        minusButton: function (key) {
            this.enumeration[key] -= 1; 
            this.total -= this.listPlatos[key].price;   
        },

        closeModal: function () {
            var modal = document.getElementById("myModal");
            modal.style.display = "none";
        },

        openModal: function () {
            var modal = document.getElementById("myModal");
            modal.style.display = "block";
        },

        enumerationData: function () {
            for (let i=0; i < this.listPlatos.length; i ++) {
                    this.enumeration.push(0);
            }
        },

        infoVentaFillData: function () {
            this.infoVenta = [];
            for (let i=0; i < this.listPlatos.length; i ++) {
                if (this.enumeration[i] != 0){
                    this.infoVenta.push({id:this.listPlatos[i].id,amount:this.enumeration[i]});
                }
            }
        },

        processSales: async function () {
            console.log(1);
            this.infoVentaFillData();
            console.log(2); 
            if (localStorage.getItem("token_access")  === null ||
                localStorage.getItem("token_refresh") === null ) {
                this.$emit("logOut");
                return;
            }
            console.log(3);     
            localStorage.setItem("token_access", "");
            console.log(4);
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
                    console.log(5.1);
                    localStorage.setItem("token_access", result.data.refreshToken.access);
                    //alert("funcionó la autenticación");
                })
                .catch((error) => {
                    console.log(5.2);
                    //alert("Falló la autenticación");
                    this.$emit("logOut");
                    return;
                });

            await this.$apollo
                .mutate({
                    mutation: gql`
                        mutation ($infoVenta: [NewVentaInput!]!) {
                            createNewVenta(infoVenta: $infoVenta) {
                                id
                            }
                        }
                    `,
                    variables:{
                        infoVenta: this.infoVenta,
                    },
                })
                .then((result) => {
                    console.log(6.1);
                    alert("Transacción Realizada de Manera Exitosa");
                })
                .catch((error) => {
                    console.log(6.2)
                    alert("Algo falló");
                });
        }, 
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
                amount
                }
            }
        `,
        }
    },

    created: async function () {
        this.$apollo.queries.listPlatos.refetch().then(()=> {this.enumerationData()});
    }
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
    align-items: center;
}

.big-card-button {
    align-self: stretch;
}

.container-button {
    margin-right: 15%;
    margin-left: 15%;
    margin-bottom: 20px;
}

.card-add-status {
    margin: 0 30px 0 30px;
    padding: 0 0 0 0;
    text-align: center;
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
    display: flex;
    flex-direction: column;
    padding: 2px 16px;
}

.modal-cards {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
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
    width: 60%; /* Could be more or less, depending on screen size */
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