<template>
  <div>
  
    <!--<div>
      Burgers
      <Burger v-for="burger in burgers"
              v-bind:burger="burger" 
              v-bind:key="burger.name"/>
    </div>

    <div id="map" v-on:click="addOrder">
      click here - eller inte
    </div>-->

    
    <div class="header">
        <h1 id="headtext">Joels Burger Online</h1>
        <img id="headpic" src="https://emvwr2994ad.exactdn.com/wp-content/uploads/2014/11/prime-burger-hamburgare-stockholm.jpg?strip=all&lossy=1&quality=77&ssl=1">
    </div>

   
    <div id="burger_list">
      <h2>Select burger(s)</h2>
      <p>This is where you select your burger</p>        
       
      <div id="wrapper">
          <div id="box" v-for="burger of burgers" :key="burger.name">
              <Burger :burger="burger" />
          </div>
      </div> 
    </div>

    <p></p>   
         <section id="contact">
            <form>
               <h2>Customer Information</h2>
               <p>This is where you provide neccesary information</p>
               <h3>Delivery information</h3>
               <p>
                <label for="name">Full Name</label><br>
                <input type="text" id="name" v-model="fn" required="required" placeholder="Full name">
               </p>
               <p>
                  <label for="email">E-mail</label><br>
                  <input type="email" id="email" name="em" required="required" placeholder="E-mail address">
               </p>
               <p>
                  <label for="street">Street</label><br>
                  <input type="text" id="email" name="st" required="required" placeholder="Street">
               </p>
               <p>
                  <label for="number">House</label><br>
                  <input type="number" id="house" name="no" required="required" placeholder="House">
               </p>
               <p>
                  <label for="payments">Payment options</label><br>
                  <select id="payments" name="po">
                      <option>MasterCard</option>
                      <option>Visa</option>
                      <option>American Express</option>
                      <option>Invoice</option>
                  </select>
            
               <p>
                  <label for ="gender">Gender</label><br>
                  <input type="radio" id="male" name="gender" value="male">
                  <label for="male">Male</label>
               </p>
               <p>
                  <input type="radio" id="female" name="gender" value="female">
                  <label for="female">Female</label>
               </p>
               <p>
                  <input type="radio" id="donotknow" name="gender" value="donotknow">
                  <label for="donotknow">I'm not sure</label>
               </p>
               <p>
                  <button type="submit">
                     Send Info
                  </button>
               </p>
            </form>
         </section>

  </div>
</template>

<script>
import Burger from '../components/OneBurger.vue';
import menu from "../assets/menu.json"
import io from 'socket.io-client'
const socket = io();

export default {
  name: 'HomeView',
  components: {
    Burger
  },

  data: function () {
    return {
      burgers: menu
    }    

  },

  mounted() {
    this.fetchBurgers();
  },

  methods: {
    fetchBurgers() {
    // Hämta innehållet i json-filen och spara i burgers-arrayen
    },
    
    getOrderNumber: function () {
      return Math.floor(Math.random()*100000);
    },
    addOrder: function (event) {
      var offset = {x: event.currentTarget.getBoundingClientRect().left,
                    y: event.currentTarget.getBoundingClientRect().top};
      socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                details: { x: event.clientX - 10 - offset.x,
                                           y: event.clientY - 10 - offset.y },
                                orderItems: ["Beans", "Curry"]
                              }
                 );
    }
  }

  // displayOrderInfo() {
  //  booleanDisplay = true;
  // }
}
</script>

<style>
body {
    font-family: Arial;
 }

#map {
    width: 300px;
    height: 300px;
    background-color: red;
  }

.header {
    height: 200px;
    overflow: hidden;
 }

.allergi {
    font-weight: bold;
    color: blue;
 }

#wrapper {
    grid-template-columns: 3;
    padding: 10px;
}

#box {
    column-width: 30%;
    padding: 10px;
    display: inline-block;
}

#burger_list {
    background-color: black;
    width: 750px;
    color: white;
    padding: 20px;
    border: solid black;
    border-style: dashed;
 }

#contact {
    background-color: white;
    width: 750px;
    padding: 20px;
    border: solid black;
    border-style: dashed;
 }

 #headpic {
    opacity: 0.5;
    width: 790px;
 }

 #headtext {
    position: absolute;
    margin-top: 50px;
    margin-left: 50px;
 }

 button:hover {
    cursor: pointer;
    color: green;
    border: solid black;
    border-style: dashed;
 }

</style>