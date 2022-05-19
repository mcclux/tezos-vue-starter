<template>
  <div class="flex justify-center">
    <div class="md:w-1/2 sm:w-full">
      <div class="p-6 shadow-lg rounded-lg bg-gray-100 text-gray-700">
        <h2 class="font-semibold text-3xl mb-5">
          Check a Tezos wallet address
        </h2>
        <div>
          <input
            v-model="tezosAddress"
            class="w-full h-12 p-4 text-xl text-slate-400 border-2 border-gray-400 focus:border-gray-800 rounded-md"
            spellcheck="false"
            @focus="resetErrorAndBalance"
          >
        </div>
        <div class="grid grid-cols-5 grid-flow-col gap-4 my-px">
          <div class="col-span-5 justify-self-end h-3">
            <button v-show="wallet.address != tezosAddress" class="text-xs float-right underline" @click="fillAddressInput()">
              Here's Yours
            </button>
          </div>
        </div>
        <div class="grid grid-cols-5 grid-flow-col gap-4 my-px">
          <div class="col-span-1 mb-2">
            <button
              type="button"
              class="inline-block px-6 py-2.5 mt-6 bg-blue-600 text-white font-medium text-xs leading-tight uppercase rounded shadow-md hover:bg-blue-700 hover:shadow-lg focus:bg-blue-700 focus:shadow-lg focus:outline-none focus:ring-0 active:bg-blue-800 active:shadow-lg transition duration-150 ease-in-out"
              @click="queryTzAddress(tezosAddress)"
            >
              Check
            </button>
          </div>
          <div class="col-span-1 mt-6 px-2">
            <img v-show="queryStarted" src="/spinner.svg" height="36" width="36">
          </div>
          <div class="col-span-3 justify-self-end text-2xl text-slate-400 mb-0 mt-6">
            {{ addressBalance }}&nbsp;<span v-if="addressBalance">ꜩ</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
// tz1a8QSHVHVkCmDYio6ptk7LAL7ZvywL6Vjo
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
    networks: {
      type: Object,
      required: true
    },
    cnetwork: {
      type: Object,
      required: true
    }
  },
  data () {
    return {
      tezosAddress: '',
      addressBalance: '',
      queryStarted: false,
      queryCompleted: false
    }
  },
  methods: {
    async queryTzAddress (address:string) {
      this.queryStarted = true
      this.addressBalance = ''
      const rpcUrl = this.cnetwork.node
      const tezos = new TezosToolkit(rpcUrl)
      try {
        const balance = await tezos.rpc.getBalance(address)
        this.queryCompleted = true
        this.queryStarted = false
        this.addressBalance = (balance.toNumber() / 1000000).toString()
      } catch (error) {
        this.queryCompleted = true
        this.queryStarted = false
        this.displayErrorBar((error as Error).message)
      }
    },
    resetErrorAndBalance () {
      this.addressBalance = ''
      this.dismissErrorBar()
    },
    fillAddressInput () {
      this.tezosAddress = this.wallet.address
    }
  }
})
</script>
