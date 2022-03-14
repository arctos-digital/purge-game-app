<!-- 
  TO DO
  --------------------
  Error handling
  Check for/switch to correct network
  Move wallet button to parent component (walletAddress already exposed)
-->

<script setup>
import { ref, onMounted } from "vue";
import MetaMaskOnboarding from "@metamask/onboarding"; // https://docs.metamask.io/guide/onboarding-library.html

const isMetaMaskInstalled = () => {
  if (
    typeof ethereum !== "undefined" &&
    typeof ethereum.isMetaMask !== "undefined"
  ) {
    return true;
  }
};

const walletAddress = ref(null);
defineExpose({ walletAddress });

const getAccounts = async () => {
  if (isMetaMaskInstalled()) {
    const accounts = await ethereum.request({ method: "eth_accounts" });
    walletAddress.value = accounts[0] || null;
  }
};

const onboarding = new MetaMaskOnboarding();

const onClickInstall = () => {
  onboarding.startOnboarding();
};

const onClickConnect = async () => {
  await ethereum.request({ method: "eth_requestAccounts" });
  getAccounts();
};

onMounted(() => {
  getAccounts();
});
</script>

<template>
  <!-- Full screen modal if wallet not connected, else load main interface -->
  <div
    v-if="!walletAddress"
    class="
      absolute
      top-0
      left-0
      w-full
      h-full
      bg-black
      opacity-70
      grid
      place-content-center
    "
  >
    <div class="inline bg-white p-4 rounded-lg text-black text-xl text-center">
      MetaMask connection required<br />
      <button
        v-if="isMetaMaskInstalled()"
        @click="onClickConnect()"
        class="
          mt-2
          px-4
          py-1
          mt-2
          bg-zinc-400
          rounded-md
          hover:ring
          ring-amber-300
        "
      >
        <img src="/metamask-fox.svg" class="inline" />
        Connect Wallet
      </button>
      <button
        v-else
        @click="onClickInstall()"
        class="
          mt-2
          px-4
          py-1
          mt-2
          bg-zinc-400
          rounded-md
          hover:ring
          ring-amber-300
        "
      >
        <img src="/metamask-fox.svg" class="inline" />
        Install MetaMask
      </button>
    </div>
  </div>
  <!-- Wallet button, to be moved to parent component -->
  <div
    v-if="walletAddress"
    class="
      absolute
      top-0
      right-0
      z-10
      m-1
      px-4
      md:py-1
      bg-black
      rounded-md
      hover:ring-2
      ring-amber-500
    "
  >
    <button @click="walletAddress = null">
      <img src="/metamask-fox.svg" class="inline scale-75" />
      {{ walletAddress.substring(0, 5) }}...{{ walletAddress.substring(38) }}
    </button>
  </div>
</template>
