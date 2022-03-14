<script setup>
import { ref } from "vue";
import MetaMaskConnection from "./components/MetaMaskConnection.vue";
import MainUserInterface from "./components/MainUserInterface.vue";

const walletConnection = ref(null);
</script>

<template>
  <MetaMaskConnection ref="walletConnection" />
  <div v-if="walletConnection && walletConnection.walletAddress">
    <MainUserInterface />
  </div>
</template>
