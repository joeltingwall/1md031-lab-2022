<template>
  
  <!--<div>
    <h3>{{ burger.name }}</h3>
    <p>{{ burger.kCal }}</p>
  </div>-->
  
  <div id="box">
    <h2>{{burger.name}}</h2>
    <!--Amount: {{amountOrdered}}-->
    <img v-bind:src="burger.img" style="width:200px">
    <ul>
      <li>{{burger.kCal}} kCal</li>
      <section v-if="burger.lactose" class="allergi">Laktos</section>
      <section v-if="burger.gluten" class="allergi">Gluten</section>
      <li>Lökringar</li>
      <li>Nötkött</li>
    </ul> 
    
    <button @click="subtractBurger">-</button>
    Amount: {{amountOrdered}}
    <button @click="addBurger">+</button>
    
    <!--
    <div id="orders">
      <button v-on:click="markDone(key)">Klart</button>
    </div>
-->

  </div> 
  
  </template>
  
</script>

Det saknas en stängande hakparentes i metoden addBurger och en stängande klammer i metoden subtractBurger. Korrigerad kod:

<script>

  export default {
    name: 'OneBurger',
    props: {
      burger: Object
    },

    data: function () {
      return {
        amountOrdered: 0,
      }
    },

    methods: {

      addBurger: function () {
        this.amountOrdered += 1;
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              });
    },
    
      subtractBurger: function () {
        if (this.amountOrdered > 0) {
        this.amountOrdered -= 1;
        this.$emit('orderedBurger', { name:   this.burger.name, 
                                amount: this.amountOrdered 
                              });
     }
    },
    }
  }
  
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  
  <style scoped>

  .allergi {
    font-weight: bold;
    color: blue;
 }

 #box {
    column-width: 30%;
    padding: 10px;
    display: inline-block;
}
  
  </style>
  