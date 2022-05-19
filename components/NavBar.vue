<template>
  <div>
    <nav class="bg-gray-800">
      <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex justify-between h-14">
          <div class="flex">
            <div class="text-white px-8 py-4">
              Tezos Vue Starter
            </div>
          </div>
          <div class="flex items-center">
            <div class="flex-shrink-0">
              <SettingsButton
                :networks="networks"
                :cnetwork="cnetwork"
                :update-network="updateNetwork"
              />
              <WalletButton
                :connectwallet="connectWallet"
                :wallet="wallet"
                :disconnect-wallet="disconnectWallet"
              />
            </div>
          </div>
        </div>
      </div>
    </nav>
    <div v-if="errorui.hasError" id="errormessagestrip" class="h-8 py-1 max-w-7xl mx-auto px-8 sm:px-6 lg:px-8 bg-pink-500 text-center text-white">
      <button
        class="float-right w-5 h-5 mr-3 bg-white border border-gray-400 shadow-sm rounded-md"
        @click="dismissErrorBar()"
      >
        <svg fill="#999" height="25" width="25" viewBox="0 0 32 32" xmlns="http://www.w3.org/2000/svg">
          <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z" />
          <path d="M0 0h24v24H0z" fill="none" />
        </svg>
      </button>
      <div>{{ errorui.errorMessage }}</div>
    </div>
    <div v-else class="h-8 py-1 max-w-7xl mx-auto px-8 sm:px-6 lg:px-8" />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { TezosToolkit } from '@taquito/taquito'
export default Vue.extend({
  props: {
    errorui: {
      type: Object,
      required: true
    },
    displayErrorBar: {
      type: Function,
      required: true
    },
    dismissErrorBar: {
      type: Function,
      required: true
    },
    wallet: {
      type: Object,
      required: true
    },
    updateWallet: {
      type: Function,
      required: true
    },
    networks: {
      type: Object,
      required: true
    },
    cnetwork: {
      type: Object,
      required: true
    },
    updateNetwork: {
      type: Function,
      required: true
    }
  },
  methods: {
    async connectWallet () {
      const tezos = new TezosToolkit(this.cnetwork.node)
      const bwallet = Vue.prototype.$beaconwallet
      tezos.setWalletProvider(bwallet)
      const activeAccount = await bwallet.client.getActiveAccount()
      if (activeAccount) {
        this.updateWallet(activeAccount.address, true)
      } else {
        try {
          const permissions = await bwallet.client.requestPermissions()
          this.updateWallet(permissions.address, true)
        } catch (error) {
          this.displayErrorBar((error as Error).message)
        }
      }
    },
    async disconnectWallet () {
      await Vue.prototype.$beaconwallet.clearActiveAccount()
      this.updateWallet('', false)
    }
  }
})
</script>
