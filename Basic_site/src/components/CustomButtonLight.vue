<!-- CustomButtonLight.vue -->
<template>
  <div class="custom-button" @click="handleClick">
    <span class="background-value">{{ getPayloadValue }}</span>
    <i class="icon-lightbulb icon-4x" :class="getPayloadValue > 0 ? 'green' : 'red'"></i>
    <p class="Name">
    <p class="Name">{{ getPayloadName }}</p>
    <span :ref="refName">
    </span>
    </p>
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
  padding: 20px;
  /* Adjust as needed */
}

.custom-button:hover .Name {
  max-height: none;
  /* Zeigt den gesamten Text beim Hover an */
  white-space: normal;
  /* Erlaubt Textumbruch */
}

.Name {
  font-size: 14px;
  margin-top: 10px;
  max-height: 14px;
  /* Dies entspricht der Schriftgröße, um nur eine Zeile anzuzeigen */
  overflow: hidden;
  /* Überschüssigen Text ausblenden */
  white-space: nowrap;
  /* Verhindert, dass der Text umgebrochen wird */
  text-overflow: ellipsis;
  /* Fügt "..." am Ende des sichtbaren Textes hinzu */
}

.background-value {
  position: absolute;
  top: 0;
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
