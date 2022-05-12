<template>
  <div>
    <NavBar
      :errorui="errorui"
      :display-error-bar="displayErrorBar"
      :dismiss-error-bar="dismissErrorBar"
      :wallet="wallet"
      :update-wallet="updateWallet"
    />
    <QueryForm
      :errorui="errorui"
      :display-error-bar="displayErrorBar"
      :dismiss-error-bar="dismissErrorBar"
      :wallet="wallet"
    />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { BeaconWallet } from '@taquito/beacon-wallet'
Vue.prototype.$beaconwallet = new BeaconWallet({ name: 'tezos-vue-starter-wallet' })
export default Vue.extend({
  name: 'IndexPage',
  data () {
    return {
      wallet: {
        address: '',
        balance: '',
        isConnected: false
      },
      errorui: {
        hasError: false,
        errorMessage: ''
      }
    }
  },
  methods: {
    displayErrorBar (message:string) {
      this.errorui.hasError = true
      this.errorui.errorMessage = message
    },
    dismissErrorBar () {
      this.errorui.hasError = false
      this.errorui.errorMessage = ''
    },
    updateWallet (address:string, connected:boolean) {
      this.wallet.address = address
      this.wallet.isConnected = connected
    }
  }
})
</script>
