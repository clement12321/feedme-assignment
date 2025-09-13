<script setup>
defineProps({ bots: Array });
</script>

<template>
  <div>
    <h2 class="cookbot-header">Bots</h2>
    <ul>
      <li v-for="b in bots" :key="b.id">
        🧑‍🍳 Cook Bot -
        <span v-if="b.busy">Processing Order #{{ b.currentOrderId }}</span>
        <span v-else>IDLE</span>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.cookbot-header {
  margin-top: 2rem;
  margin-bottom: 0.2rem;
}
</style>
