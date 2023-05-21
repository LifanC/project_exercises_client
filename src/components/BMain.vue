<script setup>

import PubSub from "pubsub-js";

const token =  PubSub.subscribe('', function (msg, data) {
  console.log('data',data)
});
// PubSub.unsubscribe(token)



</script>

<template>

</template>

<style scoped>

</style>