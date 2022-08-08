<template>
  <div class="pending-item">
    <div></div>
    {{ order.id }}
  </div>
</template>

<script>
export default {
  name: "PendingItem",
  props: {
    order: {
      type: Object,
      required: true
    }
  }
};
</script>

<style></style>
