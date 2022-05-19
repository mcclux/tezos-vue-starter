<template>
  <div class="absolute top-0 px-3 right-12">
    <button class="py-1 px-6 my-3 text-white" @click="showSettingsDiv = !showSettingsDiv">
      <span class="text-xs px-2 font-mono font-thin tracking-tighter">{{ cnetwork.networkname }}</span>
      <img src="/cog.svg" width="24" height="24" class="inline">
    </button>
    <div
      v-if="showSettingsDiv"
      class="w-100 absolute rounded-md right-12 top-12 px-2 py-3 bg-gray-800 text-black text-xs right-0"
    >
      <div class="text-right leading-loose w-80">
        <select id="network" class="" @change="changeNetwork($event)">
          <option
            v-for="network in networks"
            :key="network.networkname"
            :selected="network.networkname === cnetwork.networkname? true : false"
          >
            {{ network.networkname }}
          </option>
        </select>
        <select id="rpc" @change="changeNetworkAndNode(cnetwork.networkname, $event)">
          <option
            v-for="n in networks[cnetwork.networkname.toLowerCase()].nodes"
            :key="n"
            :selected="n === cnetwork.node? true : false"
          >
            {{ n }}
          </option>
        </select>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
export default Vue.extend({
  props: {
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
  data () {
    return {
      showSettingsDiv: false
    }
  },
  methods: {
    changeNetwork (selectevent: { target: { value: string } }) {
      this.updateNetwork(selectevent.target.value, this.networks[selectevent.target.value.toLowerCase()].nodes[0])
    },
    changeNetworkAndNode (network:String, selectevent: { target: { value: string } }) {
      this.updateNetwork(network, selectevent.target.value)
    }
  }
})
</script>
