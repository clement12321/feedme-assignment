<script setup>
const emit = defineEmits(["new-order", "add-bot", "remove-bot"]);
</script>

<template>
  <div>
    <button @click="emit('new-order','Normal')" class="new-normal-btn">New Normal Order</button>
    <button @click="emit('new-order','VIP')" class="new-vip-btn">New VIP Order</button>
    <button @click="emit('add-bot')" class="plus-bot-btn">+ Bot</button>
    <button @click="emit('remove-bot')" class="minus-bot-btn">- Bot</button>
  </div>
</template>
