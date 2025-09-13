<script setup>
defineProps({ orders: Array });
</script>

<template>
  <div>
    <h2 class="order-header">Orders</h2>
    <div>
      <h3 class="pending-header">Pending / Processing</h3>
      <ul>
        <li v-for="o in orders.filter(o => o.status !== 'COMPLETE')" :key="o.id">
          🍔 Order #{{ o.id }} - {{ o.type }} ({{ o.status }})
        </li>
      </ul>

      <h3 class="complete-header">Complete</h3>
      <ul>
        <li v-for="o in orders.filter(o => o.status === 'COMPLETE')" :key="o.id">
          🍽️ Order #{{ o.id }} - {{ o.type }} ({{o.status}})
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
h2, h3{
  margin-top: 1rem;
}
</style>
