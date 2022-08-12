<template>
  <q-layout view="lHh Lpr lFf">
    <!-- <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title>
          Quasar App
        </q-toolbar-title>

        <div>Quasar v{{ $q.version }}</div>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Essential Links
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container> -->
    <q-page-container>
      <q-page padding>
        <div class="window-height">
          <div class="meal-wrapper">
            <h5 class="q-ml-md q-mb-md">Select your meal:</h5>
            <div class="row">
              <div class="q-mx-md" v-for="food in foods" :key="food.id">
                <q-btn color="brown" @click="addOrder(food)" class="q-px-md">
                  {{ food.foodName }}
                </q-btn>
              </div>
            </div>
          </div>
          <div class="bot-wrapper q-mt-md">
            <div class="row">
              <div class="col" v-for="bot in bots" :key="bot.id">
                <q-card class="my-card q-mx-md">
                  <q-card-section class="bg-grey-8 text-white">
                    <div class="text-h6">Bot {{ bot.id }}</div>
                    <!-- <div class="text-subtitle2">{{ bot.id }}</div> -->
                  </q-card-section>

                  <q-card-actions vertical align="center">
                    <div>{{ bot.orderId }}</div>
                    <q-badge rounded color="red" label="1" />
                  </q-card-actions>
                </q-card>
              </div>
            </div>
          </div>
          <div class="row q-py-md">
            <div class="col text-center">
              <div>Pending</div>
              <ul class="pending-orders">
                <li
                  v-for="pendingOrder in pendingOrders"
                  :key="pendingOrder.id"
                >
                  {{ pendingOrder.orderName }}
                </li>
              </ul>
            </div>
            <div class="col text-center">
              <div>Completed</div>
              <ul class="completed-orders">
                <li
                  v-for="completedOrder in completedOrders"
                  :key="completedOrder.id"
                >
                  {{ completedOrder.orderName }}
                </li>
              </ul>
            </div>
          </div>
        </div>
      </q-page>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
//import { setTimeout } from 'timers/promises';
import { computed, ref } from 'vue';
//import OrderStatus from '../types/OrderStatus';

// --------- Food Menu ---------
interface IFood {
  id: number;
  foodName: string;
}

const foods: IFood[] = [
  { id: 1, foodName: 'Mc Chicken' },
  { id: 2, foodName: 'Spicy Chicken Mcdeluxe' },
  { id: 3, foodName: 'Ayam goreng spicy' }
];
// --------- End Food Menu ---------

// --------- Orders ---------
interface IOrder {
  id: number;
  orderName: string;
  isVIP: boolean;
  isCompleted: boolean;
  botId: number | null;
}

interface IBot {
  id: number;
  orderId: number | null;
}

const bots: IBot[] = [
  { id: 1, orderId: null },
  { id: 2, orderId: null },
  { id: 3, orderId: null }
];

var orderId = 1;
//const pendingOrders = ref<IOrder[]>([]);
const orders = ref<IOrder[]>([]);

function addOrder(food: IFood) {
  const order: IOrder = {
    id: orderId,
    orderName: food.foodName,
    isVIP: false,
    isCompleted: false,
    botId: null
  };
  orderId++;
  orders.value.push(order);
}

const pendingOrders = computed(() => {
  return orders.value.filter((order: IOrder) => order.isCompleted === false);
});

const completedOrders = computed(() => {
  return orders.value.filter((order: IOrder) => order.isCompleted === true);
});
</script>
