<script setup>
import CustomButtonLight from './components/CustomButtonLight.vue'
</script>

<template>
  <div class="hero">
    <CustomButtonLight addressValue="1/4/8" addressName="1/0/13" :getPayloadValueFunc="getPayloadValue"
      :getPayloadNameFunc="getPayloadName" :showValueFunc="showValue" />
    <CustomButtonLight addressValue="1/4/6" addressName="1/0/7" :getPayloadValueFunc="getPayloadValue"
      :getPayloadNameFunc="getPayloadName" :showValueFunc="showValue" />
    <CustomButtonLight addressValue="1/4/5" addressName="1/0/6" :getPayloadValueFunc="getPayloadValue"
      :getPayloadNameFunc="getPayloadName" :showValueFunc="showValue" />
    <CustomButtonLight addressValue="1/4/10" addressName="1/0/14" :getPayloadValueFunc="getPayloadValue"
      :getPayloadNameFunc="getPayloadName" :showValueFunc="showValue" />
    <CustomButtonLight addressValue="1/4/7" addressName="1/0/8" :getPayloadValueFunc="getPayloadValue"
      :getPayloadNameFunc="getPayloadName" :showValueFunc="showValue" />
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
    showValue: function (value_send, address) {
      // Update the content of the custom-button div with the value of address
      const value = this.getPayloadValue(address);
      this.sendMessage(value_send, address)
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
      // Assuming the item for address exists in payloadData
      const item_x = this.payloadData.find((item) => item.address === address);
      if (item_x) {
        //cut down name to functional length
        var str = item_x.devicename;
        return str.split(")").pop();
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
.hero {
  justify-content: space-around;
  align-content: space-around;
  display: flex;
  color: #fff;
  margin: 40px auto;
  background-color: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 15px;
  padding: 32px;
  backdrop-filter: blur(10px);
}
</style>