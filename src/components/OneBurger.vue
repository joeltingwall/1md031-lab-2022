<template>
  
  <!--<div>
    <h3>{{ burger.name }}</h3>
    <p>{{ burger.kCal }}</p>
  </div>-->
  
  <div id="box">
    <h2>{{burger.name}}</h2>
    <img src="/img/product_dubbel-originalburgare.png" style="width:200px">
    <ul>
      <li>{{burger.kCal}} kCal</li>
      <li><section class="allergi">Laktos</section></li>
      <li>Lökringar</li>
      <li>Nötkött</li>
      </ul>
    </div>  
  
  </template>
  
  <script>

  export default {
    name: 'OneBurger',
    props: {
      burger: Object
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
  