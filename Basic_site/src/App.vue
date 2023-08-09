<script setup>

</script>

<template>
<div class="hero">
  <div class="custom-button" v-on:click="getPayloadValue('1/4/6')>0 ? showValue(false,'1/0/7') : showValue(true,'1/0/7')">
    <i class="icon-lightbulb icon-4x" :class="getPayloadValue('1/4/6')>0 ? 'green' : 'red'"></i>
      <p class="Name">{{getPayloadName('1/0/7') }}
        <span 
          class="Button1" 
          :class="getPayloadName('1/0/7')"
        >
          {{ getPayloadValue('1/4/6') }}
        </span>
      </p>
  </div>
  <div class="custom-button" v-on:click="getPayloadValue('1/4/8')>0 ? showValue(false,'1/0/13') : showValue(true,'1/0/13')">
    <i class="icon-lightbulb icon-4x" 
      :class="getPayloadValue('1/4/8')>0 ? 'green' : 'red'">
    </i>
    <p class="Name">
      {{getPayloadName('1/0/13') }}
      <span class="Button2" 
        :class="getPayloadName('1/4/8')"
      >
      {{ getPayloadValue('1/4/8') }}
      </span>
    </p>
  </div>
</div>
</template>

<script>
export default {
  name: 'App',
  data: function () {
    return {
      connection: null,
      payloadData: [],
    }
  },
  methods: {
    sendMessage: function (value, device) {
      const messageObject = {
        value: value,
        device: device
      };
      const jsonString = JSON.stringify(messageObject);
      this.connection.send(jsonString);
    },
    showValue: function (value_send,address) {
      // Update the content of the custom-button div with the value of address
      const value = this.getPayloadValue(address);
      document.querySelector('.custom-button .Button2').textContent = value;
      //document.querySelector('[class=${CSS.escape(value)}]').textContent = value;
      this.sendMessage(value_send,address)
      console.log('Div hit with: ', address, value_send);
    },
    getPayloadValue: function (address) {
      // Assuming the item for address "1/3/0" exists in payloadData
      const item_x = this.payloadData.find((item) => item.address === address);
      if (item_x) {
        return item_x.payload;
      }
      return '';
    },
    getPayloadName: function (address) {
      // Assuming the item for address "1/3/0" exists in payloadData
      const item_x = this.payloadData.find((item) => item.address === address);
      if (item_x) {
        //cut down name to functional length
        var str = item_x.devicename;
        return str.split(")").pop() ;
      }
      return ''; // Return an empty string if the item is not found
    },
    handleWebSocketMessage: function (event) {
      const jsonData = JSON.parse(event.data);
      if (jsonData.topic === "KNXViewer" && jsonData.payload && jsonData.payload.length) {
        // Update the payloadData with the new data from the WebSocket
        this.payloadData = jsonData.payload;
      }
    },

  },
  created: function () {
    console.log("Starting Connection to WebSocket Server")
    this.connection = new WebSocket("ws://192.168.1.146:1880/websocket")
    this.connection.onopen = function (event) {
      console.log(event)
      console.log("Successfully connected to the echo WebSocket Server");
    }
    this.connection.onmessage = this.handleWebSocketMessage; // Use the handler to process received data
  }
}
</script>

<style scoped>

.Name {
  text-align: center;
  margin: 10px;
}

[class="icon-lightbulb icon-4x"] {
  display: inline-block;
  width: 100%;
  margin: auto;
  --color: lightcoral;
  text-align: center;
}
.hero {
  justify-content: space-around;
  align-content: space-around;
  display: flex;
  color: #fff;
  margin: 40px auto;
  background-color: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 15px;
  padding: 32px;
  backdrop-filter: blur(10px);
}
.custom-button {
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 30px;
  cursor: pointer;
  justify-content: center;
  align-items: center;
  width: 120px;
  height: 120px;
  border-radius: 15px;
  box-shadow: 3px 3px 10px 1px;
}
.custom-button:hover {
  background-color: rgba(255,255,255,0.2);
}

.green {
  color:greenyellow;
}

.red {
  color:red;
}

</style>