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
            <div v-if="total==0">
                <h1>Agregue Productos al Carrito</h1>    
            </div> 
            <div v-if="total!=0" class="modal-body">
                <div class="modal-cards" v-for="(n,k) in enumeration" :key=k>
                    <!-- <div V-if="">X</div> -->
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
                <button @click="closeModal()">Seguir Comprando</button>
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
            
            this.infoVentaFillData();
            
            if (localStorage.getItem("token_access")  === null ||
                localStorage.getItem("token_refresh") === null ) {
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
                    //alert("funcionó la autenticación");
                })
                .catch((error) => {
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
                    alert("Transacción Realizada de Manera Exitosa");
                })
                .catch((error) => {
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




.modal {
    display: none;  
    position: fixed; 
    z-index: 1; 
    left: 0;
    top: 0;
    width: 100%; 
    height: 100%;
    overflow: auto; 
    background-color: rgb(0,0,0); 
    background-color: rgba(0,0,0,0.4); 
    
}

/* Modal Header 
.modal-header {
    padding: 6px 16px;
    color: white;
}
*/

/* Modal Body 
.modal-body {
    display: flex;
    flex-direction: column;
    padding: 2px 16px;
    
}
*/
/*
.modal-cards {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
}
*/
/* Modal Footer 
.modal-footer {
    padding: 2px 16px;
    color: white;
}

*/
/* Modal Content/Box 
    background-color: #9C2713;
    margin: 10% auto; 
    /* padding: 20px; 
    border: none;
    width: 60%; 
    height:60%;
    position: relative;
    border-radius: 10px; 
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
    color: white;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close:hover,
.close:focus {
    color: rgb(226, 226, 226);
    text-decoration: none;
    cursor: pointer;
}

</style>