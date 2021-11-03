<template>
  <div class="qr-stream-wrap">
    <QrcodeStream
      v-if="qrScan"
      :camera="camera"
      @decode="onDecode"
      @init="onInit"
    >
      <div v-if="camera === 'off'" class="validation-pending">Loading ...</div>
    </QrcodeStream>

    <div v-if="!qrScan" class="qr-input">
      <div>
        {{ result }}
      </div>
      <button @click="openQrScan">สแกน</button>
    </div>
  </div>
</template>

<script>
import { QrcodeStream } from 'vue3-qrcode-reader'
export default {
  components: {
    QrcodeStream,
  },

  data() {
    return {
      camera: 'auto',
      result: null,

      qrScan: true,
    }
  },

  methods: {
    onInit(promise) {
      promise
        .catch(console.error)
    },

    openQrScan() {
      this.result = null
      this.qrScan = true
    },

    async onDecode(content) {
      this.camera = 'off'

      // pretend it's taking really long
      await this.timeout(3000)
      this.camera = 'auto'
      this.result = content
      this.qrScan = false
    },

    timeout(ms) {
      return new Promise(resolve => {
        window.setTimeout(resolve, ms)
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.qr-stream-wrap {
  max-width: 600px;
  margin: auto;
}

.qr-input {
  display: flex;
  justify-content: space-between;
  padding: 20px 10px;
  border: 1px solid;
}

.validation-success,
.validation-failure,
.validation-pending {
  position: absolute;
  width: 100%;
  height: 100%;

  background-color: rgba(255, 255, 255, 0.8);
  text-align: center;
  font-weight: bold;
  font-size: 1.4rem;
  padding: 10px;

  display: flex;
  flex-flow: column nowrap;
  justify-content: center;
}
.validation-pending {
  color: gold;
}
</style>
