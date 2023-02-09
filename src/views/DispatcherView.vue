<template>
    <div id="orders">
      <div id="orderList" >
      
        <div v-for="(order, key) in orders" v-bind:key="'order'+key">

          <div>
            ==================<br>
            #{{key}} : {{formatOrder(order)}}
          </div>
          <div style="font-style: ;font-family: 'Arial';">
            <!--{{order.personalInfo.name}}, -->
          <div style="font-size:small">
            <ul>
              <li><b>{{order.personalInfo.name}}</b></li>
              <li>{{order.personalInfo.email}}</li>
              <li>{{order.personalInfo.pay}}</li>
              <li>{{order.personalInfo.gender}}</li>
            </ul>
          </div>
        </div>
        </div>
        <button v-on:click="clearQueue">Clear Queue</button>
      </div>
        <div id="dots">
          <div v-for="(order, key) in orders" v-bind:style="{ left: order.details.x + 'px', top: order.details.y + 'px'}" v-bind:key="'dots' + key">
            {{key}}
        </div>
      </div>
    </div>
  </template>
  <script>
  import io from 'socket.io-client'
  const socket = io();

  export default {
    name: 'DispatcherView',
    data: function () {
      return {
        orders: null
      }
    },
    created: function () {
      socket.on('currentQueue', data =>
        this.orders = data.orders);
        console.log("Laddad och klar");
    },
    methods: {
      clearQueue: function () {
        socket.emit('clearQueue');
      },
      formatOrder: function (order) {
        let str = ""
        for (const itemName in order.orderItems) {
          if (str != ""){
            str += ", "
          }
          str += itemName + " " + order.orderItems[itemName].toString()
        }
        return str
      }
    }
  }
  </script>
  <style>

  #orderList {
    top:1em;
    right:1em;
    position: absolute;
    z-index: 2;
    color:black;
    /* background: rgba(255,255,255, 0.5);*/
    background: white;
    padding: 1em;
  }
  #dots {
    position: relative;
    margin: 0;
    padding: 0;
    background-size: cover;
    background-repeat: no-repeat;
    background: url(../../public/img/polacks.jpg);
    width: 700px;
    height: 400px;
  }
  
  #dots div {
    position: absolute;
    color:rgb(0, 0, 0);
    font-weight:bold;
    background-color: rgb(242, 108, 12);
    border-radius: 50px;
    border:4px solid rgb(245, 17, 131);
    width:50px;
    height:20px;
    text-align: center;
  }
  </style>
  