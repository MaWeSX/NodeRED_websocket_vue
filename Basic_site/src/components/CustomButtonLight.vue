<!-- CustomButtonLight.vue -->
<template>
  <div class="custom-button" @click="handleClick">
    <span class="background-value">{{ getPayloadValue }} %</span>
    <p class="Name">{{ getPayloadName }}</p>
    <i class="icon-lightbulb icon-4x button" :class="getPayloadValue > 0 ? 'green' : 'red'"></i>
    <span :ref="refName">
    </span>

  </div>
</template>

<script>
export default {
  props: {
    addressValue: {
      type: String,
      required: true
    },
    addressName: {
      type: String,
      required: true
    },
    getPayloadValueFunc: {
      type: Function,
      required: true
    },
    getPayloadNameFunc: {
      type: Function,
      required: true
    },
    showValueFunc: {
      type: Function,
      required: true
    },
    // ... andere props
  },
  computed: {
    refName() {
      return "span_" + this.addressName.replace(/\//g, '_');
    },
    getPayloadValue() {
      return this.getPayloadValueFunc(this.addressValue);
    },
    getPayloadName() {
      return this.getPayloadNameFunc(this.addressName);
    }
  },
  methods: {
    handleClick() {
      const value = this.getPayloadValue > 0 ? false : true;
      this.showValueFunc(value, this.addressName);
    }
  }
}
</script>

<style>
.custom-button {
  position: relative;
  /* To position child elements absolutely */
  display: flex;
  flex-direction: column;
  align-items: center;
  /* To center the icon and name horizontally */
  justify-content: center;
  /* To center the icon vertically */
  padding: 10px;
  border: 1px solid #ccc;
  margin-bottom: 30px;
  cursor: pointer;
  width: 180px;
  height: 180px;
  border-radius: 15px;
  box-shadow: 3px 3px 10px 1px;

}

.custom-button:hover .Name {
  max-height: none;
  /* Zeigt den gesamten Text beim Hover an */
  white-space: normal;
  /* Erlaubt Textumbruch */

}

.custom-button:hover {
  background-color: rgba(255, 255, 255, 0.2);
}

.button {
  margin-bottom: 10px;
}


.Name {
  font-size: 15px;
  margin: -10px 10px 10px 10px;
  max-height: 20px;
  text-align: center;
  /* Dies entspricht der Schriftgröße, um nur eine Zeile anzuzeigen */
  overflow: hidden;
  /* Überschüssigen Text ausblenden */
  /* white-space: nowrap; */
  /* Verhindert, dass der Text umgebrochen wird */
  text-overflow: ellipsis;
  /* Fügt "..." am Ende des sichtbaren Textes hinzu */
}

.background-value {
  position: absolute;
  top: 15;
  left: 0;
  right: 0;
  bottom: 0;
  z-index: -1;
  /* Place it below other content */
  font-size: 40px;
  /* Adjust to your needs */
  opacity: 0.1;
  /* Make it subtle */
  display: flex;
  align-items: center;
  justify-content: center;
}

.green {
  color: greenyellow;
}

.red {
  color: red;
}
</style>
