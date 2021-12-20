<template>
  <div id="app">
    <div id="metamask" v-if="status">Metamask Connection: 🟢</div>
    <div id="metamask" v-else>Metamask Connection: 🔴</div>
    <div>
      Wallet: <span id="wallet">{{ $store.state.walletAccount }} </span>
    </div>
    <div id="nav">
      <router-link to="/">Native Token</router-link> |
      <router-link to="/erc20">ERC20 Token</router-link>
    </div>
    <router-view />
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import { ethers, utils } from "ethers";

declare global {
  export interface Window {
    ethereum: any;
  }
}

@Component
export default class ConnectProvider extends Vue {
  @Prop() private msg!: string;
  private status = false;
  // request access to the user's MetaMask account
  async mounted(): Promise<void> {
    const provider = new ethers.providers.JsonRpcProvider();
    const signer = provider.getSigner();
    this.$store.state.provider = provider;
    this.status = true;
    this.$store.state.walletAccount = await signer.getAddress();
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
}

#metamask {
  color: #2c3e50;
}

#wallet {
  font-weight: bold;
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
