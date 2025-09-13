<script setup>
import { reactive, ref } from "vue";
import OrderList from "./components/OrderList.vue";
import BotList from "./components/BotList.vue";
import Controls from "./components/Controls.vue";

const state = reactive({
  orders: [],
  bots: [],
});

let orderCounter = ref(1);
let botCounter = ref(1);

// Add new order
function addOrder(type) {
  const order = {
    id: orderCounter.value++,
    type,
    status: "PENDING",
  };

  if (type === "VIP") {
    let lastVipIndex = state.orders.findLastIndex(o => o.type === "VIP" && o.status === "PENDING");
    if (lastVipIndex !== -1) {
      state.orders.splice(lastVipIndex + 1, 0, order);
    } else {
      state.orders.unshift(order);
    }
  } else {
    state.orders.push(order);
  }
  assignOrders();
}

// Add bot
function addBot() {
  state.bots.push({
    id: botCounter.value++,
    busy: false,
    currentOrderId: null,
  });
  assignOrders();
}

// Remove bot
function removeBot() {
  const bot = state.bots.pop();
  if (!bot) return;

  //Change order back to PENDING
  if (bot.busy && bot.currentOrderId !== null) {
    const order = state.orders.find(o => o.id === bot.currentOrderId);
    if (order) order.status = "PENDING";
  }
  assignOrders();
}

// Assign pending orders to free bots
function assignOrders() {
  for (const bot of state.bots) {
    if (bot.busy) continue;

    const nextOrder = state.orders.find(o => o.status === "PENDING");
    if (nextOrder) {
      bot.busy = true;
      bot.currentOrderId = nextOrder.id;
      nextOrder.status = "PROCESSING";

      setTimeout(() => {
        nextOrder.status = "COMPLETE";
        bot.busy = false;
        bot.currentOrderId = null;
        assignOrders();
      }, 10000); // 10 seconds
    }
  }
}
</script>

<template>
  <div>
    <h1>🍟 FeedMe Order Bot Prototype</h1>
    <Controls @new-order="addOrder" @add-bot="addBot" @remove-bot="removeBot" />
    <div>
      <OrderList :orders="state.orders" />
      <BotList :bots="state.bots" />
    </div>
  </div>
</template>
