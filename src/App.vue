<template>
  <OrderPage />
</template>

<script>
import OrderPage from "./components/OrderPage.vue";

export default {
  name: "App",
  components: {
    OrderPage
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
    followUser() {
      this.followers++;
    }
  },
  mounted() {
    this.followUser();
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
