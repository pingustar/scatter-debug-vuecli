<template>
  <button @click="login">Login</button>
  <div v-if="username">Username: {{ username }}</div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import ScatterJS from "@scatterjs/core";
import ScatterEOS from "@scatterjs/eosjs2";

const username = ref<string>("");

const login = async () => {
  ScatterJS.plugins(new ScatterEOS());

  const network = ScatterJS.Network.fromJson({
    blockchain: "eos",
    chainId: "aca376f206b8fc25a6ed44dbdc66547c36c6c33e3a119ffbeaef943642f0e906",
    host: "nodes.get-scatter.com",
    port: 443,
    protocol: "https",
  });

  const connected = await ScatterJS.connect("Test App", { network });
  if (!connected) return console.error("no scatter");

  const id = await ScatterJS.login();
  if (!id) return console.error("no identity");

  const account = ScatterJS.account("eos");

  if (!account) return console.error("no account");
  console.log(account);
  username.value = account.name;
};
</script>
