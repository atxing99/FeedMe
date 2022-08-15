<script setup lang="ts">
// import Header from './Header.vue';
// import Footer from './Footer.vue';
import { ref, computed } from 'vue';

// enum MemberLevel {
//   Platinum,
//   Gold_Member,
//   Normal_Member
// }

// const newUser: User = {
//   username: 'Ting Xing',
//   memberLevel: MemberLevel.Platinum
// };

// interface User {
//   username: string;
//   memberLevel: MemberLevel;
// }

// Step-1: Interface
interface Order {
  id: number;
  isVIP: boolean;
  botId: number | null;
  isCompleted: boolean;
}

interface Bot {
  id: number;
  orderId: number | null;
  timer?: NodeJS.Timeout;
}

// Declaration
let botIndex = 1;
let orderIndex = 1;
const bots = ref<Bot[]>([]);
const orders = ref<Order[]>([]);

const pendingOrders = computed(() =>
  orders.value.filter((order) => order.isCompleted === false)
);

const completedOrders = computed(() =>
  orders.value.filter((order) => order.isCompleted === true)
);

// Step-2: Create Functions
function addBot() {
  const bot = {
    id: botIndex++,
    orderId: null
  };
  bots.value.push(bot);
  botGetTask(bot);
}

function removeBot() {
  let removedBot = bots.value.pop();
  if (removedBot?.orderId) {
    var selectedOrder = orders.value.find(
      (order) => order.id === removedBot?.orderId
    );
    if (selectedOrder) {
      selectedOrder.botId = null;
      removedBot.orderId = null;

      console.log('before clearTimeout', removedBot.timer);
      clearTimeout(removedBot.timer);
      console.log('after clearTimeout', removedBot.timer);
    }
  }
}

function addOrder(isVIP = false) {
  orders.value.push({
    id: orderIndex++,
    isVIP: isVIP,
    botId: null,
    isCompleted: false
  });

  const freeBot: Bot = bots.value.filter((bot) => bot.orderId === null)[0];

  if (freeBot) {
    botGetTask(freeBot);
  }
}

function botGetTask(bot: Bot) {
  const freePendingOrder: Order = orders.value
    .filter((order) => order.botId === null)
    .sort((orderA, orderB) => Number(orderB.isVIP) - Number(orderA.isVIP))[0];

  if (freePendingOrder) {
    freePendingOrder.botId = bot.id;
    bot.orderId = freePendingOrder.id;
    bot.timer = setTimeout(() => {
      freePendingOrder.isCompleted = true;
      bot.orderId = null;
      botGetTask(bot);
    }, 5000);
  }
}
</script>

<template>
  <div class="column window-height bg-grey">
    <div class="col-shrink bg-orange">
      <div class="text-h5 text-center q-pa-md">Add Robot/Add order</div>
      <div class="row">
        <div class="col q-pa-md">
          <q-card class="my-card">
            <q-card-section class="bg-purple text-white">
              <div class="text-h6">Robot</div>
            </q-card-section>

            <q-card-actions align="around">
              <q-btn flat @click="removeBot()">- Bot</q-btn>
              <q-btn flat @click="addBot()">+ Bot</q-btn>
            </q-card-actions>
          </q-card>
        </div>
        <div class="col q-pa-md">
          <q-card class="my-card">
            <q-card-section class="bg-purple text-white">
              <div class="text-h6">Order</div>
            </q-card-section>

            <q-card-actions align="around">
              <q-btn flat @click="addOrder()">New Normal Order</q-btn>
              <q-btn flat @click="addOrder(true)">New VIP Order</q-btn>
            </q-card-actions>
          </q-card>
        </div>
      </div>
    </div>
    <div class="col q-pa-md bg-green">
      <q-card class="my-card">
        <q-card-section class="bg-purple text-white">
          <div class="text-h6">Processing</div>
        </q-card-section>

        <div class="row">
          <div v-for="bot in bots" :key="bot.id" class="col-4 q-pa-sm">
            <div class="q-ma-sm q-pa-md" style="border: 1px solid black">
              <div>Robot ID: {{ bot.id }}</div>
              <div>
                Order ID:
                {{ bot.orderId == null ? 'Pending Task' : bot.orderId }}
              </div>
              <div>Status: ToDo</div>
            </div>
          </div>
        </div>
      </q-card>
    </div>
    <div class="col bg-grey-4">
      <div class="row">
        <div class="col q-pa-md">
          <q-card class="my-card">
            <q-card-section class="bg-purple text-white">
              <div class="text-h6">Pending</div>
            </q-card-section>

            <div class="q-pa-sm">
              <table class="text-center" style="width: 100%">
                <tr>
                  <td>Order ID</td>
                  <td>isVIP</td>
                  <td>Status</td>
                  <td>Count Down</td>
                  <td>Queue</td>
                </tr>

                <tr v-for="(order, index) in pendingOrders" :key="order.id">
                  <td>{{ order.id }}</td>
                  <td>{{ order.isVIP ? 'VIP' : 'Normal Member' }}</td>
                  <td :class="{ processing: order.botId != null }">
                    {{ order.botId === null ? 'Pending' : 'Processing' }}
                  </td>
                  <td>1</td>
                  <td>{{ index + 1 }}</td>
                </tr>
              </table>
            </div>
          </q-card>
        </div>
        <div class="col q-pa-md">
          <q-card class="my-card">
            <q-card-section class="bg-purple text-white">
              <div class="text-h6">Complete</div>
            </q-card-section>

            <div class="q-pa-sm">
              <table class="text-center" style="width: 100%">
                <tr>
                  <td>Order ID</td>
                  <td>isVIP</td>
                  <td>Status</td>
                  <td>Count Down</td>
                  <td>Queue</td>
                </tr>

                <tr v-for="(order, index) in completedOrders" :key="order.id">
                  <td>{{ order.id }}</td>
                  <td>{{ order.isVIP ? 'VIP' : 'Normal Member' }}</td>
                  <td>Completed</td>
                  <td>1</td>
                  <td>{{ index + 1 }}</td>
                </tr>
              </table>
            </div>
          </q-card>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
.processing {
  color: blue;
}
</style>
