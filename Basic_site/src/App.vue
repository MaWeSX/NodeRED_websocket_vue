<script setup>

</script>

<template>
<div class="hero">
  <div class="custom-button" v-on:click="">
    <i class="icon-lightbulb icon-4x"></i><p class="Name">Office</p>
  </div>
  <div class="custom-button" v-on:click="">
    <i class="icon-lightbulb icon-4x"></i><p class="Name">Office</p>
  </div>
  <div class="custom-button" v-on:click="">
    <i class="icon-lightbulb icon-4x"></i>{{ getPayloadValue('1/3/0') }}
  </div>
  <div class="custom-button" v-on:click="showValue('1/3/0')">
    <i class="icon-lightbulb icon-4x"></i>{{ getPayloadValue('1/3/0') }}
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
    updateValue: function (address) {
      // Assuming the item for address "1/3/0" exists in payloadData
      const item1_3_0 = this.payloadData.find((item) => item.address === "1/3/0");
      if (item1_3_0) {
        // Modify the payload of item1_3_0 based on your requirements
        // For example, toggling the value between true and false
        item1_3_0.payload = !item1_3_0.payload;
        console.log('Button hit');
      };
    },
    showValue: function (address) {
      // Update the content of the custom-button div with the value of item.address = "1/3/0"
      const value = this.getPayloadValue(address);
      document.querySelector('.custom-button').textContent = value;
      console.log('Div hit with: ', value);
    },
    getPayloadValue: function (address) {
      // Assuming the item for address "1/3/0" exists in payloadData
      const item_x = this.payloadData.find((item) => item.address === address);
      if (item_x) {
        //console.log('Value found for: '& item_x)
        return item_x.payload;
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
</style>
