<script setup>

</script>

<template>
<table class="KNX_overview">
      <thead>
        <tr>
          <th>Address</th>
          <th>DPT</th>
          <th>Payload</th>
          <th>Device Name</th>
          <th>Last Update</th>
          <th>Switch</th>
          <th>status</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in payloadData" :key="item.addressRAW">
          <td>{{ item.address }}</td>
          <td>{{ item.dpt }}</td>
          <td>{{ item.payload }}</td>
          <td>{{ item.devicename }}</td>
          <td>{{ item.lastupdate }}</td>
          <td>
            <span v-if="typeof item.payload === 'boolean'"><button v-on:click="sendMessage( true, item.dpt )" >true</button></span>
            <span v-if="typeof item.payload === 'boolean'"><button v-on:click="sendMessage( false, item.dpt )" >false</button></span></td>
          <td>          
              <span v-if="typeof item.payload === 'boolean'">
              <span v-if="item.payload" class="dot green"></span>
              <span v-else class="dot red"></span>
              </span>
          </td>
        </tr>
      </tbody>
    </table>
</template>
<script>
export default {
  name: 'App',
  data: function() {
    return {
      connection: null,
      payloadData: [],
    }
  },
  methods: {
    sendMessage: function(value, device) {
      const messageObject = {
        value: value,
        device: device
      };
      const jsonString = JSON.stringify(messageObject);
      this.connection.send(jsonString);
    },
    handleWebSocketMessage: function(event) {
      const jsonData = JSON.parse(event.data);
      if (jsonData.topic === "KNXViewer" && jsonData.payload && jsonData.payload.length) {
        // Update the payloadData with the new data from the WebSocket
        this.payloadData = jsonData.payload;
      }
    },
    
  },
  created: function() {
    console.log("Starting Connection to WebSocket Server")
    this.connection = new WebSocket("ws://192.168.1.146:1880/websocket")
    this.connection.onopen = function(event) {
      console.log(event)
      console.log("Successfully connected to the echo WebSocket Server");
    }
    this.connection.onmessage = this.handleWebSocketMessage; // Use the handler to process received data
    //this.connection.onmessage = function(event) {
     // console.log(JSON.parse(event.data))
    
  }
}
</script>
<style scoped>
.dot {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    display: inline-block;
    margin-right: 5px;
  }
  .green {
    background-color: green;
  }
  .red {
    background-color: red;
  }
  .KNX_overview {
    width: 100%;
  }
</style>
