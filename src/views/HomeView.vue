<template>
  <div>
              
    <header> 
      <div class="header">
        <h1 id="headtext">Joels Burger Online</h1>
        <img id="headpic" src="https://emvwr2994ad.exactdn.com/wp-content/uploads/2014/11/prime-burger-hamburgare-stockholm.jpg?strip=all&lossy=1&quality=77&ssl=1">
      </div>
    </header>

      <section id="burger_list" >
         <h1 class="burtext">
            Select your burger
         </h1>
         <div class="burtext"> 
            This is where you select your burger
         </div>

         <div class="wrapper burre">
            <Burger v-for="burger in burgers"
               v-bind:burger="burger" 
               v-bind:key="burger.name"
               v-on:orderedBurgers="addToOrder($event)"/>
         </div>
      </section>


      <section id="contact">
         <div class="ctext">
            <h2>Who are you?</h2>  
            <div>
               Please, add some information below:
            </div>  
          
               <form>
                    <p>
                        <label for="name">Name</label><br>
                        <input type="text" id="name" v-model="n" required="required" placeholder="Namn">
                    </p>
                    <p>
                        <label for="email">E-mail</label><br>
                        <input type="email" id="email" v-model="em" required="required" placeholder="E-mail address">
                    </p>
               
                    <p>
                        <label for="betalningsmetod">Payment Options</label> <br> <!--fanns <be> innan br men funkade ej -->
                        <select id="payment" v-model="pay" >
                            <option>MasterCard</option>
                            <option selected="Kort">Visa</option>
                            <option>Klarna</option>
                            <option>Swish</option>
                            <option>Invoice</option>
                        </select>
                     </p>

               <h3>Gender?</h3>  
                     <input type="radio" id="kvinna" v-model="gender" value="kvinna">
                     <label for="kvinna">Female</label><br>
                     <input type="radio" id="man" v-model="gender" value="man">
                     <label for="man">Man</label><br>
                     <input type="radio" id="vea" v-model="gender" value="vill ej ange" checked="checked">
                     <label for="vill ej ange">I don't know</label>
                </form>
            </div>
            
            <div class="ctext">
            <h3>Where are you?</h3>
            <div id="map" v-on:click="setLocation">
                <div id="dot" :style="{left: location.x + 'px',top: location.y + 'px'}">
                </div>
            </div>
            </div>
            
        </section> 
            <br>


         <section id="submit">
            <button type="submit" v-on:click="addOrder" >
               <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRzBZShxkwLuCWvEWCEnu8XD9-uU8XLbx-c4Q&usqp=CAU" style="width:20px;height:20px;">
               Beställ
            </button> 
         </section>             
      
    <hr>
    <footer>
        &#169; Joel Copy Written
    </footer>

    </div>
</template>

<script>
  import Burger from '../components/OneBurger.vue'
  import io from 'socket.io-client'
  import menu from '../assets/menu.json'
  const socket = io();


  export default {
   name: 'HomeView',
   components: {
      Burger
   },
   
   data: function () {
      return {
        burgers: menu,
        n:"",
        em:"",
        //ad:"",
        //nr:"",
        pay:"",
        gender:"",
        orderedBurgers:{},
        location: { x: 0, y: 0}
      }
    },

    methods: {
      printInformation: function(){
        console.log(this.n, this.em,/*this.ad, this.nr,*/this.pay,this.gender)
        //console.log(this.orderedBurgers)
      },

      getOrderNumber: function () {
        return Math.floor(Math.random()*100000);
      },
      
      addOrder: function () {
        socket.emit("addOrder", { orderId: this.getOrderNumber(),
                                  details: { x: this.location.x,
                                            y: this.location.y },
                                  orderItems: this.orderedBurgers,
                                  personalInfo: {name: this.n, email: this.em, pay:this.pay, gender:this.gender} 
                                }
                  );
        this.printInformation()
      },
      
      setLocation: function (event) {
        var offset = {x: event.currentTarget.getBoundingClientRect().left,
                      y: event.currentTarget.getBoundingClientRect().top};
        this.location.x = event.clientX - 10 - offset.x
        this.location.y = event.clientY - 10 - offset.y
      },
      
      addToOrder:function(event){
        this.orderedBurgers[event.name]=event.amount;
        
      }
    }
  }
</script>

<style>

@import url('https://fonts.cdnfonts.com/css/spongebob-font-condensed');

  #map {
    width: 700px;
    height: 400px;
    position: relative;
    background: url(../../public/img/polacks.jpg);
    cursor: crosshair;
    margin: 0;
    padding: 0;
  }
  
  #dot {
    position: absolute;
    background: red;
    color: white;
    border-radius: 10px;
    width:20px;
    height:20px;
    text-align: center;
  }

  body {
    font-family: "Droid Serif", sans-serif;
    background-color: grey;
    margin-left: 20px;
 }

 .header {
   height: 300px;
   overflow:hidden;
   font-family: 'SpongeBob Font Wide', sans-serif;
 }

 #headpic{
   opacity:50%;
   width: 790px;
   height: auto;
 }

 #headtext {  
   position: absolute; 
   margin-top: 50px;
   margin-left: 50px;
}

 .burre {
    padding: 40px;
 }

 .allergier {
    font-style: italic;
 }
 
 #burger_list {
    background-color: black;
    border: 10px dotted yellow;
 }

 .burtext {
    margin-left: 20px;
    color: white;
 }

 .wrapper {
   display: grid;
   grid-gap: 10px;
   grid-template-columns: auto auto auto;
   color: #fff;
}

.box {
   color: #fff;
   font-size: 100%;
   grid-template-columns: auto auto auto
}


 #contact{
    background-color: aquamarine;
    margin-top: 50px;
    margin-bottom: 40px;
    border: 10px solid #97f9ca
 }

 .ctext{
    margin-left: 20px;
    margin-bottom: 20px;
 }

 button:hover {
    color: green;
    background-color: greenyellow;
    cursor: pointer;
 }

 footer{
   margin-top: 20px;
 }
</style>