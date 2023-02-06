<template>

  <div id="orders">
    
    <div id="orderList" >
     
      <div v-for="(order, key) in orders" v-bind:key="'order'+key">
        <div>
            #{{key}} : {{formatOrder(order)}}
        </div>

        <div style="font-style:italic ;font-family: 'Times New Roman', Times, serif;">
            {{order.personalInfo.name}}
          <div style="font-size:small">
            ({{order.personalInfo.email}}, {{order.personalInfo.pay}}, {{order.personalInfo.gender}})
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
        // console.log(this.orders);
        console.log("Pundare");
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
        console.log("Tjolahop")
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
    background: rgba(255,255,255, 0.5);
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
  