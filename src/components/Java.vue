<script setup>

PubSub.subscribe('main', function (msg, data) {
  switch (data[0]) {
    case '7':
      if (data[1]) {

      } else if (!data[1]) {

      }
      break
  }
})

</script>

<template>

</template>

<style scoped>

</style>