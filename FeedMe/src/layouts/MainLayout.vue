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
      <div class="window-height">
        <div class="row">
          <div class="col text-center" v-for="food in foods" :key="food.id">
            <button @click="addOrder(food)" class="q-px-md">
              {{ food.foodName }}
            </button>
          </div>
        </div>
        <div class="row q-py-md">
          <div class="col text-center">
            <div>Pending</div>
            <ul class="pending-orders">
              <li v-for="pendingOrder in pendingOrders" :key="pendingOrder.id">
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
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
//import { setTimeout } from 'timers/promises';
import { computed, ref } from 'vue';
//import OrderStatus from '../types/OrderStatus';

interface IBot {
  id: number;
  orderId: number;
}

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
  botId: number;
}

var orderId = 1;
//const pendingOrders = ref<IOrder[]>([]);
const orders = ref<IOrder[]>([]);

function addOrder(food: IFood) {
  const order: IOrder = {
    id: orderId,
    orderName: food.foodName,
    isVIP: false,
    isCompleted: false,
    botId: 1
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

// const pendingOrders = computed((orders: IOrder) => {
//   return orders;
// });
</script>
