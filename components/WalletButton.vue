<template>
  <div>
    <div class="absolute top-0 px-3 pt-1 right-4">
      <button
        v-if="!wallet.isConnected"
        class="text-white"
        title="Connect Wallet"
        @click="connectwallet()"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-8 w-8 mr-2 mt-2"
          viewBox="0 0 64 64"
          fill="currentColor"
          width="32"
          height="32"
        >
          <path
            class="st1"
            d="M56,20.1V20v-4c0-3.31-2.69-6-6-6H10c-3.31,0-6,2.69-6,6v34c0,3.31,2.69,6,6,6h40c3.31,0,6-2.69,6-6v-8.1
        c2.28-0.46,4-2.48,4-4.9V25C60,22.59,58.28,20.57,56,20.1z M58,25c0,1.3-0.84,2.4-2,2.82v-5.63C57.16,22.6,58,23.7,58,25z M50,54H10
        c-2.21,0-4-1.79-4-4V16c0-2.21,1.79-4,4-4h40c2.03,0,3.7,1.53,3.95,3.49C52.89,14.57,51.51,14,50,14H9c-0.55,0-1,0.45-1,1
        s0.45,1,1,1h41c2.21,0,4,1.79,4,4v8h-9c-3.86,0-7,3.14-7,7s3.14,7,7,7h9v8C54,52.21,52.21,54,50,54z M55,40H45c-2.76,0-5-2.24-5-5
        s2.24-5,5-5h10c1.13,0,2.16-0.39,3-1.03V37C58,38.65,56.65,40,55,40z"
          />
        </svg>
      </button>
      <button
        v-if="wallet.isConnected"
        class="text-white"
        :title="wallet.address"
        @click="showDisconnectDiv = !showDisconnectDiv"
      >
        <svg
          xmlns="http://www.w3.org/2000/svg"
          class="h-8 w-8 mr-2 mt-2"
          viewBox="0 0 64 64"
          fill="white"
          width="32"
          height="32"
        >
          <rect x="4" y="14" width="50" height="40" style="fill:green" />
          <path
            class="st1"
            d="M56,20.1V20v-4c0-3.31-2.69-6-6-6H10c-3.31,0-6,2.69-6,6v34c0,3.31,2.69,6,6,6h40c3.31,0,6-2.69,6-6v-8.1
        c2.28-0.46,4-2.48,4-4.9V25C60,22.59,58.28,20.57,56,20.1z M58,25c0,1.3-0.84,2.4-2,2.82v-5.63C57.16,22.6,58,23.7,58,25z M50,54H10
        c-2.21,0-4-1.79-4-4V16c0-2.21,1.79-4,4-4h40c2.03,0,3.7,1.53,3.95,3.49C52.89,14.57,51.51,14,50,14H9c-0.55,0-1,0.45-1,1
        s0.45,1,1,1h41c2.21,0,4,1.79,4,4v8h-9c-3.86,0-7,3.14-7,7s3.14,7,7,7h9v8C54,52.21,52.21,54,50,54z M55,40H45c-2.76,0-5-2.24-5-5
        s2.24-5,5-5h10c1.13,0,2.16-0.39,3-1.03V37C58,38.65,56.65,40,55,40z"
          />
        </svg>
      </button>
      <div v-if="showDisconnectDiv" class="absolute rounded-md top-12 px-2 py-4 bg-gray-800 text-white text-xs right-0">
        <button @click="disconnectAndClose()">
          Disconnect
        </button>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
    connectwallet: {
      type: Function,
      required: true
    },
    disconnectWallet: {
      type: Function,
      required: true
    },
    wallet: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      showDisconnectDiv: false
    }
  },
  methods: {
    disconnectAndClose () {
      this.disconnectWallet()
      this.showDisconnectDiv = false
    }
  }
})
</script>
