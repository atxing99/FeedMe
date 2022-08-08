<template>
  <div class="order-wrapper">
    <button @click="addOrder('Spicy Chicken')"></button>
    <div class="order-pending-wrapper">
      <PendingItem
        class="order-pending"
        v-for="meal in order"
        :key="meal.id"
        :order="order"
      />
    </div>
    <div class="order-complete-wrapper">
      <div class="order-pending" v-if="order.isCompleted"></div>
    </div>
  </div>
</template>

<script>
import PendingItem from "./PendingItem.vue";
export default {
  name: "OrderPage",
  components: {
    PendingItem
  },
  data() {
    return {
      order: [
        {
          id: 1,
          orderName: "McChicken",
          isVIP: false,
          isCompleted: false
        }
      ]
    };
  },
  watch: {
    order(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },
  computed: {
    orderID() {
      return `${this.order.id}`;
    }
  },
  methods: {
    addOrder(productName) {
      this.followers++;
    }
  },
  mounted() {
    this.followUser();
  }
};
</script>
